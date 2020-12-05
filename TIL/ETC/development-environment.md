# > Visual Studio Code 환경

### Visual Studio Code 확장 프로그램 관리

```bash
# Windows (PowerShell)
code --list-extensions | % { "code --install-extension $_" }

# Unix 계열
code --list-extensions | xargs -L 1 echo code --install-extension
```

```
code --install-extension azemoh.one-monokai
code --install-extension dbaeumer.vscode-eslint
code --install-extension eamodio.gitlens
code --install-extension esbenp.prettier-vscode
code --install-extension ms-azuretools.vscode-docker        
code --install-extension MS-CEINTL.vscode-language-pack-ko  
code --install-extension ms-python.python
code --install-extension octref.vetur
code --install-extension shd101wyy.markdown-preview-enhanced
code --install-extension tht13.python
```

### Visual Studio Code Settings

`settings.json`

```
{
  "files.watcherExclude": {
    "**/node_modules": true,
    "**/platforms": true,
    "**/plugins": true
  },
  "eslint.alwaysShowStatus": true,
  "[vue]": {},
  "explorer.confirmDelete": false,
  "editor.tabSize": 2,
  "typescript.implementationsCodeLens.enabled": true,
  "typescript.referencesCodeLens.enabled": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "editor.renderIndentGuides": true,
  "terminal.integrated.shell.windows": "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
  "javascript.updateImportsOnFileMove.enabled": "always",
  "typescript.updateImportsOnFileMove.enabled": "always",
  "[typescript]": {
    "editor.formatOnSave": true
  },
  "workbench.colorTheme": "One Monokai"
}
```

> Windows 환경에서 `Ctrl + Shift + P` 단축키로 `settings.json` 수정 가능하다.

# > 맥린이 맥북 적응하기

### 맥OS에서 NVM으로 Node 설치

```
sudo curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash

nvm install v12
```

- 참고 : https://gist.github.com/falsy/8aa42ae311a9adb50e2ca7d8702c9af1


### zsh에서 Node 명령어 오류가 발생할 때

```
vi .zshrc

export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

- 참고 : https://likejirak.tistory.com/m/224

### 맥북 환경설정 순서

1. 맥OS의 키보드와 마우스, 스크롤 환경 설정을 해야 한다.
2. 키보드에서는 키 반복 빠르게, 지연시간 짧게 설정
3. 맥OS에서 fn키 누르지 않고 F1 ~ F12 사용하도록 설정
4. 단어 자동완성, 맞춤법 해제하기
5. 맥북 한영을 윈도우처럼 오른쪽 커맨드로 변경 (Karabiner-Elements)
6. 터치패드 설정 변경
7. 맥 파인더 경로 막대 설정

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
      MYSQL_ROOT_PASSWORD: "root1234"  # MYSQL 패스워드 설정 옵션
    command: # 명령어 실행
      - --character-set-server=utf8mb4
      - --collation-server=utf8mb4_unicode_ci
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

# > DBeaver 설정 (DBeaver 7.2.0)

### 키워드 대문자 자동 설정

키워드 대소문자 변경은 `Preferences > DBeaver > SQL Editor settings`를 통해 들어가서 `SQL Formatting`을 선택해서 `Formatter`를 설정 가능하다. 기본값은 대문자로 되어있다.

### 개발, QA, 운영 구분

`Window > Preferences > Database > Connection Types`에서 여러 가지 설정이 가능하다.
좌측 내비게이터에서 연결된 DB를 오른쪽 클릭하고 `Edit Connection > General`에 들어가면 `Connection type`을 변경 가능하며, 옆에 `Edit connection types`에서 커스텀이 가능하다.

### 기타

DBeaver를 사용하려면 JDK가 필요한데, 최신 버전 설치하는게 좋아보인다.

맥 OS 업데이트 이후에 기존에 사용하던 DBeaver가 열리지 않았는데, DBeaver GitHub Issuse에서는 JDK를 최신 버전으로 업데이트 하면 된다고 해서 업데이트하니 정상적으로 열렸다.

```bash
brew cask install adoptopenjdk15
```


