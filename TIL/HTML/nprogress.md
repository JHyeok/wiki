# NProgress 로딩바 설정 관련 및 Ajax 전역 이벤트 적용

```javascript
NProgress.configure({ showSpinner: false });
NProgress.start();
var interval = setInterval(function () { NProgress.inc(); }, 1000);

$(window).on('load', function () {
    clearInterval(interval);
    NProgress.done();
});

$(window).on('unload', function () {
    NProgress.start();
});
```

또는 ajax 전역 적용

```javascript
NProgress.configure({ showSpinner: false });

$(document).ajaxStart(function () {
    NProgress.start();
});

$(document).ajaxSend(function () {
    NProgress.inc();
});

$(document).ajaxComplete(function () {
    NProgress.done();
});

$(document).ajaxStop(function () {
    NProgress.done();
});

$(document).ajaxError(function () {
    NProgress.done();
});
```

적용을 하였는데 혹시라도 로딩바가 나오지 않는다면 `z-index`를 확인해서 높은 숫자로 바꾸면 해결된다.

### onbeforeunload 를 활용하여 페이지 이동 시 프로그레스바 생성

window.onbeforeunload 이벤트

onunload 보다 먼저 발생한다.
새로고침, 페이지 이동 등 페이지를 벗어나기 전에 무조건 실행된다.
문자열을 return 하면 confirm 창이 생성되고 아무것도 return 하지 않으면 아무일도 일어나지 않는다.

```javascript
window.onbeforeunload = function(e){
    if(e != null && e != undefined){
        NProgress.start();
    }
};
```

---
#### 참고

https://api.jquery.com/category/ajax/global-ajax-event-handlers/

http://www.nextree.co.kr/p10308/

https://cofs.tistory.com/307