# 컴포넌트

## 컴포넌트가 무엇일까요?

컴포넌트는 Vue의 가장 강력한 기능 중 하나입니다. 기본 HTML 엘리먼트를 확장하여 재사용 가능한 코드를 캡슐화하는 데 도움이 됩니다.

Vue 컴포넌트는 Vue 인스턴스이기도 합니다. 그러므로 모든 옵션 객체를 사용할 수 있습니다. (루트에만 사용하는 옵션은 제외) 그리고 같은 라이프사이클 훅을 사용할 수 있습니다.

## 컴포넌트 사용하기

### 전역 등록

전역 컴포넌트를 등록하려면, `Vue.component(tagName, options)`를 사용합니다.

    //...
    
    Vue.component('my-component', {
      // 옵션
    })
    
    //index.js

일단 등록되면, 컴포넌트는 인스턴스의 템플릿에서 커스텀 엘리먼트 `my-component></my-component>` 로 사용할 수 있습니다. 루트 Vue 인스턴스를 인스턴스화하기 **전에** 컴포넌트가 등록되어 있는지 확인하세요. 아래처럼 말이죠!

    ...
    <div id="app">
    	 <my-component></my-component>
    </div>
    ...
    
    <!-- app.js -->

    // 등록
    Vue.component('my-component', {
      template: '<div>사용자 정의 컴포넌트 입니다!</div>'
    })
    
    // 루트 인스턴스 생성
    new Vue({
      el: '#app'
    })
    
    //index.js

### 지역등록

모든 컴포넌트를 전역으로 등록 할 필요는 없습니다. 컴포넌트를 `components` 인스턴스 옵션으로 등록함으로써 다른 인스턴스/컴포넌트의 범위에서만 사용할 수있는 컴포넌트를 만들 수 있습니다:

    <!-- Child.vue -->
    <template>
    	<div>사용자 정의 컴포넌트 입니다!</div>
    </template>

    // App.vue
    <template>
    	<div>
    		<Child><Child/>
    	</div>
    </template>
    
    // <my-component> 는 상위 템플릿에서만 사용할 수 있습니다.
    <script> 
    import Child from @/components/Child.vue
    new Vue({
      // ...
      components: {
        'Child': Child
      }
    })
    </script>

### `data` 는 반드시 함수여야 합니다.

Vue 생성자에 사용할 수 있는 대부분의 옵션은 컴포넌트에서 사용할 수 있습니다. 한가지 특별한 경우가 있습니다. **data 는 함수여야 합니다.** 실제로 이를 사용하는 경우를 예로 보시죠.

    Vue.component('simple-counter', {
      template: '<button v-on:click="counter += 1">{{ counter }}</button>',
      // 데이터는 기술적으로 함수이므로 Vue는 따지지 않지만
      // 각 컴포넌트 인스턴스에 대해 같은 객체 참조를 반환합니다.
      data () {
        return {
        counter: 0
        }
      }
    })
    
    new Vue({
      el: '#app'
    })
    
    //index.js

    <template>
    	<div id="app">
    	  <simple-counter></simple-counter>
    	  <simple-counter></simple-counter>
    	  <simple-counter></simple-counter>
    	</div>
    </template>
    
    //App.vue

### 컴포넌트 작성

Vue.js에서 부모-자식 컴포넌트 관계는 **props는 아래로**, **events 위로** 라고 요약 할 수 있습니다. 부모는 **props**를 통해 자식에게 데이터를 전달하고 자식은 **events**를 통해 부모에게 메시지를 보냅니다.

![](https://kr.vuejs.org/images/props-events.png)

## Props

### props로 데이터 전달하기

모든 컴포넌트 인스턴스에는 자체 **격리 된 범위** 가 있습니다. 즉, 하위 컴포넌트의 템플릿에서 상위 데이터를 직접 참조 할 수 없으며 그렇게 해서는 안됩니다. 데이터는 **`[props` 옵션](https://kr.vuejs.org/v2/api/#props)** 을 사용하여 하위 컴포넌트로 전달 될 수 있습니다.

prop는 상위 컴포넌트의 정보를 전달하기위한 사용자 지정 특성입니다. 하위 컴포넌트는`props`옵션을 사용하여 수신 할 것으로 기대되는 props를 명시적으로 선언해야합니다

    Vue.component('child', {
      // props 정의
      props: ['message'],
    	//템플릿의 값은 하위컴포넌트의 이름으로 대체될 수 있어요.
      template: '<span>{{ message }}</span>'
    
    })

    <!-- 상위 컴포넌트에서 "안녕하세요!" 라는 정보를 전달하는 예시-->
    <child message="안녕하세요!"></child>

>>> 여기서 주의!!!!

HTML 속성은 대소 문자를 구분하지 않으므로 문자열이 아닌 템플릿을 사용할 때 camelCased prop 이름에 해당하는 kebab-case(하이픈 구분)를 사용해야 합니다. 

    Vue.component('child', {
      // JavaScript는 camelCase로 써도,
      props: ['myMessage'],
      template: '<span>{{ myMessage }}</span>'
    })

    <!-- HTML DOM에 바로 쓸 때는 kebab-case만 가능해요-->
    <child my-message="안녕하세요!"></child>

### 동적 Props

이전에 디렉티브 챕터에서 배우셨죠? `v-bind` 를 사용하여 부모의 데이터에 props를 동적으로 바인딩 할 수 있습니다. 데이터가 상위에서 업데이트 될 때마다 하위 데이터로도 전달됩니다.

    <div>
      <input v-model="parentMsg">
      <br>
      <child v-bind:my-message="parentMsg"></child>
    </div>

>>> 객체로 전달하고 싶을 때,

객체의 모든 속성을 props로 전달하려면, 인자없이 v-bind를 쓸 수 있습니다. (`v-bind:prop-name="data-name"` 대신 `v-bind="object-name"`). 예를 들어 todo 객체가 있다면,

    todo: {
      text: 'Learn Vue',
      isComplete: false
    }

그런 다음,

    <todo-item v-bind="todo"></todo-item>
    
    <!-- 아래의 것과 똑같이 동작합니다.
    <todo-item
      v-bind:text="todo.text"
      v-bind:is-complete="todo.isComplete"
    ></todo-item> -->

>>> 숫자는 `v-bind`로 전달하세요!

초보자가 흔히 범하는 실수는 리터럴 구문을 사용하여 **숫자**를 전달하려고 시도하는 것입니다.

    <!-- 이것은 일반 문자열 "1"을 전달합니다. -->
    <comp some-prop="1"></comp>

리터럴 prop이기 때문에 그 값은 실제 숫자가 아닌 일반 문자열 `"1"` 로 전달됩니다. 실제 JavaScript 숫자를 전달하려면 값이 JavaScript 표현식으로 평가되도록 `v-bind`를 사용해야합니다.

    <!-- 이것은 실제 숫자로 전달합니다. -->
    <comp v-bind:some-prop="1"></comp>

>>>만약 하위 컴포넌트가 실수로 부모의 상태를 변경하면 어떡하죠??

- 이 prop는 초기 값을 전달 하는데만 사용되며 하위 컴포넌트는 이후에 이를 로컬 데이터 속성으로 사용하기만 할 때,

    props: ['initialCounter'],
    data: function () {
      return { counter: this.initialCounter }
    }

prop의 초기 값을 초기 값으로 사용하는 로컬 데이터 속성을 정의하면 됩니다.

- prop는 변경되어야 할 원시 값으로 전달될 때,

    props: ['size'],
    computed: {
      normalizedSize: function () {
        return this.size.trim().toLowerCase()
      }
    }

prop 값으로 부터 계산된 속성을 정의하면 됩니다.

이상으로 기본적인 컴포넌트 등록, 컴포넌트 데이터 통신을 알아보았습니다. 데이터 및 각 컴포넌트의 상태 관리를 더 효율적으로 하고 싶다면
[VueX](https://vuex.vuejs.org/kr/guide/) 라이브러리를 사용해 보세요!

> Vuex는 Vue.js 애플리케이션에 대한 상태 관리 패턴 + 라이브러리 입니다. 애플리케이션의 모든 컴포넌트에 대한 **중앙 집중식 저장소** 역할을 하며 예측 가능한 방식으로 상태를 변경할 수 있습니다.