# Redis를 단위 테스트에서 사용(Mocha, Sinon)

Redis를 테스트 환경에서 모의로 사용하도록하는 .ts 파일 생성, 예를 들어 `setup-redis.ts`를 만들었다. 

```typescript
import sinon from 'sinon';
import Redis from 'ioredis';

sinon.stub(Redis.prototype, 'connect').returns(Promise.resolve());
```

Root 디렉토리에 `.mocharc.json`을 만들어서 Mocha 환경에서 가장 먼저 테스트 환경을 구성할 때 `setup-redis.ts` 파일을 공통으로 사용하도록 한다. 일일이 Redis를 사용하는 단위 테스트에서 Redis를 모의하는 구문을 중복으로 추가하지 않아도 된다.

```json
{
  "file": ["./test/setup-redis.ts"]
}
```