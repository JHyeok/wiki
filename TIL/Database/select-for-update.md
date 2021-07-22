# SELECT FOR UPDATE

MySQL에서 Update 처리 시에 같은 시각에 중복된 요청이 들어올 때, 문제가 발생할 수 있다. 예를 들어 결제 요청이 같은 시각에 여러 요청이 들어와서 지갑의 잔액이 마이너스 금액이 될 수 있다.

이것을 방지하기 위한 방법으로는 여러 가지가 있는데, 가장 쉬운 SELECT FOR UPDATE에 대해서 알아본다.

가정 먼저 LOCK을 획득한 SESSION의 SELECT 된 ROW들이 UPDATE 쿼리 후 COMMIT 되기 이전까지 다른 SESSION들은 해당 ROW들을 수정하지 못한다.

데이터 수정하려고 SELECT 하는 중이야~ 다른 사람들은 데이터에 손대지 마!"라고 할 수 있다. 좀 더 딱딱한 표현으로는 동시성 제어를 위하여 특정 데이터(ROW)에 대해 배타적 LOCK을 거는 기능이다. 

Sequelize에서는 아래처럼 사용할 수 있다.

```typescript
ArticleModel.findByPk(id, {
  include: [
	//…
  ],
  lock: Transaction.LOCK.UPDATE,
})
```

```sql
SELECT * FROM article WHERE id=1 FOR UPDATE;
```

SELECT FOR UPDATE를 사용하지 않고, Redis를 앞단에 두어서 동시 요청을 막을 수도 있다.

#### 그림 설명
https://dololak.tistory.com/446 

---
#### 참고

https://benant.net/programming-note/2020/03/04/mysql-%EC%A4%91%EB%B3%B5-%EC%B2%98%EB%A6%AC-%EB%B0%A9%EC%A7%80%ED%95%98%EA%B8%B0-for-update/