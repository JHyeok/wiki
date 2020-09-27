## > Visual Studio Code 환경

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
    "powermode.enabled": true,
    "powermode.shakeIntensity": 1,
    "powermode.explosionSize": 14,
    "powermode.enableShake": false,
    "explorer.confirmDelete": false,
    "editor.tabSize": 2,
    "typescript.implementationsCodeLens.enabled": true,
    "typescript.referencesCodeLens.enabled": true,
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
}
```

## > 맥린이 맥북 적응하기

#### 맥OS에서 NVM으로 Node 설치

```
sudo curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash

nvm install v12
```

- 참고 : https://gist.github.com/falsy/8aa42ae311a9adb50e2ca7d8702c9af1


#### zsh에서 Node 명령어 오류가 발생할 때

```
vi .zshrc

export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

- 참고 : https://likejirak.tistory.com/m/224


#### 맥OS의 키보드와 마우스, 스크롤 환경 설정을 해야 한다.
#### 키보드에서는 키 반복 빠르게, 지연시간 짧게 설정
#### 맥OS에서 fn키 누르지 않고 F1 ~ F12 사용하도록 설정
#### 단어 자동완성, 맞춤법 해제하기

#### 맥북 한영을 윈도우처럼 오른쪽 커맨드로 변경
- 참고 : https://m.post.naver.com/viewer/postView.nhn?volumeNo=17887832&memberNo=724&searchKeyword=%EB%A7%A5&searchRank=7

#### Docker에서 Redis 설치하고 자동 실행 설정

```
docker pull redis:alpine
docker run —name myredis -d -p 6379:6379 redis:alpine
```
재부팅 시에 Docker의 컨테이너가 자동으로 시작되도록 설정하려면 `--restart=always` 옵션이 필요하다.

```
docker update --restart=always <container-id>
ex)
docker update --restart=always 225a25d34ad9
```