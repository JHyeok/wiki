# Tool

Visual Studio Code

mRemoteNG - 종합 원격 제어툴

PuTTY - a free SSH and telnet client for Windows

S3 Browser - Amazon S3 Client for Windows

fork - Git GUI Client

MySQL WorkBench

SQLyog - MySQL 클라이언트/관리툴

Fiddler - Free Web Debugging Proxy

WebSurge – Free Performance Testing Tool

nGrinder - 부하 테스트

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