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
INFO Percentage of the requests served within a certain time
INFO   50%      44 ms
INFO   90%      138 ms
INFO   95%      180 ms
INFO   99%      206 ms
INFO  100%      206 ms (longest request)
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

#### 성능

laodtest는 동시성을 130으로 했을 때 12만 개의 요청을 날리고 처리하는데 246초가 걸렸는데 k6은 동시성 120에 20초 만에 끝났다.

**k6**
```sh
running (20.0s), 000/120 VUs, 125890 complete and 0 interrupted iterations
default ✓ [======================================] 120 VUs  20s

     data_received..................: 93 MB  4.6 MB/s
     data_sent......................: 67 MB  3.3 MB/s
     http_req_blocked...............: avg=3.85µs  min=0s     med=1µs     max=4.89ms  p(90)=2µs     p(95)=2µs    
     http_req_connecting............: avg=2.27µs  min=0s     med=0s      max=2.85ms  p(90)=0s      p(95)=0s     
     http_req_duration..............: avg=19.02ms min=9.08ms med=18.44ms max=47.71ms p(90)=21.25ms p(95)=22.3ms 
       { expected_response:true }...: avg=19.02ms min=9.08ms med=18.44ms max=47.71ms p(90)=21.25ms p(95)=22.3ms 
     http_req_failed................: 0.00%  ✓ 0          ✗ 125890
     http_req_receiving.............: avg=17.36µs min=10µs   med=15µs    max=6.26ms  p(90)=21µs    p(95)=27µs   
     http_req_sending...............: avg=8.84µs  min=4µs    med=8µs     max=5.15ms  p(90)=10µs    p(95)=11µs   
     http_req_tls_handshaking.......: avg=0s      min=0s     med=0s      max=0s      p(90)=0s      p(95)=0s     
     http_req_waiting...............: avg=18.99ms min=8.92ms med=18.42ms max=47.67ms p(90)=21.22ms p(95)=22.27ms
     http_reqs......................: 125890 6290.66804/s
     iteration_duration.............: avg=19.06ms min=9.83ms med=18.48ms max=50.35ms p(90)=21.29ms p(95)=22.33ms
     iterations.....................: 125890 6290.66804/s
     vus............................: 120    min=120      max=120 
     vus_max........................: 120    min=120      max=120 
```

**loadtest**
```sh
INFO Target URL:          http://localhost:8000/v1/test
INFO Max requests:        120000
INFO Concurrency level:   120
INFO Agent:               none
INFO 
INFO Completed requests:  120000
INFO Total errors:        840
INFO Total time:          246.9031754 s
INFO Requests per second: 486
INFO Mean latency:        246.8 ms
INFO 
INFO Percentage of the requests served within a certain time
INFO   50%      28 ms
INFO   90%      53 ms
INFO   95%      84 ms
INFO   99%      3576 ms
INFO  100%      26689 ms (longest request)
INFO 
INFO  100%      26689 ms (longest request)
INFO 
INFO    -1:   840 errors
```

#### 결론

k6 코드를 TS로 작성하기에는 불편해서 JS로 작성을 해야 했지만 성능적인 면에서 k6이 loadtest보다 훨씬 만족스러웠다.