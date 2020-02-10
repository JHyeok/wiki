#### MySQL 튜닝

```
max_allowed_packet(서버로 질의하거나 받게되는 패킷의 최대 길이를 나타내는 시스템 변수) 

innodb_buffer_pool_size(메모리 용량의 70-80% 정도면 좋다, 해당 파라메터의 크기가 클수록 쿼리 실행시 디스크보다 메모리를 사용하게 되어 빠른 결과를 얻을 수 있다.) 

innodb_log_file_size(ransaction 의 redo log 를 저장하는 로그 파일의 사이즈이며, 256M 이상을 권장한다.)

net_read_timeout(서버가 클라이언트로부터 데이터를 읽어들이는 것을 중단하기까지 대기하는 시간) 

wait_timeout(활동하지 않는 커넥션을 끊을때까지 서버가 대기하는 시간)

max_connection

max_connect_errors
```

---
#### 참고

https://dang-dang12.tistory.com/25

https://jsonobject.tistory.com/408

https://www.lesstif.com/pages/viewpage.action?pageId=30703620