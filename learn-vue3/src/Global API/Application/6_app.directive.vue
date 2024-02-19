<template>
	<div></div>
</template>
<!--  

  app.directive()

    app.directive() 메서드는 Vue 3에서 커스텀 디렉티브를 등록하는 데 사용된다.
    디렉티브는 DOM요소의 특정 동작 또는 집합을 바인딩하는 Vue의 강력한 기능 중 하나이다.
    Vue가 제공하는 기본 디렉티브 외에도 사용자 정의 디렉티브를 생성하여 애플리케이션의 동작을 확장할 수 있다.

    'app.directive()'를 사용하여 커스텀 디렉티브를 등록하는 기본적인 구문은 다음과 같다.

      app.directive('directive-name', directiveDefinition);

    여기서

    -'directive-name'은 등록할 디렉티브의 이름이다.
    -'directiveDefinition'은 디렉티브의 정의이다. 이것은 객체 또는 디렉티브 훅을 제공하는 함수일 수 있다.

    아래는 클릭 이벤트가 발생할 때마다 해당 요소의 배경색을 무작위로 변경하는 간단한 커스텀 디렉티브를 등록하는 예제이다.

      import { createApp } from 'vue';

      const app = createApp();

      app.directive('random-bg-color', {
        mounted(el) {
          el.style.backgroundColor = getRandomColor();
          el.addEventListener('click', () => {
            el.style.backgroundColor = getRandomColor();
          });
        }
      });

      function getRandomColor() {
        return `#${Math.floor(Math.random() * 16777215).toString(16)}`;
      }

    이제 random-bg-color 라는 커스텀 디렉티브를 사용하여 요소에 바인딩할 수 있다.

      <div v-random-bg-color>Click me!</div>

    위의 예제에서 커스텀 디렉티브 요소가 마운트될 때 배경색을 설정하고, 요소가 클릭될 때마다 배경색을 무작위로 변경한다.

    app.directive()를 사용하여 커스텀 디렉티브를 등록하면 애플리케이션의 동작을 확장할 수 있다.
    이를 통해 재사용 가능한 로직을 디렉티브로 추상화하고, 컴포넌트 간에 쉽게 공유할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  이름(문자열)과 디렉티브 정의를 모두 전달하는 경우, 전역 커스텀 디렉티브를 등록한다. 이름만 전달되는 경우, 이미 등록된 것을 찾는다.

  타입

    interface App {
      directive(name: string): Directive | undefined
      directive(name: string, directive: Directive): this
    }

  예제

    import { createApp } from 'vue'

    const app = createApp({
      /* ... */
    })

    // 등록 (객체 디렉티브)
    app.directive('my-directive', {
      /* 커스텀 디렉티브 훅 */
    })

    // 등록 (간단하게 사용하기 위한 함수형 디렉티브)
    app.directive('my-directive', () => {
      /* ... */
    })

    // 등록된 디렉티브 찾기
    const myDirective = app.directive('my-directive')

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
