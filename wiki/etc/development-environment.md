# > Visual Studio Code 환경

### Visual Studio Code 확장 프로그램 관리

```bash
# Windows (PowerShell)
code --list-extensions | % { "code --install-extension $_" }

# Unix 계열
code --list-extensions | xargs -L 1 echo code --install-extension
```

```
code --install-extension charasyn.chc-dark-color-theme
code --install-extension dbaeumer.vscode-eslint
code --install-extension esbenp.prettier-vscode
code --install-extension golang.go
code --install-extension hbenl.vscode-mocha-test-adapter
code --install-extension hbenl.vscode-test-explorer
code --install-extension kavod-io.vscode-jest-test-adapter
code --install-extension mhutchie.git-graph
code --install-extension ms-azuretools.vscode-docker
code --install-extension MS-CEINTL.vscode-language-pack-ko
code --install-extension ms-vscode-remote.remote-wsl
code --install-extension ms-vscode.test-adapter-converter
code --install-extension shd101wyy.markdown-preview-enhanced
```

```
code --install-extension WSL: Ubuntu-22.04에 설치된 확장:
code --install-extension dbaeumer.vscode-eslint
code --install-extension esbenp.prettier-vscode
code --install-extension golang.go
code --install-extension hbenl.vscode-mocha-test-adapter
code --install-extension hbenl.vscode-test-explorer
code --install-extension kavod-io.vscode-jest-test-adapter
code --install-extension mhutchie.git-graph
code --install-extension ms-azuretools.vscode-docker
code --install-extension MS-CEINTL.vscode-language-pack-ko
code --install-extension ms-vscode.test-adapter-converter
code --install-extension shd101wyy.markdown-preview-enhanced
```

### Visual Studio Code Settings

`settings.json`

#### Windows

```json
{
  "files.watcherExclude": {
    "**/node_modules": true,
    "**/platforms": true,
    "**/plugins": true
  },
  "eslint.alwaysShowStatus": true,
  "explorer.confirmDelete": false,
  "typescript.implementationsCodeLens.enabled": true,
  "typescript.referencesCodeLens.enabled": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "terminal.integrated.fontFamily": "'D2Coding ligature'",
  "javascript.updateImportsOnFileMove.enabled": "always",
  "typescript.updateImportsOnFileMove.enabled": "always",
  "[go]": {
    "editor.defaultFormatter": "golang.go",
    "editor.formatOnSave": true
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true
  },
  "editor.renderWhitespace": "boundary",
  "editor.inlineSuggest.enabled": true,
  "workbench.colorCustomizations": {
    "editor.lineHighlightBackground": "#1073cf2d",
    "editor.lineHighlightBorder": "#9fced11f"
  },
  "workbench.colorTheme": "CHC Dark"
}
```

> Windows 환경에서 `Ctrl + Shift + P` 단축키로 `settings.json` 수정 가능하다.

#### Mac

```json
{
  "terminal.integrated.fontFamily": "D2Coding",
  "files.watcherExclude": {
    "**/node_modules": true,
    "**/platforms": true,
    "**/plugins": true
  },
  "eslint.alwaysShowStatus": true,
  "explorer.confirmDelete": false,
  "editor.tabSize": 2,
  "typescript.implementationsCodeLens.enabled": true,
  "typescript.referencesCodeLens.enabled": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "typescript.updateImportsOnFileMove.enabled": "always",
  "javascript.updateImportsOnFileMove.enabled": "always",
  "editor.fontSize": 16,
  "[markdown]": {
    "editor.rulers": [50, 72],
    "editor.fontFamily": "NanumGothicCoding"
  },
  "[git-commit]": {
    "editor.rulers": [50, 72],
    "editor.fontFamily": "NanumGothicCoding"
  }
}
```

# > 맥린이 맥북 적응하기

### 맥OS에서 NVM으로 Node 설치

```
sudo curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash

nvm install v12
```

- 참고 : https://gist.github.com/falsy/8aa42ae311a9adb50e2ca7d8702c9af1

### zsh에서 Node 명령어 오류가 발생할 때

```
vi ~/.zshrc
```

```
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

- 참고 : https://likejirak.tistory.com/m/224

### 프로젝트 별로 노드 버전 변경하기 (zsh + .nvmrc)

```
vi ~/.zshrc
```

```
# Load nvmrc
load-nvmrc() {
  if [[ -f .nvmrc && -r .nvmrc ]]; then
    nvm use
  elif [[ $(nvm version) != $(nvm version default)  ]]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc
```

- 참고 : https://hyeok999.github.io/2020/06/02/NVM/

### 맥북 환경설정 순서

1. 맥OS의 키보드와 마우스, 스크롤 환경 설정을 해야 한다.
2. 키보드에서는 키 반복 빠르게, 지연시간 짧게 설정
3. 맥OS에서 fn키 누르지 않고 F1 ~ F12 사용하도록 설정
4. 단어 자동완성, 맞춤법 해제하기
5. 맥북 한영을 윈도우처럼 오른쪽 커맨드로 변경 (Karabiner-Elements)
6. 터치패드 설정 변경
7. 맥 파인더 경로 막대 설정

#### 맥북 키 반복 입력으로 대체하기

기존에는 영어 키를 꾹 누르고 있으면 액센트 표시가 있는 문자 입력인데, 이것을 Windows 환경에서 처럼 키 반복 입력으로 변경 가능하다. 설정한 이후에 해당 응용프로그램만 재시작하면 적용된다.

```
# 키 반복 입력
defaults write -g ApplePressAndHoldEnabled -bool false
# 기존의 액센트 표시가 있는 문자 입력
defaults delete -g ApplePressAndHoldEnabled
```

### Docker Compose - MySQL

```yml
version: "3" # 파일 규격 버전
services: # 이 항목 밑에 실행하려는 컨테이너 들을 정의
  db: # 서비스 명
    image: mysql:5.7 # 사용할 이미지
    container_name: mysql # 컨테이너 이름 설정
    ports:
      - "3306:3306" # 접근 포트 설정 (컨테이너 외부:컨테이너 내부)
    environment: # -e 옵션
      MYSQL_ROOT_PASSWORD: "root1234" # MYSQL 패스워드 설정 옵션
    command: # 명령어 실행
      - --character-set-server=utf8mb4
      - --collation-server=utf8mb4_unicode_ci
      - --sql-mode=STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION
    volumes:
      - /Users/{userName}/datadir:/var/lib/mysql # -v 옵션 (다렉토리 마운트 설정)
    restart: always
```

> {userName} 에는 컴퓨터 사용자 이름이 들어간다.

### Docker Compose - Redis

```yml
version: "3"
services:
  # Redis Server
  redis:
    image: redis:alpine
    container_name: redis
    ports:
      - "6379:6379"
    entrypoint: redis-server --appendonly yes
    restart: always
  # Redis Web-UI
  redis-commander:
    image: rediscommander/redis-commander:latest
    container_name: redis-commander
    ports:
      - "8081:8081"
    environment:
      - REDIS_HOSTS=local:redis:6379
    volumes:
      - /Users/{userName}/datadir:/var/lib/redis
    restart: always
```

> {userName} 에는 컴퓨터 사용자 이름이 들어간다.

### Docker Compose - MongoDB

```yml
version: "3"
services:
  mongodb:
    image: mongo
    container_name: mongodb
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: password
    volumes:
      - /Users/{userName}/mongodb/data:/data/db
    ports:
      - "27017:27017"
    restart: always
# connect URL : mongodb://root:password@localhost:27017
```

> {userName} 에는 컴퓨터 사용자 이름이 들어간다.

### Docker Compose - MongoDB Standalone to a Replica Set

```yml
version: "3.4"
services:
  mongodb:
    image: arm64v8/mongo:latest
    container_name: mongodb
    hostname: mongodb
    volumes:
      - ./.docker/mongodb/mongod.conf:/etc/mongod.conf
      - ./.docker/mongodb/mongodb.key:/etc/mongodb.key
      - ./.docker/mongodb/initdb.d/:/docker-entrypoint-initdb.d/
      - ./.docker/mongodb/data/db/:/data/db/
      - ./.docker/mongodb/data/log/:/var/log/mongodb/
    env_file:
      - .env
    environment:
      MONGO_INITDB_ROOT_USERNAME: ${MONGO_INITDB_ROOT_USERNAME}
      MONGO_INITDB_ROOT_PASSWORD: ${MONGO_INITDB_ROOT_PASSWORD}
      MONGO_INITDB_DATABASE: ${MONGO_INITDB_DATABASE}
      MONGO_REPLICA_SET_NAME: ${MONGO_REPLICA_SET_NAME}
    ports:
      - "27017:27017"
    healthcheck:
      test: test $$(echo "rs.initiate().ok || rs.status().ok" | mongo -u $${MONGO_INITDB_ROOT_USERNAME} -p $${MONGO_INITDB_ROOT_PASSWORD} --quiet) -eq 1
      interval: 10s
      start_period: 30s
    command:
      [
        "-f",
        "/etc/mongod.conf",
        "--replSet",
        "${MONGO_REPLICA_SET_NAME}",
        "--bind_ip_all",
      ]
```

### MongoDB 단일 리플리카 설정

[Convert a Standalone to a Replica Set](https://docs.mongodb.com/manual/tutorial/convert-standalone-to-replica-set/)

1. 실행 중인 mongod 인스턴스를 정지한다.

```
brew services stop mongodb-community
```

2. replSet 옵션을 주고 mongod 인스턴스를 재시작한다.

```
mongod --replSet rs0
```

3. mongosh로 mongod 인스턴스에 접속한다.

```
mongosh
```

4. 새로운 복제본 세트를 초기화한다.

```
rs.initiate()
```

5. `rs.conf()`로 복제본 세트의 구성을 확인하거나 `rs.status()`로 복제본 세트의 상태를 확인할 수 있다.

[Creating a mongo image set with --replSet ](https://github.com/docker-library/mongo/issues/246)
[MongoDB Single Node Replica Set For Dev Environment](https://github.com/MaBeuLux88/docker/tree/master/mongo-single-node-rs)

# > DBeaver 설정 (DBeaver 7.2.0)

### 키워드 대문자 자동 설정

키워드 대소문자 변경은 `Preferences > DBeaver > SQL Editor settings`를 통해 들어가서 `SQL Formatting`을 선택해서 `Formatter`를 설정 가능하다. 기본값은 대문자로 되어있다.

### 개발, QA, 운영 구분

`Window > Preferences > Database > Connection Types`에서 여러 가지 설정이 가능하다.
좌측 내비게이터에서 연결된 DB를 오른쪽 클릭하고 `Edit Connection > General`에 들어가면 `Connection type`을 변경 가능하며, 옆에 `Edit connection types`에서 커스텀이 가능하다.

`cd ~/LibraryDBeaverData/workspace6/General`을 옮겨서 DBeaver에서 데이터베이스 연결 정보와 작성한 SQL 스크립트들을 다른 컴퓨터로 옮길 수 있다.

### 기타

DBeaver를 사용하려면 JDK가 필요한데, 최신 버전 설치하는게 좋아보인다.

맥 OS 업데이트 이후에 기존에 사용하던 DBeaver가 열리지 않았는데, DBeaver GitHub Issuse에서는 JDK를 최신 버전으로 업데이트 하면 된다고 해서 업데이트하니 정상적으로 열렸다.

```bash
# adoptopenjdk15가 설치가 되지 않을 때
brew tap AdoptOpenJDK/openjdk


brew install --cask adoptopenjdk15
java -version
```
