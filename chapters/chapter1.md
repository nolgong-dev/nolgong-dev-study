# Hello Vue!

Vue.js는 프론트엔드 자바스크립트 프레임워크 입니다. 

자료 출처:  [https://velopert.com/](https://velopert.com/3007)  (패스트캠퍼스 vue.js 강사 블로그)
                 [https://kr.vuejs.org/v2/guide/index.html](https://kr.vuejs.org/v2/guide/index.html) (vue.js 도큐멘트)

# 소개

Vue.js 는 다른 프레임워크 (앵귤러, 리액트)에 비해 매우 작고 가벼우며 복잡도도 낮기 때문에 사용하기에 매우 간편하고 시작하기도 쉽습니다. 이름이 그러하듯, 뷰에만 초점을 두기 때문에 다른 라이브러리와 프레임워크와 혼용해서 사용하기도 쉽습니다. 

앵귤러와 리액트는 프로젝트를 시작하는게 스타터킷을 사용하면 꽤 쉽긴하지만, 정말 처음 시작하는 단계에서 뭔가 프로젝트를 설정하는 과정이 조금은 복잡합니다. Vue.js의 경우엔 CDN으로 블러와서 사용하기도 편하고 원한다면 webpack으로도 구성할 수 있습니다 ( 저는 개인적으로 webpack이 프로젝트 관리를 할 때 매주 편리하다고 생각합니다. )

이 프레임워크의 엄청난 장점 중하나는 매뉴얼이 다른 프레임워크, 라이브러리에 비해서 한글화가 엄청 잘 되어있습니다. 정리도 아주 잘 되어있구요! 매뉴얼중, ['다른 프레임 워크와의 비교'](https://kr.vuejs.org/v2/guide/comparison.html) 라는 글이 있는데 한번 꼭 읽어보시길 추천드립니다. 

# Vue의 특징

- 가상DOM을 사용합니다
- 컴포넌트를 제공합니다
- 뷰에만 집중하고 있고, 라우터, 상태관리를 위해 써드파티 라이브러리를 사용합니다.
- Vue 에서는 `<template>` 를 사용합니다.
```
    <!-- 바로 이렇게 말이죠 !! -->
    <!DOCTYPE html>
    <template>
      <div class="list-container">
        <ul v-if="items.length">
          <li v-for="item in items">
            {{ item.name }}
          </li>
        </ul>
        <p v-else>No items found.</p>
      </div>
    </template>
```
템플릿을 사용할 때의 장점은 HTML파일에서 바로 사용할 수 있따는 점입니다. 따라서 다른 HTML템플릿에서도 사용할 수 있죠 (예: 장고 템플릿, EJS)

- Vue에서도 서버사이드 렌더링이 지원됩니다. 리액트에 비하여 훨씬 개선되어있습니다. 그 이유는 스트리밍 서버사이드 렌더링이 지원되기 때문인데요! 스트리밍 서버사이드 렌더링이 지원되지 않으면 생기는 문제점은 동기적으로 서버 렌더링시 렌더링 코드가 진행되는 동안 이벤트 루프가 막히게 된다는 점입니다. 이러한 서버사이드 렌더링은 서버의 사용률이 높을 경우 성능을 악화시키고 response시간이 늘어나기도 하죠.

Vue에서는 스트리밍 서버사이드 렌더링이 지원되어 이벤트 루프가 막히지 않습니다. 따라서 유저들에게 더 빠른 결과를 반환 할 수 있죠.

# 맛보기

## [JSBin](https://jsbin.com/fivomus/edit?html,output) 열기

위 링크를 눌러서 JSBin을 들어가세요  

![](https://velopert.com/wp-content/uploads/2017/01/Screenshot-2017-01-21-164446.png)

동일한 화면이 떴나요? 상단의 JavaScript 탭을 누르면 JavaScript 를 작성 할 수 있는 박스도 생겨납니다. 여기서 입력하는 내용이 최우측 Output 에서 나타난답니다.

Output 부분에서 Auto-run JS 부분도 미리 체크를 해주세요. 

## Vue.js  불러오기

이제 Vue.js를 불러올 것입니다. 메뉴얼의 [설치방법](https://kr.vuejs.org/v2/guide/installation.html#CDN) 을 보시면 CDN주소가 제공되는데 거기에 있는 링크 중 unpkg에서 제공하는 링크를 사용하면 됩니다. 다음 링크를 사용하면 그때 그때 최신 버전으로 불러와줍니다. 

    https://unpkg.com/vue/dist/vue.js

주소를 복사하고, JSBin의 body 태그가 끝나기 전에 다음 코드처럼 스크립트 태그를 삽입해 주세요.

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <title>JS Bin</title>
    </head>
    <body>
    
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
      
    </body>
    </html>

## Hello, Vueworld!

프로젝트에 Vue를 불러왔으니 첫 예제 코드를 작성해봅시다.
body 태그에서 vue를 불러오기 전에 다음과 같이 코드를 삽입해주세요.

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <title>JS Bin</title>
    </head>
    <body>
    	<div id="app">
    		<h1>Hello, Vueworld!</h1>
    	</div>
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </body>
    </html>

여기까지는 그냥 평범한 HTML입니다. 한번, Hello, **Vue** 부분에서 Vue 대신에 다른 값이 들어가보게, 뷰를 정의해보겠습니다.

그렇게 하기 위해선, Vue 라고 적힌 부분을 다음과같이 `{{ name }}` 으로 바꿔주세요

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <title>JS Bin</title>
    </head>
    <body>
      
      <div id="app">
        <h1>Hello, {{ name }}</h1>
      </div>
      
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </body>
    </html>

그 다음에, JavaScript 탭을 열어서 다음과 같이 코드를 작성하세요.

    // 새로운 뷰를 정의합니다
    var app = new Vue({
      el: '#app', // 어떤 엘리먼트에 적용을 할 지 정합니다
      // data 는 해당 뷰에서 사용할 정보를 지닙니다
      data: {
        name: 'Vue'
      }   
    });

그리고 나서 화면을 보시면 아까와 같습니다. 우리가 **name** 이라는 변수를 뷰 안에 만들었고, 그 name 의 값을 Vue 를 정의하면서 data 안에 넣어서 설정한것이죠.

우리가 이 name 의 값을 ‘Vue’ 라고 설정했기에, 이 값이 `{{ name }}` 부분에 대입이 되는 것이랍니다.

지금까지 잘 렌더링이 됐다면, 한번 상단의 Console 탭을 열어서 다음과 같이 명령어를 입력해보세요.

>> app.name ="nolgong"

![](https://github.com/nolgong-dev/Vue-Study/MDimg/_2019-05-16__3-01983c7d-7aa3-4d58-b21c-65a97388cdeb.37.16.png)

위와 같이, 콘솔에서 `app.name = "nolong"` 라고 입력을 해보니 우측 화면에 바로 값이 바뀌어서 렌더링됩니다. 지금은, **one-way binding** 이 되어서 값을 업데이트하면 저렇게 반영이 바로 됩니다.

Vue 에서는 v-model 을 통해서 two-way binding 도 지원을 해주는데 그것에 대해선 추후 알아보겠습니다.

다음 챕터에서는 디렉티브에 대한 내용을 공유하도록 하겠습니다 :) (디렉티브는 Vue 엘리먼트에서 사용되는 특별한 속성입니다.)

그럼 다음 챕터에서 만나요! 안녕!