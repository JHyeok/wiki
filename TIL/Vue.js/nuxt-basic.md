# Nuxt.js 기본

Nuxt는 Vue.js로 빠르게 웹을 제작할 수 있게 도와주는 프레임워크이다.
웹 애플리케이션을 제작할 때 필요한 뷰엑스, 라우터, Axios와 같은 라이브러리들을 미리 구성하여 싱글 페이지 애플리케이션(Single Page Application), 서버 사이드 렌더링(Server Side Rendering), 정적 웹 사이트(Static Generated Website)를 쉽게 제작할 수 있게 한다.

Nuxt 특징
- pages 폴더 기반의 자동 라우팅 설정
- 코드 스플리팅
- 서버 사이드 렌더링
- 비동기 데이터 요청 속성
- ES6/ES6+ 변환
- 웹팩을 비롯한 기타 설정

```
npx create-nuxt-app <projectName>
```

package manager는 Npm\
UI freamework는 Bootstrap Vue\
Server framework는 Express\
test framework는 Jest\
rendering mode는 Universal (SSR)

Assets - 이미지, LESS, SASS, Javascript 파일과 같은 컴파일 되지 않은 파일을 포함\
Components - Vue.js 컴포넌트를 포함\
Layouts - 에플리케이션의 레이아웃을 포함, 레이아웃은 페이지 모양을 변경하는데 사용되며 여러 페이지에 사용할 수 있음\
Middleware - 에플리케이션의 미들웨어를 포함, 미들웨어는 페이지가 렌더링되기 전에 실행되는 사용자 정의 기능\
Pages - 응용 프로그램의 보기 및 경로를 포함, Nuxt.js는 모든 .vue 파일을 읽어서 응용 프로그램의 라우터를 만듬\
Plugins - Vue.js 에플리케이션이 인스턴스화되기 전에 실행될 Javascript 플러그인이 포함\
Static - 정적 파일(변경되지 않는 파일)을 포함, 이러한 모든 파일은 응용프로그램의 `/` 으로 매핑 (예시: `/static/robots.txt` 는 `/robots.txt`로 매핑\
Store - Nuxt.js와 Vuex를 함께 사용하려는 경우. 

각 디렉토리별로 README.md가 있어서 어떤 역할에 쓰이는지 알 수 있다.

pages에 users폴더를 만들어서 `/users`를 하려고 했는데 페이지를 찾을 수 없다고 오류가 떴다.\
nodemon을 `restart(command: rs)`하거나, 다시 `npm run dev`를 하면 된다.

---
#### 참고

https://nuxtjs.org/guide