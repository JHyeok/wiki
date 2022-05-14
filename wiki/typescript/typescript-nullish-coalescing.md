# TypeScript Nullish Coalescing

Typescript 3.7 이상의 버전에서 Nullish coalescing operator를 사용할 수 있다.
`??`은 왼쪽 피연산자가 `null` 또는 `undefined`일 때, 오른쪽 피연산자를 반환하고, 그렇지 않으면 왼쪽 피연산자를 반환한다.
기존에 `||`를 자주 사용했다면 `??`가 `||`와 어떤 점이 다른지는 아래의 코드를 보면 알 수 있다.

```ts
const response = {
  foo: null,
  bar: '',
  baz: 0,
  dgx: false
};

const undefinedValue = response.undefinedValue || 'Hello, world!'; // 결과: 'Hello, world!'
const nullValue = response.foo || 'Hello, world!'; // 결과: 'Hello, world!'
const emptyValue = response.bar || 'Hello, world!'; // 결과: 'Hello, world!'
const zeroValue = response.baz || 50; // 결과: 50
const falseValue = response.dgx || true; // 결과: true
```

```ts
const response = {
  foo: null,
  bar: '',
  baz: 0,
  dgx: false
};

const undefinedValue = response.undefinedValue ?? 'Hello, world!'; // 결과: 'Hello, world!'
const nullValue = response.foo ?? 'Hello, world!'; // 결과: 'Hello, world!'
const emptyValue = response.bar ?? 'Hello, world!'; // 결과: ''
const zeroValue = response.baz ?? 50; // 결과: 0
const falseValue = response.dgx ?? true; // 결과: false
```

실제 개발할 때 `''`를 `null`로 반환하거나 `null`이나 `undefined`일 때는 `||`와 `??`의 동작이 같아서 크게 문제를 느끼지 않을 수도 있다.
직접 개발하면서 코드에서 겪은 문제는 boolean 값을 사용하면서 원하지 않는 값을 반환할 때이다.

---
#### 참고

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator
