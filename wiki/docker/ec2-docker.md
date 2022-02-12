# EC2에서 Docker 설치 및 사용법과 DockerFile 및 Docker-Compose

#### Docker 설치 및 사용

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

#### DockerFile, docker-compose.yml 작성

1. Git 설치하고 프로젝트 저장소를 클론

```
sudo yum install -y git

git clone [repo 주소]
```

2. docker-compose 설치

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
```

3. 내가 자주 사용하는 docker 명령어들

```
# Dockerfile Image build
docker build -t jaebook-server:0.1 .
--no-cache : 캐시 비활성화

# 모든 도커 컨테이너 삭제
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)

# 모든 도커 이미지 삭제
docker rmi $(docker images -q)

# 사용하지 않는 도커 볼륨 삭제
docker volume prune
```

볼륨을 제거하려면 컨테이너가 제거되어 있어야 한다.

Docker를 사용하다 보면 `<none>:<none>`의 이미지들이 쌓이기 시작한다. 이러한 이미지들을 정리하려면 아래의 명령어를 입력하면 된다.

```
docker rmi $(docker images -f "dangling=true" -q)
```

3. 내가 자주 사용하는 docker-compose 명령어들

```
# 알아서 컨테이너를 재생성하고 재시작해준다 (docker-compose 파일 수정되었을 때 사용)
docker-compose up -d 

# --build 옵션을 넣으면 알아서 이미지를 새로 만들고 서비스를 재시작 (Dockerfile 수정되었을 때 사용)
docker-compose up -d --build

# 로그 확인
docker-compose logs

# docker-compose로 설치한 volume 삭제
docker-compose down -v
```

4. [도커에서 bcrypt 설치 오류 발생 해결 방법](https://www.richardkotze.com/top-tips/install-bcrypt-docker-image-exclude-host-node-modules)

5. [자주 쓰는 DockerFile 명령어](https://rampart81.github.io/post/dockerfile_instructions/)

6. 로컬에서 Docker Desktop을 사용하는데 도커를 사용하다가 메모리 부족 오류가 발생하면 Docker Desktop의 `Preferences` - `Resources` - `Advanced` 에서 Memory를 늘려주면 해결이된다.

---
#### 참고

https://tech.osci.kr/2020/03/03/91690190/

https://www.44bits.io/ko/post/almost-perfect-development-environment-with-docker-and-docker-compose