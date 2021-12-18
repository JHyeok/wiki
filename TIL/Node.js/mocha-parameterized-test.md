# Mocha 파라미터화 테스트

Jest에서 `test.each()`, `describe.each()`를 사용해서 parameterized 테스트를 할 수 있다.
만약 Mocha에서 하려면 아래와 같이하면 된다.

```typescript
describe('isStarted() 테스트', () => {
    const isStartedTests = [
      { args: ['READY', 'DONE'], expected: true },
      { args: ['READY', 'COMPLETE'], expected: true },
      { args: ['START', 'DONE'], expected: true },
      { args: ['START', 'COMPLETE'], expected: true },
      { args: ['', 'DONE'], expected: false },
      { args: ['START', ''], expected: false },
    ];

    isStartedTests.forEach(({ args, expected }) => {
      it(`args(${args[0]}, ${args[1]}) returns ${expected} `, () => {
        const result = service.isStarted(args[0], args[1]);

        result.should.be.deepEqual(expected);
      });
    });
  });
```

---
#### 참고

파라미터화 테스트란: https://www.daleseo.com/jest-each
Mocha: https://mochajs.org/#dynamically-generating-tests