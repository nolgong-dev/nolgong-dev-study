# Directive 알기

디렉티브는 Vue 엘리먼트에서 사용되는 특별한 속성입니다. 엘리먼트에게 "이렇게 작동해!" 하고 지시를 해주는 지시문입니다. 

## 실습

이번에도 JSBin에서 작업합니다. 지난 포스트에서 Hello nolgong을 했었죠? 그 때 작성했던 코드에서 계속 진행하겠습니다.

[여기](https://jsbin.com/fivomus/1/edit?html,js,output)를 누르시면, 이전 포스트에서 사용했던 코드가 사전에 작성된 JSBin프로젝트를 열어서 바로 시작을 할 수 있습니다. (페이지에 들어가서 우측 상단의 Auto Run JS가 체크되어 있는지 확인하세요!)

## 디렉티브 소개

디렉티브는 Vue의 기능들을 사용하기 위해서 사용하는 HTML태그 안에 들어가는 하나의 속성입니다. 디렉티브는 여러 종류가 있는데요. 모두 `v-text` 이런 식으로 `v-어쩌구` 형태를 가집니다. 

직접 사용해보면 디렉티브가 이런 역할을 하는구나 ! 하고 쉽게 이해하실 겁니다.

## 디렉티브 종류

디렉티브는 현재 13개의 종류가 있습니다. 디렉티브 리스트는 여기서 확인할 수 있는데요, 그 중 제일 많이 사용하는 디렉티브 9개를 먼저 다뤄보겠습니다.

- v-text
- v-show
- v-if
- v-else
- v-else-if
- v-bind
- v-model
- v-for
- v-on

## 사용해보기

### v-text 디렉티브

저번에는 템플릿 구문을 이용해서 다음과 같은 코드를 작성했습니다!
`<h1> {{ name }} </h1>` 

{{ }} 를 사용하는 대신에 v-text 라는 디렉티브를 사용하여 구현해보겠습니다.

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <title>JS Bin</title>
    </head>
    <body>
      
      <div id="app">
        <h1>Hello 
          <span v-text="name"></span>
        </h1>
      </div>
      
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </body>
    </html>

이전과 달라진 것이 없지요? span태그를 만들어서 디렉티브를 사용하여 내부의 값이 Vue 앨리먼트의 name변수로 설정되게 했습니다.

아주 간단하쥬?

### v-show  디렉티브

이 디렉티브는 해당 엘리먼트가 보여질지, 보여질지 않을지 `true`/ `false`값으로 지정할 수 있습니다. 

자바스크립트의 data부분에 다음과 같이 visible이랑 변수를 넣으세요

    // 새로운 뷰를 정의합니다
    var app = new Vue({
      el: '#app', // 어떤 엘리먼트에 적용을 할 지 정합니다
      // data 는 해당 뷰에서 사용할 정보를 지닙니다
      data: {
        name: '<i>이탈릭</i>',
        visible: true
      }   
    });

여기서 변수명은 마음대로 정해도 됩니다. visible로 해도 되고 show로 해도 되고요. 네이밍은 여러분 자유입니다. 

그 다음, html에서 v-show 값을 우리가 만든 변수명, visible로 설정하겠습니다.

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <title>JS Bin</title>
    </head>
    <body>
      
      <div id="app">
        <h1>Hello 
          <span v-show="visible" v-html="name"></span>
        </h1>
      </div>
      
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </body>
    </html>

그리고, Console 탭을 열어서 `app.visible = false` 라고 입력해보세요. 사라지나요? 그럼 다시 `app.visible = true` 라고 입력해보세요.

### v-if 디렉티브 (중요)

`v-if` 디렉티브는 위에서 배운 디렉티브와 살짝 비슷한데, 여기에 변수명 대신에 조건문을 씁니다. 한번 사용을 해볼까요?

data 부분에 기존에 있던 것을 지우고 value 에 값을 0으로 설정하세요.

    // 새로운 뷰를 정의합니다
    var app = new Vue({
      el: '#app', // 어떤 엘리먼트에 적용을 할 지 정합니다
      // data 는 해당 뷰에서 사용할 정보를 지닙니다
      data: {
        value: 0
      }   
    });

한번 `v-if`를 사용하여 값이 5 이상일 때만 보이도록 설정해 볼까요? 해당 디렉티브의 값에 조건문을 넣어주시면 됩니다. 큰 따옴표로 감싸주시구요.

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <title>JS Bin</title>
    </head>
    <body>
      
      <div id="app">
        <h1 v-if="value > 5">value 가 5보다 크군요</h1>
      </div>
      
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </body>
    </html>

렌더링된 화면을 보면 아무것도 안나올 것입니다. 한번 콘솔에서 app.value = 6 이렇게 값을 5보다 큰 값으로 설정해보세요.

![](https://velopert.com/wp-content/uploads/2017/01/Peek-2017-01-21-23-15.gif)

### v-else 디렉티브

이름만 봐도 예상가죠? if가 있다면 else도 있겠죠? v-if 디렉티브를 사용했을 때 그 아래에 v-else디렉티브를 사용하는 엘리먼트를 넣어주면 윗부분의 조건문이 만족하지 않을 때 보여집니다.

html부분을 다음과 같이 수정해주세요.

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <title>JS Bin</title>
    </head>
    <body>
      
      <div id="app">
        <h1 v-if="value > 5">value 가 5보다 크군요</h1>
        <h1 v-else>value 가  5 보다 작아요</h1>
      </div>
      
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </body>
    </html>

`v-if`바로 아래에 `v-else`디렉티브를 사용했죠? 이 디렉티브의 값은 따로 설정해주지 않아도 됩니다.

한번 결과를 볼까요?

![](https://velopert.com/wp-content/uploads/2017/01/Peek-2017-01-21-23-26.gif)

### v-else-if 디렉티브

`v-else-if` 는 첫번째 조건문의 값이 참이 아닐때. 다른 조건문을 체크하여 다른 결과물을 보여줄 수 있게 해줍니다.

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <title>JS Bin</title>
    </head>
    <body>
      
      <div id="app">
        <h1 v-if="value > 5">value 가 5보다 크군요</h1>
        <h1 v-else-if="value === 5">값이 5네요</h1>
        <h1 v-else>value 가  5보다 작아요</h1>
      </div>
      
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </body>
    </html>

`v-else-if` 는, 언제나 `v-if` 디렉티브를 사용하는 엘리먼트의 다음위치에 있어야 합니다. 만약에 `v-else` 디렉티브가 사용되는 경우엔 그 사이에 위치해있어야 하구요, 이 디렉티브를 여러번 사용해도 됩니다.

### v-bind (중요)

이전에는 {{ }} 를 사용해서 name값을 설정하도록 했습니다. 이렇게 html태그 안의 내용을 Vue인스턴스 안의 데이터값으로 설정할 수 있죠. 하지만 **HTML 태그의 속성값**을 데이터 값으로 사용해야 한다면 어떻게 해야할까요?

예를 들어서, 이미지 태그 `<img src="링크주소" />` 의 링크 부분에서 data 안에 명시되어있는 값을 사용하고 싶다면? 그럴 때 바로 `v-bind` 를 사용합니다!

만약에, 여러분들이 HTML 엘리먼트에서 src값을 Vue 엘리먼트의 데이터 중 image로 설정하고 싶다면. `<img v-bind:src = "image" />` 와 같은 형식으로 사용하면 됩니다. `v-bind:` 뒤에 속성의 이름을 넣어주는 것이죠. 

한번 직접 사용해볼까요? javascript  탭에 다음과 같이 코드를 입력하세요

    // 새로운 뷰를 정의합니다
    var app = new Vue({
      el: '#app', // 어떤 엘리먼트에 적용을 할 지 정합니다
      // data 는 해당 뷰에서 사용할 정보를 지닙니다
      data: {
        name: 'Vue',
        feelsgood: 'https://imgh.us/feelsgood_1.jpg'
      }   
    });

html 탭에 다음과 같이 코드를 입력하세요

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
        <img v-bind:src="feelsgood"/>
      </div>
      
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </body>
    </html>

자, 이제 제대로 작동합니다. 

![](https://velopert.com/wp-content/uploads/2017/01/Screenshot-from-2017-01-22-15-04-45.png)

조금 더 응용해 볼까요? 사실, {{ }} 태그나, 디렉티브를 사용할 때 그 내부의 값을 꼭 데이터 명으로 해야하는 것은 아니에요. 그 안에 자바스크립트 표현식을 사용할 수 있습니다.

예를 들어, 이런식으로도 사용할 수 있어요.

![](https://velopert.com/wp-content/uploads/2017/01/Screenshot-from-2017-01-22-15-14-08.png)

머스태쉬 태그 안에 자바스크립트 코드를 입력하여 현재의 시각이 나타나도록 해보았습니다. 이건 너무 간단한가요?  조금 더 응용을 해보자면 ...

Vue 인스턴스의 데이터 안에 smile 값에 따라 다른 이미지를 보여주는 예제를 보여드릴게요.

    // 새로운 뷰를 정의합니다
    var app = new Vue({
      el: '#app', // 어떤 엘리먼트에 적용을 할 지 정합니다
      // data 는 해당 뷰에서 사용할 정보를 지닙니다
      data: {
        name: 'Vue',
        smile: true,
        feelsgood: 'https://imgh.us/feelsgood_1.jpg',
        feelsbad: 'http://imgh.us/feelsbad.jpg'
      }   
    });

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
        <h2>{{ Date() }}</h2>
    <!-- (조건) ? (true일때 값) : (false일때 값) -->
        <img :src="smile ? feelsgood : feelsbad"/>
      </div>
      
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </body>
    </html>

코드를 다 작성한 후, console에 들어가서 app.smile값에 변화를 줘본 결과입니다.

![](https://velopert.com/wp-content/uploads/2017/01/Peek-2017-01-22-15-24.gif)

### v-for (중요)

`v-for` 디렉티브는 정말 사용하기 쉬우면서도 강력크합니다! 이 디렉티브는  HTML 에서 for-loop을 구현하기 위하여 사용됩니다. 즉. 비슷한 내용을 반복적으로 보여줄 때 사용되는 것이죠! 이런식으로 무언가의 목록을 구현할 때 잘 사용되는 디렉티브 입니다.

- 데이터 추가

우선 렌더링할 데이터들을 객체 배열 형식으로 자바스크립트 탭을 켜서 자바스크립트 코드 안에 입력해보세요.

    var app = new Vue({
      el: '#app', 
      data: {
        todos: [
          { text: 'Vue.js 튜토리얼 작성하기' },
          { text: 'Webpack2 알아보기' },
          { text: '사이드 프로젝트 진행하기' }
        ]
      }   
    });

- v-for 디렉티브 사용

자, 이제 v-for 디렉티브를 사용할 차례입니다. 이 디렉티브를 반복할 태그에서 사용하면 되는데요. (우리의 경우엔 `li` 리스트 태그가 되겠습니다.)

이 디렉티브는 `item in items` 의 형식으로 작성합니다.

여기서 `items` 는 Vue 엘리먼트의 데이터 안에 들어있는 배열 이름 (우리의 경우는 todos 가 됩니다) 으로 설정하고,

`item` 은 렌더링 하게 될 때, 각 원소를 가르키는 별칭(alias) 입니다 (우리의 경간단하죠? Vue 를 사용하면 자바스크립트를 그렇게 많이 입력하지 않아도, 이렇게 쉽게 구현 할 수 있답니다우엔 todo 가 되겠군요. 이건 별칭이라 사실 마음대로 작성하셔도 된답니다)

그럼 html을 다음과 함께 수정해 보세요

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <title>JS Bin</title>
    </head>
    <body>
      
      <div id="app">
        <h2>오늘 할 일</h2>
        <ul>
          <li v-for="todo in todos">{{ todo.text }}</li>
        </ul>
      </div>
      
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </body>
    </html>

 결과물을 확인해 볼까요?

![](https://velopert.com/wp-content/uploads/2017/01/Screenshot-2017-01-27-151833.png)

- index 값을 받아오기

렌더링을 할 때. 각 원소들을 순서번호(index)를 가져오려면. 디렉티브 값에 `(todo, index) in todos` 형식으로 작성을 하면 됩니다. 

![](https://velopert.com/wp-content/uploads/2017/01/Screenshot-2017-01-27-152331.png)

여기서 `index` 라는 레퍼런스 이름은 우리가 정하는 것이기 때문에, `i` 로 대체해도 상관 없답니다. 이 인덱스 값은 나중에 리스트에 있는 값을 제거 및 수정 할 일이 있을때, 이 값을 참조하여 처리할 수 있습니다.

### v-model (중요)

이전의 디렉티브는 데이터 값이 뷰에 나타나는 단방향 바인딩입니다. v-model은 양방향 바인딩입니다. 뷰 ⇄ 데이터 형태로 바인딩하여 데이터가 양 방향으로 흐르게 해주는 것 입니다. 즉, 데이터에 있는 값이 뷰에 나타나고, 이 뷰의 값이 바뀌면 데이터의 값도 바뀌는것이죠.

HTML 에서 **input** 태그를 작성하고, 그 태그의 `v-model` 디렉티브를 data 레퍼런스에 명시된 것으로 설정하세요. 우리의 경우엔. `name` 이 되겠군요.

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
        <input type="text" v-model="name"/>
      </div>
      
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </body>
    </html>

    var app = new Vue({
      el: '#app', 
      data: {
        name: 'Vue'
      }   
    });

v-model 을 설정함으로서, 이 input엘리먼트의 값이 업데이트 되면 name값이 자동으로 바뀌게 한 것이죠.

![](https://velopert.com/wp-content/uploads/2017/01/Peek-2017-01-27-16-44.gif)

`v-model` 디렉티브는 이렇게 폼에 관련된 태그에만 사용 될 수 있습니다.  `<input>` `<select>` `<textarea>`폼을 컨트롤하는데에는 정말 유용하지만, 이것만으로는 프로젝트의 인터랙션을 디자인하기엔 역부족이죠. 

마지막 디렉티브, `v-on` 디렉티브는, 이벤트 처리를 담당하는 디렉티브로서, 더 많은 종류의 인터랙션을 관리 할 수 있게 됩니다.

### v-on (중요)

시작하기에 앞서서 기본적인 것을 체크합니다. 이벤트란? 웹 페이지에서 하는 모든 동작이 바로 이벤트 입니다. 사용자가 마우스를 움직이면 mousemove 이벤트, 키보드를 누르면 keypress 이벤트, 클릭을 하면 click이벤트 입니다. 이렇게 웹에서 사용될 수 있는 이벤트들의 목록은 [w3school](http://www.w3schools.com/tags/ref_eventattributes.asp) 에도 자세히 나와있습니다.

- 메소드 (method)

우리가 카운터를 만들기 위해선 버튼의 click 이벤트 발생시 우리가 준비한 작업을 하도록 설정을 해야겠죠. 그러기 위해선 이벤트가 발생했을 때 어떤 작업을 할 지, 함수를 준비해주어야합니다. 이 함수는 우리의 뷰 인스턴스 내부의 위치해있는데요 Vue 객체와 관계가 있으므로 이를 method라고 부릅니다.

메소드를 준비할 때는 우리가 뷰 인스턴스에서 사용할 데이터들을 data안에 넣은 것처럼, 함수들을 만들어서 뷰 인스턴스의 methods안에 넣으면 됩니다. 

다음 코드에서는, 데이터 모델에 number라는 변수를 만들고 , 값을 증가시키는 increment, 감소시키는 decrement메소드들을 준비합니다.

    var app = new Vue({
      el: '#app', 
      data: {
        number: 0
      },
      // app 뷰 인스턴스를 위한 메소드들
      methods: {
        increment: function() {
          // 인스턴스 내부의 데이터모델에 접근 할 땐,
          // this 를 사용한다
          this.number++;
        },
        decrement: function() {
          this.number--;
        }
      }
    });

메소드 내에서 인스턴스 내부의 데이터 모델에 접근 할 때에는 `this` 키워드를 사용합니다.

- 뷰 템플릿 준비하기

html에서 number값을 보여주고, 버튼 두 개도 만들어주겠습니다.

그리고 `v-on` 디렉티브를 이벤트 처리를 위한 태그에 설정하면 됩니다. 우리의 경우엔 button 태그에 넣어주면 되겠죠?

이 디렉티브를 설정할 때는 다음과 같은 형식으로 합니다

`v-on:이벤트 이름="메소드 이름"`

우리가 이번에 처리할 이벤트 이름은 click 이고, 메소드 이름은 increment 또는 decrement가 됩니다.

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <title>JS Bin</title>
    </head>
    <body>
      
      <div id="app">
        <h1>카운터: {{ number }}</h1>
        <button v-on:click="increment">증가</button>
        <button v-on:click="decrement">감소</button>
      </div>
      
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </body>
    </html>

결과 화면을 보면,

![](https://velopert.com/wp-content/uploads/2017/01/Peek-2017-01-27-21-59.gif)

간단하죠? Vue 를 사용하면 자바스크립트를 그렇게 많이 입력하지 않아도 간단하게 구현할 수 있어요.

**::: 조금 더 편하게 사용하기**

위 코드를 조금 더 편하게 작성할 수 있는 방법이 존재합니다. 바로 `v-on:` 을 `@` 로 대체하는 것입니다.

    <div id="app">
      <h1>카운터: {{ number }}</h1>
      <button @click="increment">증가</button>
      <button @click="decrement">감소</button>
    </div>

코드가 훨씬 간편해졌습니다 후후...