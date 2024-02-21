<template>
	<div></div>
</template>
<!--  

  defineCustomElement()

    이 메서드는 defineComponent와 동일한 인자를 사용하지만, 네이티브 커스텀 엘리먼트 클래스 생성자를 반환한다.

    타입

      function defineCustomElement(
        component:
          | (ComponentOptions & { styles?: string[] })
          | ComponentOptions['setup']
      ): {
        new (props?: object): HTMLElement
      }

    세부 사항
    
      defineCustomElement()는 일반 컴포넌트 옵션 외에도 추가로 엘리먼트의 섀도우 루트에 삽입되어야 하는 CSS를 제공하기 위해, 
      인라인 CSS 문자열을 배열로 감싼 특수 옵션 styles도 지원한다.

      반환 값은 customElement.define()을 사용하여 등록할 수 있는 커스텀 엘리먼트 생성자이다.

    예제

      import { defineCustomElement } from 'vue'

      const MyVueElement = defineCustomElement({
        /* 컴포넌트 옵션 */
      })

      // 커스텀 엘리먼트를 등록함
      customElements.define('my-vue-element', MyVueElement)

    
    defineCustomElement() 함수는 Vue 3에서 사용되는 새로운 기능으로,
    사용자 정의 요소(custom element)를 정의하는 데 사용된다.
    이 함수를 사용하면 Vue 컴포넌트를 웹 표준의 사용자 정의 요소로 변환할 수 있다.

    사용자 정의 요소는 기존 HTML 요소(<dib>, <button>, <input> 등)와 동일한 방식으로 사용되지만,
    애플리케이션의 동작을 확장하기 위해 사용자가 정의한 새로운 요소이다.
    사용자 정의 요소는 일반적인 HTML요소와 동일한 방식으로 DOM에서 조작할 수 있으며, 다른 프레임워크나 라이브러리와 상호 작용할 수 있는 장점이 있다.

    defineCustomElement() 함수는 다음과 같은 형식을 가진다.

      import { defineCustomElement } from 'vue';

      const MyCustomElement = defineCustomElement({
        // 컴포넌트 옵션들
      });

    defineCustomElement() 함수를 호출할 때에는 Vue 컴포넌트의 옵션들을 포함하는 객체를 전달한다.
    이 객체는 Vue 컴포넌트와 동일한 구조를 가지며, 템플릿, 데이터, 메서드, 라이프사이클 훅 등을 정의할 수 있다.

    예를 들어, 다음은 defineCustomElement() 함수를 사용하여 사용자 정의 요소를 정의하는 예제이다.

      import { defineCustomElement } from 'vue';

      const MyCustomElement = defineCustomElement({
        data() {
          return {
            message: 'Hello, Vue!'
          };
        },
        template: `
          <div>
            <p>{{ message }}</p>
          </div>
        `,
        methods: {
          greet() {
            console.log('Hello from custom element!');
          }
        }
      });

      customElements.define('my-custom-element', MyCustomElement);

    위의 예제에서는 defineCustomElement() 함수를 사용하여 간단한 사용자 정의 요소인 'my-custom-element'를 정의하고 있다.
    이 요소는 HTML에서 일반적인 사용자 정의 요소처럼 사용할 수 있으며, Vue 컴포넌트의 기능을 활용할 수 있다.
    마지막으로 customElement.define() 함수를 사용하여 사용자 정의 요소를 등록한다.
    이제 HTML에서 <my-custom-element>를 사용하여 해당 컴포넌트를 사용할 수 있다.
    


-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
