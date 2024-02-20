<template>
	<div></div>
</template>
<!--  

  defineComponent()

    타입 추론으로 Vue 컴포넌트를 정의하기 위한 타입 헬퍼이다.

    타입

      // options 문법
      function defineComponent(
        component: ComponentOptions
      ): ComponentConstructor

      // function 문법 (requires 3.3+)
      function defineComponent(
        setup: ComponentOptions['setup'],
        extraOptions?: ComponentOptions
      ): () => any

    (타입은 가독성을 위해 단순화되었다.)

    세부사항

      첫 번째 인자로 컴포넌트 옵션 객체가 필요하다. 이 함수는 본질적으로 타입 추론 목적이므로,
      동일한 옵션 객체를 반환하며, 런타임에 작동하지 않는다.

      반환된 타입은 약간 특별한데, 옵션을 기반으로 추론된 컴포넌트 인스턴스 타입인 생성자 타입이 된다.
      이것은 반환된 타입이 TSX에서 태그로 사용될 때 타입 추론에 사용된다.

      다음과 같이 defineComponent()의 반환 타입에서 컴포넌트의 인스턴스 타입(해당 옵션의 this 타입과 동일)을 추출할 수 있다.

        const Foo = defineComponent(/* ... */)

        type FooInstance = InstanceType<typeof Foo>

    Note on webpack Treeshaking

      defineComponent()는 함수 호출이기 때문에 웹팩과 같은 일부 빌드 도구에 부작용(side-effect)을 일으키는 것처럼 보일 수 있다.
      이렇게 하면 컴포넌트가 전혀 사용되지 않는 경우에도 트리가 흔들리는 것을 방지할 수 있다.

      이 함수 호출이 트리 셰이크해도 안전하다는 것을 웹팩에 알리려면 함수 호출 앞에 /*#__PURE__*/ 주석 표기법을 추가하면 된다.

        export default /*#__PURE__*/ defineComponent(/* ... */)

      롤업(Vite에서 사용하는 기본 프로덕션 번들러)은 수동 어노테이션 없이도 defineComponent()가 실제로 부작용이 없는지 판단할 수 있을 만큼 똑똑하기 때문에
      Vite를 사용하려는 경우 이 작업이 필요하지 않다.


    defineComponent() 함수는 Vue 3에서 컴포넌트를 정의하는 데 사용되는 함수이다.
    Vue 2에서는 객체 리터럴로 컴포넌트를 정의했지만, Vue 3에서는 defineComponent() 함수를 사용하여 더욱 명시적으로 컴포넌트를 정의한다.

    defineComponent() 함수는 다음과 같은 형식을 가진다.

      import { defineComponent } from 'vue';

      const MyComponent = defineComponent({
        // 컴포넌트 옵션들
      });

    defineComponent() 함수를 호출할 때에는 컴포넌트의 옵션을 포함하는 객체를 전달한다.
    이 객체는 Vue 컴포넌트의 옵션과 동일한 구조를 가지며, 템플릿, 데이터, 메서드, 라이프사이클 훅 등을 정의할 수 있다.

    defineComponent() 함수를 사용하여 정의된 컴포넌트는 Vue의 createApp() 함수를 통해 생성된 Vue 애플리케이션에 등록할 수 있다.

    예를 들어, 다음은 defineComponent() 함수를 사용하여 간단한 Vue 컴포넌트를 정의하는 예제이다.

      import { defineComponent } from 'vue';

      const HelloWorld = defineComponent({
        template: `
          <div>
            <h1>{{ greeting }}</h1>
            <button @click="updateGreeting">Update Greeting</button>
          </div>
        `,
        data() {
          return {
            greeting: 'Hello, Vue!'
          };
        },
        methods: {
          updateGreeting() {
            this.greeting = 'Hello, Vue 3!';
          }
        }
      });

      export default HelloWorld;

    위의 예제에서는 defineComponent() 함수를 사용하여 간단한 HelloWorld 컴포넌트를 정의하고 있다.
    컴포넌트 옵션에는 템플릿, 데이터 및 메서드가 포함되어 있다.
    이렇게 정의된 컴포넌트는 다른 파일에서 import하여 Vue 애플리케이션에 등록할 수 있다.

    defineComponent() 함수를 사용하면 컴포넌트를 보다 명시적으로 정의할 수 있으며, 타입 검사와 같은 추가적인 이점을 얻을 수 있다.

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
