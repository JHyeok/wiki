# EC2에서 Docker와 Docker Compose 설치

EC2 리눅스 기준

1. EC2 인스턴스 생성

2. EC2 인스턴스 접속

`.pem` 파일을 PuTTYgen 도구로 `.pek` 파일로 만들어서 그 파일을 이용해서 EC2로 만든 서버에 접속할 수 있다.

퍼블릭 DNS(IPv4) 또는 IPv4 퍼블릭 IP 를 통해 접속하고 경고 또는 안내 메시지가 나타나면 예를 클릭한다.

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

7. Git 설치하고 프로젝트 저장소를 클론

```
sudo yum install -y git

git clone [repo 주소]
```

8. Docker Compose 설치

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
```

---
#### 참고

https://www.44bits.io/ko/post/almost-perfect-development-environment-with-docker-and-docker-compose