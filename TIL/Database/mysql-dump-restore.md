# DBeaver에서 DB Dump 하고 Docker MySQL DB에 Restore

DBeaver에서 Dump를 하려면 덤프하려는 데이터베이스를 오른쪽 클릭을 해서 `tool > dump database`를 하면 된다.

client 선택은 `/usr/local/opt/mysql@5.7/5.7.31/bin` 로 하면 된다. client 선택이 올바르게 되어있으면 Next 버튼이 활성화 된다. Output Folder 설정을 한 번 해주면 다음 Dump 에서도동일한 폴더 위치로 설정되어 있다.

Dump 도중에 오류가 발생하여도 중간에 오류가 발생한 지점에서 멈추어서 `.sql` 파일이 생성되어 파일 크기가 무척 작은데 그런 파일들은 삭제하고 다시 Dump 시도하면 된다. 처음에는 MySQL 버전이 달라서 오류가 발생하였고, 그 다음번에는 Security 설정의 Authentication에서 아이디와 비밀번호가 잘못되어서 오류가 발생한 적이 있다.

Docker 에서 MySQL을 설치하였기 때문에 Homebrew에서 `brew install mysql@5.7`을 해서 설치하고 위의 드라이버만 선택해서 DBeaver의 clinet로 사용했다.

```bash
cat dump.sql | docker exec -i container_name /usr/bin/mysql -u root —password=root db_name
```

container_name, db_name 에 사용하려는 도커 컨테이너 이름과 Restore 하려는 데이터베이스 이름을 입력하면 된다. dump.sql 은 sql 파일이 있는 경로의 터미널에서 하는 것이 편한 것 같다.

---
#### 참고

https://dev.to/bhaidar/how-to-dump-a-database-using-dbeaver-56ga

https://elfinlas.github.io/2018/06/09/docker-mariadb-backup/