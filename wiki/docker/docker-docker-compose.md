# Docker, Docker Compose 정리

1. 내가 자주 사용하는 Docker 명령어

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

2. 내가 자주 사용하는 Dcoker Compose 명령어

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

3. 로컬에서 Docker Desktop을 사용하는데 도커 이미지를 빌드 중에 메모리 부족 오류가 발생하면 Docker Desktop의 `Preferences` - `Resources` - `Advanced` 에서 Memory를 늘려주면 해결이된다.

4. [도커에서 bcrypt 설치 오류 발생 해결 방법](https://www.richardkotze.com/top-tips/install-bcrypt-docker-image-exclude-host-node-modules)

5. [Dockerfile instruction](https://rampart81.github.io/post/dockerfile_instructions/)

---
#### 참고

https://www.44bits.io/ko/post/almost-perfect-development-environment-with-docker-and-docker-compose