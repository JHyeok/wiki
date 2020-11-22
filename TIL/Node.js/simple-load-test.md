# 개발환경에서 간단하게 부하 테스트 하는 방법


npm에서 [loadtest](https://github.com/alexfernandez/loadtest) 또는 [autocannon](https://github.com/mcollina/autocannon)을 설치한다.

사용하려는 도구만 설치하면 될 듯한데, 개인적으로 두 개의 도구를 사용해보고 마음에 드는 것을 선택해서 쓰면 될 것같다. 더 많은 요청을 보내는 성능적인 측면에서는 autocannon이 같은 시간 대비 훨씬 더 많은 요청을 보냈었다.

```bash
npm install -g loadtest
```

```bash
npm install -g autocannon
```

여러 도구들이 있었지만 두 개의 도구를 설치해서 직접 사용해보았다. 아래는 결과들이다.

```
loadtest -t 20 -c 10 http://localhost:3000/api/v1/users/

INFO Requests: 0, requests per second: 0, mean latency: 0 ms
INFO Requests: 5886, requests per second: 1178, mean latency: 8.5 ms
INFO Requests: 12107, requests per second: 1245, mean latency: 8 ms
INFO Requests: 16342, requests per second: 846, mean latency: 8.5 ms
INFO
INFO Target URL:          http://localhost:3000/api/v1/users/
INFO Max time (s):        20
INFO Concurrency level:   10
INFO Agent:               none
INFO
INFO Completed requests:  16342
INFO Total errors:        0
INFO Total time:          20.017884772000002 s
INFO Requests per second: 816
INFO Mean latency:        8.3 ms
INFO
INFO Percentage of the requests served within a certain time
INFO   50%      6 ms
INFO   90%      8 ms
INFO   95%      8 ms
INFO   99%      11 ms
INFO  100%      1023 ms (longest request)
 
autocannon -d 20 -c 10 http://localhost:3000/api/v1/users/

Running 20s test @ http://localhost:3000/api/v1/users/
10 connections

┌─────────┬──────┬──────┬───────┬──────┬──────┬──────────┬─────────┐
│ Stat    │ 2.5% │ 50%  │ 97.5% │ 99%  │ Avg  │ Stdev    │ Max     │
├─────────┼──────┼──────┼───────┼──────┼──────┼──────────┼─────────┤
│ Latency │ 3 ms │ 4 ms │ 8 ms  │ 9 ms │ 5 ms │ 18.28 ms │ 1000 ms │
└─────────┴──────┴──────┴───────┴──────┴──────┴──────────┴─────────┘
┌───────────┬────────┬────────┬─────────┬─────────┬─────────┬────────┬────────┐
│ Stat      │ 1%     │ 2.5%   │ 50%     │ 97.5%   │ Avg     │ Stdev  │ Min    │
├───────────┼────────┼────────┼─────────┼─────────┼─────────┼────────┼────────┤
│ Req/Sec   │ 238    │ 238    │ 1958    │ 2155    │ 1819.8  │ 498.44 │ 238    │
├───────────┼────────┼────────┼─────────┼─────────┼─────────┼────────┼────────┤
│ Bytes/Sec │ 234 kB │ 234 kB │ 1.92 MB │ 2.12 MB │ 1.79 MB │ 489 kB │ 233 kB │
└───────────┴────────┴────────┴─────────┴─────────┴─────────┴────────┴────────┘

Req/Bytes counts sampled once per second.

36k requests in 20.02s, 35.7 MB read
```

위처럼 테스트가 가능하다. GET 말고도 아래처럼 POST로도 테스트 할 수 있다.

```
# Load script for GET (simple)
autocannon "localhost:8080/<orm>/orders?simple=true" \
  -m "GET" \
  -c 100 -d 20

# Load script for GET (nested)
autocannon "localhost:8080/<orm>/orders" \
  -m "GET" \
  -c 100 -d 20

# Load script for POST
autocannon "localhost:8080/<orm>/orders" \
  -m "POST" \
  -c 100 -d 20 \
  -i "./data.json" \
  -H "Content-Type: application/json"
```

#### 참고

https://github.com/alexfernandez/loadtest

https://github.com/mcollina/autocannon

https://github.com/emanuelcasco/typescript-orm-benchmark