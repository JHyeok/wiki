# MySQL Slow Query 설정

MySQL `my.cnf` 설정

```
# vi my.cnf
log_slow_querise = /var/log/mysql/mysql-slow.log
long_query_time = 3
```

쿼리가 3초를 초과하는 쿼리에 대한 로그를 남기는 설정이다.
log_slow_querise : 로그의 저장 경로
long_query_time : 쿼리타임이 지정한 시간을 초과하는 부분만 저장

MySQL 재시작

```
# /etc/init.d/mysqld restart
```

**Slow Query 로그 분석**

Query_time : 쿼리 처리시간
Lock_time : lock이 걸린 횟수
Row_sent : 조회 결과 Row 수
Rows_examined : 조회 대상 ROW 수

슬로우 쿼리로 남은 쿼리문들은 EXPLAIN을 사용하여 실행계획을 분석하고 Index를 제대로 타거나 체크하거나, Index를 주거나 하는 방식으로 문제를 해결해야 한다.

**MySQL 쿼리 응답시간 체크**

```
# mysqladmin -i5 status -u root -p
```

Uptime : MySQL server 시삭된 후 현재 시간 (초 단위)\
Threads : 현제 DB 서버에 연결된 유저수\
Questions : 서버 시작후 지금까지 요청된 쿼리수\
Slow queries : mysql 설정파일에 슬로우쿼리의 쿼리시간 이상을 가진 요청수\
Opens : 서버가 시작된 후 현재까지 열렸던 테이블수\
Open tables : 현재 열려 잇는 테이블 수\
Queries per second avg : 평균 초단 쿼리수

---
#### 참고

https://server-talk.tistory.com/34

https://itstudyblog.tistory.com/384
