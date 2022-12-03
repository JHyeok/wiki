# DB Dump

```bash
docker exec mysql /usr/bin/mysqldump -u root -p'password' -h <ip address> <db name> 1> dump.sql
```

DBeaver에서 Dump를 할 때 선택하는 client에는 로컬에 설치된 MySQL에만 접근이 가능해서 Dokcer에 설치된 clinet에 접근이 힘들 때 그냥 터미널에서 위의 방법으로 하면 된다.

#### DBeaver에서 DB Dump

DBeaver에서 Dump를 하려면 Dump 하려는 데이터베이스를 오른쪽 클릭을 해서 `tool > dump database`를 하면 된다.

client 선택은 `/usr/local/opt/mysql@5.7/5.7.31/bin` 로 하면 된다. client 선택이 올바르게 되어있으면 Next 버튼이 활성화된다. Output Folder 설정을 한 번 해주면 다음 Dump에서도 동일한 폴더 위치로 설정되어 있다.

Dump 도중에 오류가 발생하여도 중간에 오류가 발생한 지점에서 멈추어서 `.sql` 파일이 생성되어 파일 크기가 무척 작은데 그런 파일들은 삭제하고 다시 Dump 시도하면 된다. 처음에는 MySQL 버전이 달라서 오류가 발생하였고, 그 다음번에는 Security 설정의 Authentication에서 아이디와 비밀번호가 잘못되어서 오류가 발생한 적이 있다.

Docker에서 MySQL을 설치하였기 때문에 Homebrew에서 `brew install mysql@5.7`을 해서 설치하고 위의 드라이버만 선택해서 DBeaver의 clinet로 사용했다.

# Docker MySQL에서 DB Restore

```bash
cat dump.sql | docker exec -i container_name /usr/bin/mysql -u root —password=root db_name
```

container_name, db_name에 사용하려는 도커 컨테이너 이름과 Restore 하려는 데이터베이스 이름을 입력하면 된다. dump.sql 은 sql 파일이 있는 경로의 터미널에서 하는 것이 편한 것 같다. 그리고 Restore 하기 전에 db_name의 데이터베이스를 만들어놓아야 한다.

```
Warning: Using a password on the command line interface can be insecure.
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
```

위의 오류 메시지가 발생하면서 Restore가 안될 때가 있다.

```bash
cat dump.sql | docker exec -i mysql /usr/bin/mysql -u root -p"rootpassword" db_name
```

위의 명령어로 하면 Restore 된다.

---

#### 참고

https://dev.to/bhaidar/how-to-dump-a-database-using-dbeaver-56ga

https://elfinlas.github.io/2018/06/09/docker-mariadb-backup/

https://stackoverflow.com/questions/43008167/mysql-login-warning-using-a-password-on-the-command-line-interface-can-be-inse
