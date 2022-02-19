# Node.js 패키지 관리

Node.js의 패키지 관리에 주로 사용하는 두 가지는 [NPM](https://www.npmjs.com/)(Node Package Manager)과 [Yarn](https://classic.yarnpkg.com/en/)이다.

## NPM으로 패키지 관리

`npm update`를 입력하면 설치된 의존성 모듈들을 전부 업데이트한다.

```sh
npm update gatsby
```

특정한 모듈만 업데이트하려면 위처럼 `npm update 모듈명`

```sh
npm outdated
```

npm 버전들을 관리할 수 있다.

많은 의존성 모듈에서 새로운 버전이 나오는 모듈을 일일이 확인하기 어려운데 `npm outdated` 명령어를 사용하면 의존성 모듈 중에 업데이트가 필요한 모듈을 확인할 수 있다.

`npm outdated`를 실행하면 업데이트가 필요한 모듈만 정리되어 나온다.

"Current"는 현재 설치된 버전이고 "Wanted"는 `package.json`에 지정한 버전 범위로 설치되는 최대 범위를 의미한다.

즉, `npm update`를 실행하면 설치되는 버전이다. "Latest"는 모듈의 최신 버전이다. 위 화면에서는 "Wanted"와 "Latest"가 같은 모듈이 빨간색으로 표시되었고 "Latest"가 "Wanted"보다 높은 모듈은 구별할 수 있게 노란색으로 표시되었다.

## npm-check-updates를 활용하기

```sh
# 설치
npm install -g npm-check-updates
# minor 버전까지만 package.json 업데이트
ncu -u --target minor
```

package.json이 최신화되었으니 이 정보를 바탕으로 `npm install` 하거나 package-lock.json, node_modules을 지우고 `npm install` 하거나 그 이후는 원하는 대로 사용할 수 있다.

기존의 `npm update` 명령어로는 `^`에 표기된 Minor 버전들까지로만 업데이트한다.

```sh
ncu -u
```

`npm-check-updates`를 사용해서 Major 버전들도 모두 업데이트할 수 있다. package.json이 최신화되었으니 `npm install`로 다시 설치하면 된다.

```sh
npm install
```

```sh
# 특정 패키지만 업데이트
ncu eslint
```

## Yarn으로 패키지 관리

`yarn outdated`로 어떤 패키지를 업데이트해야 하는지 확인한 이후에,

`yarn upgrade`를 사용 하고 나면 yarn.lock 파일만 업데이트 되고 node_modules의 패키지만 업데이트가 된다.

`package.json`에 버전이 나와있는 부분이 업데이트가 되어있지 않다.

NPM을 사용할 때는 이런 일이 없지만 yarn에서는 이런식으로 yarn.lock에 나와있는 버전과 package.json의 버전이 다를 수 있다.

```sh
yarn global add syncyarnlock // install syncyarnlock globally
yarn upgrade // update dependencies, updates yarn.lock
syncyarnlock -s -k // updates package.json with versions installed from yarn.lock
yarn install // updates yarn.lock with current version constraint from package.json
```

위는 해결방법이다.

syncyarnlock를 전역으로 설치하고, `yarn upgrade`로 의존성을 최신 버전으로 올린 이후에, syncyarnlock를 사용하면 `package.json`도 최신화된다.

yarn outdated를 사용했을 때, 빨강색 버전으로 나오는 Major 버전들로 모두 업데이트 하는 방법은 `--latest` 플래그를 사용하면 된다.

```sh
yarn upgrade --latest
yarn upgrade gatsby --latest
```

---
#### 참고

https://blog.outsider.ne.kr/1228

https://github.com/yarnpkg/yarn/issues/3266

https://flaviocopes.com/update-npm-dependencies/