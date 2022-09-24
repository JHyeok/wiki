# k6과 loadtest 비교

부하 테스트로 [k6](https://github.com/grafana/k6), [loadtest](https://github.com/alexfernandez/loadtest)를 최근에 사용했다.

Mac에서 사용한다면 k6은 `brew install k6`로 설치를 해서 사용할 수 있다.

loadtest는 npm에서 설치해서 사용할 수 있다.

두 가지를 사용해보고 느낀 점을 간단하게 정리한다.

#### k6

- POST 요청을 테스트할 때 Request Body는 `JSON.stringify()` 메서드로 JSON 문자열 형태로 요청해야 한다.
- 내 Mac에서는 동시 사용자(vus)가 130이 마지노선이었다. 그 이상으로 하면 요청 오류가 발생한다.
- 테스트 결과가 loadtest 보다 직관적이지 못한 느낌은 들었다.
- TS로 작성을 하려면 조금 귀찮은 작업이 있어서 JS로 작성하는 게 편한 것 같다.

```js
import http from 'k6/http';

export const options = {
  discardResponseBodies: true,
  vus: 130,
  duration: '5s',
};

export default function () {
  const url = 'http://localhost:8000/v1/test';
  const data = JSON.stringify({
    // ...
  });
  const params = {
    // ...
  };

  const res = http.post(url, data, params);
}
```

#### loadtest

- ts-node로 실행을 하면 TS로 작성할 수 있다.
- maxRequests 옵션으로 테스트를 한 결과와 maxSeconds 옵션으로 테스트한 결과의 포맷이 다르다. (maxSeconds는 result를 따로 출력해야 하는 불편함이 있다.)
- maxRequests로 테스트한 결과는 k6보다 직관적이었지만 maxSeconds로 테스트를 하면 나오는 결과의 포맷은 k6보다 더 좋지 않았다.

maxRequests로 테스트한 결과의 일부
```sh
[Tue Sep 06 2022 16:41:02 GMT+0900 (Korean Standard Time)] INFO Percentage of the requests served within a certain time
[Tue Sep 06 2022 16:41:02 GMT+0900 (Korean Standard Time)] INFO   50%      44 ms
[Tue Sep 06 2022 16:41:02 GMT+0900 (Korean Standard Time)] INFO   90%      138 ms
[Tue Sep 06 2022 16:41:02 GMT+0900 (Korean Standard Time)] INFO   95%      180 ms
[Tue Sep 06 2022 16:41:02 GMT+0900 (Korean Standard Time)] INFO   99%      206 ms
[Tue Sep 06 2022 16:41:02 GMT+0900 (Korean Standard Time)] INFO  100%      206 ms (longest request)
```
maxSeconds로 테스트한 결과
```js
{
  totalRequests: 16263,
  totalErrors: 0,
  totalTimeSeconds: 20.031381528,
  rps: 812,
  meanLatencyMs: 35.7,
  maxLatencyMs: 685,
  minLatencyMs: 0,
  percentiles: { '50': 27, '90': 39, '95': 51, '99': 121 },
  errorCodes: {},
  instanceIndex: 0
}
```

```ts
/* eslint-disable no-console */
import loadtest, { LoadTestOptions } from 'loadtest';

const options: LoadTestOptions = {
  url: 'http://localhost:8000/v1/test',
  method: 'POST',
  body:
    // ...
  },
  maxRequests: 1000,
  concurrency: 50,
  headers: {
    // ...
  },
  contentType: 'application/json',
  quiet: false,
};

loadtest.loadTest(options, function (error, result) {
  console.log('LoadTest!');
  if (error) {
    return console.error('Got an error: %s', error);
  }
  console.log(result);
});
```

#### 결론

k6 코드를 TS로 작성하기에는 불편해서 JS로 작성을 해야 했지만 성능적인 면에서 k6이 loadtest보다 훨씬 만족스러웠다.
laodtest는 동시성을 130으로 했을 때 12만 개의 요청을 날리고 처리하는데 246초가 걸렸는데 k6은 동시성 120에 20초 만에 끝났다.
개인적으로 k6이 만족스러웠다.