# Docker 멀티 스테이지 빌드

Dockerfile을 만들 때, 멀티 스테이지 빌드는 Docker에 사용되는 이미지의 크기를 줄여서 작은 이미지를 만드려는 목적으로만 사용하는 줄 알았다.

하지만 아래의 Dockerfile을 보고 여러 활용 방안이 있는 것을 알았다. 마치 우리가 CI/CD에서 코드를 통합하고 린트하고 테스트 하는 과정을 Dockerfile내에서 이루어지게 하고 있다. 내가 본 이 글에서는 Jenkins와 같은 도구를 사용해서 CI/CD 파이프라인을 구축하는데 개발자가 코드를 커밋하고 푸시하면 GitHub에 통합되고 CI/CD 파이프라인이 작동해서 새로운 문제를 발견하고 이를 해결하는 작업을 반복하는데 Dockerfile에서 이러한 작업을 실행하도록 해서 초기에 문제를 발견하도록 한다는 것이다.

```dockerfile
# Copies in our code and runs NPM Install
FROM node:latest as builder
WORKDIR /usr/src/app
COPY package* ./
COPY src/ src/
RUN [“npm”, “install”]

# Lints Code
FROM node:latest as linting
WORKDIR /usr/src/app
COPY --from=builder /usr/src/app/ .
RUN [“npm”, “lint”]

# Gets Sonarqube Scanner from Dockerhub and runs it
FROM newmitch/sonar-scanner:latest as sonarqube
COPY --from=builder /usr/src/app/src /root/src

# Runs Unit Tests
FROM node:latest as unit-tests
WORKDIR /usr/src/app
COPY --from=builder /usr/src/app/ .
RUN [“npm”, “test”]

# Runs Accessibility Tests
FROM node:latest as access-tests
WORKDIR /usr/src/app
COPY --from=builder /usr/src/app/ .
RUN [“npm”, “access-tests”]

# Starts and Serves Web Page
FROM node:latest as serve
WORKDIR /usr/src/app
COPY --from=builder /usr/src/app/dest ./
COPY --from=builder /usr/src/app/package* ./
RUN [“npm”, “start”]
```

멀티 스테이지 빌드를 사용하면 별도의 Dockerfile이 필요한 빌드, 테스트 및 런타임 환경을 분리 할 수 ​​있다. 다양한 계층이 더 이상 최종 컨테이너에 저장되지 않기 때문에 배포하는 최종 Docker 컨테이너의 실제 크기를 최소화 할 수 있다.

이렇게하면 단계와 사용 사례에 따라 컨테이너를 절반으로 또는 2/3까지 줄일 수 있다. 빌드 프로세스의 비 효율성을 쉽게 강조하고 Dockerfile/기본 이미지 만 업데이트해야하므로 통합 최적화가 가능합니다. 이를 통해 명령을 표준화하고, 과도한 혼동을 방지하고, 코드 작성 과정에서 남은 정보를 이동할 수 있다.

다단계 빌드의 또 다른 추가 이점은 단계들을 병렬로 실행할 수 있다는 것이다. 테스트 프레임 워크 및 레포지토리가 커지고 통합이 커지고 요구 사항은 모든 단계를 동시에 실행하고 CI/CD 도구를 사용하거나 로컬에서 빌드 시간을 줄이는 기능을 변경합니다. 

다단계 빌드는 개발팀의 CI/CD 파이프 라인을 많이 단순화하고 개발자가 프로덕션 배포로가는 길에 다양한 예상 게이트와 상호 작용할 수있는 쉬운 방법을 제공한다. 이를 통해 애플리케이션 전반의 일관성이 크게 향상되었으며 개발자와 감사 자에게 결과를 얻을 수있는 통합 된 방법을 제시한다.

항상 장점만 있는 것은 아니다, 빌드 및 실행 환경에서 일관성을 제공하지만 빌드 및 실행 단계 구성, Dockerfile의 물리적 크기 및 논리적 구성 증가, 단계간에 복사 된 파일 관리와 관련된 문제가 있을 수 있다.

MSDN에서는 `base`, `build`, `publish`, `final`로 단계를 나누어서 멀티 스테이지 빌드의 Dockerfile를 만드는 방법을 추천하고 있다.

---
#### 참고

https://medium.com/capital-one-tech/multi-stage-builds-and-dockerfile-b5866d9e2f84

https://docs.microsoft.com/ko-kr/visualstudio/containers/container-build?view=vs-2019

https://blog.bitsrc.io/a-guide-to-docker-multi-stage-builds-206e8f31aeb8