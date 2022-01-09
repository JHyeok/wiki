# ASP.NET Core MVC에서 인메모리 캐시 사용

IMemoryCache를 사용해야 하는데 ASP.NET Core에서는 IMemoryCache가 종속성 주입을 기본적으로 사용한다.

그렇기 때문에 사용하려는 컨트롤러의 생성자에서 요청만 하면 된다.

```csharp
public class HomeController : Controller
{
    private IMemoryCache _cache;

    public HomeController(IMemoryCache memoryCache)
    {
        _cache = memoryCache;
    }
}
```

결과를 필터링하고 필터링된 결과를 엑셀로 다운로드받는 부분을 인메모리 캐시와 같이 구현해보았다.

먼저 Startup.cs에서 `services.AddDistributedMemoryCache();`를 컨테이너 서비스에 추가해야 한다.
분산 메모리 캐시를 사용한다는 선언이다.

```csharp
/// <summary>
/// 필터링된 결과를 엑셀파일로 만들어서 캐시에 넣어놓는다
/// </summary>
/// <param name="themeSearchDto"></param>
/// <returns></returns>
[HttpGet]
public async Task<IActionResult> Excel(SampleSearchDto sampleSearchDto)
{
    string handle = Guid.NewGuid().ToString();

    using (var memoryStream = await _sampleService.GetSampleListConvertExcelAsync(sampleSearchDto))
    {
        _cache.Set(handle, memoryStream.ToArray(),
        new MemoryCacheEntryOptions().SetSlidingExpiration(TimeSpan.FromMinutes(10)));
    }

    return Json(new { fileGuid = handle, fileName = "ReportOutput.xlsx" });
}

/// <summary>
/// 캐시에 넣어놓은 스트림을 꺼내서 파일 다운로드를 사용자에게 제공한다
/// </summary>
/// <param name="fileGuid"></param>
/// <param name="fileName"></param>
/// <returns></returns>
[HttpGet]
public virtual ActionResult Download(string fileGuid, string fileName)
{
    if (_cache.Get<byte[]>(fileGuid) != null)
    {
        byte[] data = _cache.Get<byte[]>(fileGuid);
        _cache.Remove(fileGuid);
        return File(data, "application/vnd.ms-excel", fileName);
    }
    else
    {
        return View("Error");
    }
}
```

그리고 위처럼 엔드포인트를 두개 구현해주면 된다.

Excel은 DB에서 결과를 필터링하고 받은 목록을 엑셀 파일의 메모리 스트림으로 변환해서 캐시에 저장하는 부분이고,
Download는 캐시에서 메모리 스트림을 꺼내서 사용자에게 파일 다운로드를 제공하는 부분이다.
ajax를 사용하는 클라이언트에서는 아래처럼 사용을 하면 되겠다.

```javascript
var exportExcel = function () {
    var searchData = getSearchData();
    $.ajax({
        cache: false,
        type: 'GET',
        url: '/Sample/Excel',
        data: searchData
    }).done(function (result) {
        window.location = '/Sample/Download?fileGuid=' + result.fileGuid
            + '&filename=' + result.fileName;
    });
}
```

먼저 `/Sample/Excel`로 요청해서 파일을 인메모리 캐시에 저장하고 완료가 되면 `/Sample/Download`로 파일Guid와 파일 이름을 같이 넘겨서
다운로드를 받도록 한다.

---
#### 참고

https://stackoverflow.com/questions/16670209/download-excel-file-via-ajax-mvc