#### map/forEach 에서 async/await를 병렬로 사용

먼저, for..of는 iterable 객체를 모두 순회할 수 있는 반복문이다.
for..in은 Object의 Key를 순회하기 위한 반복문이다. forEach를 Array를 순회하는데 사용된다.
for..of를 사용할 수 있는 객체를 이터러블이라고 부릅니다.


기존에는 for..of로 async/await을 순차적으로 처리하였다.
일반적으로는 Promise.all로 병렬로 처리가 가능하였지만, array 구조를 가진 객체를 병렬로 처리하기 위해서는 `.map`과 `Promise.all()`을 적절히 이용하면 된다. (map의 concurrency)
만약, bluebird를 사용할 수 있는 환경이라면 bluebird를 사용하는 것도 좋은 방법이다.


```ts
  for (const obj of testObj) {
    for (const item of obj.subObj) {
      if (item.isStatus === false) {
        item.result = await TestService.testFunction(item.id, item.name);
      }
    }
  }
```

for..of는 직렬로 처리되어 매우 비효율적이다. (순차적으로 처리가 필요하다면 for..of를 사용해야 한다.)
위의 비효율적인 부분들을 아래처럼 병렬 처리로 바꿀 수 있다.

```ts
  await Promise.all(
    testObj.map(async obj => {
      await Promise.all(
        obj.subObj.map(async iem => {
          if (item.isStatus === false) {
            item.result = await TestService.testFunction(item.id, item.name);
          }
        }),
      );
    }),
  );
```

위의 예제는 map이 중첩되었지만, 아래는 map이 한 번만 있을 경우이다.

```ts
  for (const item of obj) {
    const testFlag = await isCheck(item.id, item.name);
    if (testFlag === true) {
      result = item.name;
    }
  }
```

```ts
  await Promise.all(
    obj.map(async item => {
      if (await isCheck(item.id, item.name)) {
        result.push(item.name);
      }
    }),
  );
```
