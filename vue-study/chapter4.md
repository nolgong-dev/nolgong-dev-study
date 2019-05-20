# Vue-CLI로 webpack 사용법 알기  

다음 문서는 이 [링크](https://beomy.tistory.com/40)를 바탕으로 작성되었습니다.

## 설치

최소 요구사항으로 Node.js 4.x 이상(6.x 추천)과 [Git](https://git-scm.com/) 이 설치되어 있어야 합니다.

    // vue-cli 전역 설치, 권한에러시 sudo 추가
    npm install -g vue-cli

## 프로젝트 생성

    // vue init <template-name> <project-name>
    vue init webpack my-project

정말 간단하게 webpack을 사용하여 vue 프로젝트를 시작할 수 있습니다.

webpack 설정은 취향에 따라 해놓으면 되오나  아래 설정은.... ~~유닛테스트를 위한 설정을 모두 꺼놓은 상태... TDD하고 싶다(흑흑)~~   이후 나오는 예제는 아래의 프로젝트 설정을 기준으로 진행합니다.

![](https://raw.githubusercontent.com/nolgong-dev/Vue-Study/master/MDimg/ch4-1.png)

    build/
    ...
    config/
    dev.env.js
    index.js
    prod.env.js
    dist/
    ...
    node_modules/
    ...
    src/
    assets/
    ...
    components/
    HelloWorld.vue
    router/
    index.js
    App.vue
    main.js
    static/
    ...
    .babelrc
    .editorconfig
    .eslintignore
    .eslintrc.js
    .gitignore
    .postcssrc.js
    index.html
    package-lock.json
    package.json
    README.md

vue-cli를 통해 생성한 프로젝트의 디렉토리 구조는 위와 같습니다. 중요한 프로젝트 구조만 이야기 하면..

- `build/` : webpack 빌드 관련 설정이 모여 있는 디렉토리입니다.
- `config/` : 프로젝트에서 사용되는 설정이 모여 있는 디렉토리입니다. `dev.env.js` : `npm start` 시 적용되는 설정입니다. `prod.env.js`: `npm run build` 로 배포 버전에 적용되는 설정입니다.
- `dist/` : 배포버전의 Vue 어플리케이션 파일들이 모여 있는 디렉토리입니다. `npm run build` 명령어 실행시 생성됩니다.
- `node_modules/` : npm으로 설치되는 서드파트 라이브러리들이 모여 있는 디렉토리입니다.
- `src/` : 실제 대부분의 코딩이 이루어지는 디렉토리입니다. `assets/` : 이미지 등.. 어플리케이션에서 사용되는 파일들이 모여 있는 디렉토리입니다. `components/` : Vue 컴포넌트들이 모여 있는 디렉토리입니다. `router/` : vue-router 설정을 하는 디렉토리입니다. `App.vue` : 가장 최상위 컴포넌트입니다. `main.js` : 가장 먼저 실행되는 javascript 파일입니다. Vue 인스턴스를 생성하는 역활을 합니다.
- `index.html` : 어플리케이션의 뼈대가 되는 html 파일입니다.

## npm start 실행 순서

 `npm start` 로 어플리케이션이 실행되는 순서를 간단히 살펴보도록 하겠습니다.

### package.json

`package.json`의 `scripts`부분을 살펴보면,

    {
      ...
      "scripts": {
        "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
        "start": "npm run dev",
        "unit": "jest --config test/unit/jest.conf.js --coverage",
        "e2e": "node test/e2e/runner.js",
        "test": "npm run unit && npm run e2e",
        "lint": "eslint --ext .js,.vue src test/unit test/e2e/specs",
        "build": "node build/build.js"
      },
      ...
    }

`npm start`명령어는  `npm run dev`를 실행 하고,

`npm run dev`는 `webpack-dev-server --inline --progress --config build/webpack.dev.conf.js`를 실행 시키는 것을 볼 수 있습니다.

`webpack.dev.conf.js`와 `webpack.base.conf.js`는 webpack의 설정 파일로, 두 설정 파일에 따라 webpack이 프로젝트를 빌드하게 됩니다. 

## index.html

`index.html`은 어플리케이션의 뼈대가 되는 html 입니다.

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width,initial-scale=1.0">
        <title>my-project</title>
      </head>
      <body>
        <div id="app"></div>
        <!-- built files will be auto injected -->
      </body>
    </html>

`<div id="app"></div>`에 Vue 컴포넌트들이 마운팅 됩니다.

`<div id="app"></div>`에 마운팅 되는 이유는 `main.js`를 살펴보면 알 수 있습니다.

## main.js

    // The Vue build version to load with the `import` command
    // (runtime-only or standalone) has been set in webpack.base.conf with an alias.
    import Vue from 'vue'
    import App from './App'
    import router from './router'
     
    Vue.config.productionTip = false
     
    /* eslint-disable no-new */
    new Vue({
      el: '#app',
      router,
      components: { App },
      template: '<App/>'
    })

`main.js`는 Vue 인스턴스 `new Vue()`를 생성합니다.

`el: '#app'` 이 정의 되어 `index.html`의 `<div id="app"></div>`에 Vue 컴포넌트들이 마운팅되게 됩니다.

## App.vue

가장 먼저 마운팅 되는 App 컴포넌트를 살펴보도록 하겠습니다.

    <template>
      <div id="app">
        <img src="./assets/logo.png">
        <router-view/>
      </div>
    </template>
     
    <script>
    export default {
      name: 'App'
    }
    </script>
     
    <style>
    #app {
      font-family: 'Avenir', Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      color: #2c3e50;
      margin-top: 60px;
    }
    </style>

`index.html`의 `<div id="app"></div>`에 위의 App 컴포넌트`<div id="app"></div>`가 랜더링 됩니다. `<img />`와 `<router-view />`

가 랜더링 되는데, `<router-view />`에 무엇이 랜더링 되는지는`router/index.js`를 확인 하면 알 수 있습니다.

## router/index.js

vue-router에 사용 될 라우터들이 정의되어 있는 파일입니다.

    import Vue from 'vue'
    import Router from 'vue-router'
    import HelloWorld from '@/components/HelloWorld'
     
    Vue.use(Router)
     
    export default new Router({
      routes: [
        {
          path: '/',
          name: 'HelloWorld',
          component: HelloWorld
        }
      ]
    })

`/`로 접근할 경우 HelloWorld 컴포넌트를 랜더링하게 됩니다.

## components/HelloWorld.vue

`/` 로 접근 할 때 보이게 되는 컴포넌트입니다.

    <template>
      <div class="hello">
        <h1>{{ msg }}</h1>
        <h2>Essential Links</h2>
        <ul>
          <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
          <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
          <li><a href="https://chat.vuejs.org" target="_blank">Community Chat</a></li>
          <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
          <br>
          <li><a href="http://vuejs-templates.github.io/webpack/" target="_blank">Docs for This Template</a></li>
        </ul>
        <h2>Ecosystem</h2>
        <ul>
          <li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>
          <li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>
          <li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>
          <li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>
        </ul>
      </div>
    </template>
     
    <script>
    export default {
      name: 'HelloWorld',
      data () {
        return {
          msg: 'Welcome to Your Vue.js App'
        }
      }
    }
    </script>
     
    <!-- Add "scoped" attribute to limit CSS to this component only -->
    <style scoped>
    h1, h2 {
      font-weight: normal;
    }
    ul {
      list-style-type: none;
      padding: 0;
    }
    li {
      display: inline-block;
      margin: 0 10px;
    }
    a {
      color: #42b983;
    }
    </style>