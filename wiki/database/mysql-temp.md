# MySQL

```sql
# Lock 관련 쿼리
use information_schema;

select * from INNODB_LOCKS;
select * from INNODB_LOCK_WAITS;
select * from INNODB_TRX;
```

```sql
# test 유저 만들기 예시
CREATE USER 'test'@'localhost' IDENTIFIED BY 'test';
CREATE USER 'test'@'%' IDENTIFIED BY 'test';
grant all privileges on *.* to 'test'@'localhost';
grant all privileges on *.* to 'test'@'%';
```

---
#### 참고

https://www.popit.kr/mysql-lock-%EC%83%81%ED%99%A9-%EB%AC%B8%EC%A0%9C-%ED%95%B4%EA%B2%B0/

https://myinfrabox.tistory.com/188
