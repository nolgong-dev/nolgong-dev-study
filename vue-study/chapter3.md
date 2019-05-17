# computed 속성 알기

## computed 속성

템플릿 내에 표현식을 넣으면 편리합니다. 하지만 너무 많은 연산을 템플릿 안에서 하면 코드가 비대해지고 유지보수가 어렵습니다. 아래의 예제를 보시죠.

    <template>	
    	<div id="example">
    	    {{ message.split('').reverse().join('') }}
    	</div>
    <template>

이 템플릿은 더이상 명료하지 않습니다 `message` 를 역순으로 표시한다는 것을 알려면 찬찬히 살펴봐야 하겠죠. 템플릿에 메세지를 역순으로 표시할 일이 더 있으면... 템플릿은 더 복잡해질거예요.

복잡한 로직이라면 반드시 **computed** 속성을 사용해야 하는 이유입니다.

### 기본예제

    <div id="example">
      <p>원본 메시지: "{{ message }}"</p>
      <p>역순으로 표시한 메시지: "{{ reversedMessage }}"</p>
    </div>

    var vm = new Vue({
      el: '#example',
      data: {
        message: '안녕하세요'
      },
      computed: {
        // 계산된 getter
        reversedMessage: function () {
          // `this` 는 vm 인스턴스를 가리킵니다.
          return this.message.split('').reverse().join('')
        }
      }
    })

결과는...

![](https://raw.githubusercontent.com/nolgong-dev/Vue-Study/master/MDimg/ch3-1.png)

이 예제에서는 computed 속성인 `reversedMessage`를 선언했습니다. 우리가 작성한 함수는 `vm.reversedMessage`속성에 대한 getter 함수로 사용됩니다. 콘솔에서 이 예제를 직접 해볼 수 있습니다.

    console.log(vm.reversedMessage) // => '요세하녕안'
    vm.message = 'Goodbye'
    console.log(vm.reversedMessage) // => 'eybdooG'

`vm.reversedMessage`의 값은 항상 `vm.message`의 값에 의존합니다.

### method vs computed 속성의 캐싱

표현식에서 메소드를 호출하여 같은 결과를 얻을 수도 있습니다. 

    <p>뒤집힌 메시지: "{{ reversedMessage() }}"</p>

    // 컴포넌트 내부
    methods: {
      reversedMessage: function () {
        return this.message.split('').reverse().join('')
      }
    }

computed 속성 대신 methods 같은 함수를 정의할 수도 있습니다. 최종 결과에 대해 두 가지 접근 방식은 서로 동일합니다. 차이점은 **computed 속성은 종속 대상을 따라 저장(캐싱)**된다는 것 입니다. computed 속성은 해당 속성이 종속된 대상이 변경될 때만 함수를 실행합니다. 즉 `message`가 변경되지 않는 한, computed 속성인 `reversedMessage`를 여러 번 요청해도 계산을 다시 하지 않고 계산되어 있던 결과를 즉시 반환합니다.

method와 computed의 차이를 눈으로 확인해 보고 싶다면 >> [여기](https://medium.com/@hozacho/%EB%A7%A8%EB%95%85%EC%97%90-vuejs-%EA%B3%84%EC%82%B0%EB%90%9C-%EC%86%8D%EC%84%B1-vuejs-instance-computed-93cb6ad7dca9)

이에 비해 메소드를 호출하면 렌더링을 다시 할 때마다 **항상** 함수를 실행합니다.

캐싱이 왜 필요할까요? 계산에 시간이 많이 걸리는 computed 속성인 **A**를 가지고 있다고 해봅시다. 이 속성을 계산하려면 거대한 배열을 반복해서 다루고 많은 계산을 해야 합니다. 그런데 **A** 에 의존하는 다른 computed 속성값도 있을 수 있습니다. 캐싱을 하지 않으면 **A** 의 getter 함수를 꼭 필요한 것보다 더 많이 실행하게 됩니다! 캐싱을 원하지 않는 경우 메소드를 사용하세요.

### computed 속성의  setter 함수

computed 속성은 기본적으로 getter 함수만 가지고 있지만, 필요한 경우 setter 함수를 만들어 쓸 수 있습니다.

    var vm = new Vue({	
    	el: '#demo',
      data: {
        firstName: 'Foo',
        lastName: 'Bar'
      },
      computed: {
    	 fullName: {
    	   // getter
    	   get: function () {
    	      return this.firstName + ' ' + this.lastName
    	    },
        // setter
        set: function (newValue) {
          var names = newValue.split(' ')
          this.firstName = names[0]
          this.lastName = names[names.length - 1]
        }
      }
    }
    // ...

이제 콘솔에 `vm.fullName = 'John Doe'`를 실행하면 설정자가 호출되고 `vm.firstName`과 `vm.lastName`이 그에 따라 업데이트 됩니다.