# EC2에서 Docker 설치 및 사용

1. EC2 인스턴스 생성

2. EC2 인스턴스 접속

`.pem` 파일을 PuTTYgen 도구로 `.pek` 파일로 만들어서 그 파일을 이용해서 EC2로 만든 서버에 접속할 수 있다.

퍼블릭 DNS(IPv4) 또는 IPv4 퍼블릭 IP 를 통해 접속하고 경고 또는 안내 메세지창이 뜨면 예를 클릭한다.

3. 업데이트를 한다
```
sudo yum -y upgrade
```

4. Docker 설치
```
sudo amazon-linux-extras install -y docker
```

5. 설치를 했으니 실행하고 확인해본다
```
sudo systemctl start docker
ps -ef | grep docker
```

6. ec2-user에게 권한을 준다

```
sudo usermod -aG docker ec2-user
```

적용을 위해서 다시 재접속한다.

`docker ps`를 하면 권한을 받았기 때문에 커맨드가 정상적으로 실행된다.

아래 명령어로 설치된 Docker 버전을 확인할 수 있다.
```
docker version
```


---
#### 참고

https://tech.osci.kr/2020/03/03/91690190/