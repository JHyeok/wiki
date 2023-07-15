# esModuleInterop

tsconfig.json에서 해당 옵션을 설정할 수 있다.

esModuleInterop의 기본값은 false인데, lodash에서는 CommonJS 스펙의 require를 사용한다.

```ts
// ERROR 발생
import _ from 'lodash';

// ERROR 발생하지 않음
import * as _ from 'lodash';
```

따라서 위 코드와 같이 CommonJS 모듈을 ES6 모듈 코드베이스로 가져오려고 할 때 문제가 발생한다.

esModuleInterop 옵션을 true로 하면 ES6 모듈 사양을 준수하여 CommonJS 모듈을 가져올 수 있게 된다.

---
#### 함께 읽으면 좋은 글

[esModuleInterop 속성을 이용한 Import 에러 해결](https://pewww.tistory.com/26)

[Understanding esModuleInterop in tsconfig file](https://stackoverflow.com/questions/56238356/understanding-esmoduleinterop-in-tsconfig-file)

[ES Module Interop -
esModuleInterop](https://www.typescriptlang.org/tsconfig#esModuleInterop)

[How Does esModuleInterop Affect TSC?](https://www.alibabacloud.com/blog/how-does-esmoduleinterop-affect-tsc_599767)