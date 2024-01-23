<template>
	<div></div>
</template>
<!--  

  컴포넌트 이벤트

  ----------------------------------------------------------------------------------------------------------------------------

    이벤트 발신 및 수신하기

      컴포넌트는 내장 메서드 $emit을 사용하여 템플릿 표현식(예: v-on 핸들러에서)에서 직접 사용자 정의 이벤트를 발신할 수 있다.

      //MyComponent
      <button @click="$emit('someEvent')">클릭하기</button>

    그러면 부모는 v-on을 사용하여 수신할 수 있다.

      <MyComponent @some-event="callback" />

    .once 수식어는 컴포넌트 이벤트 리스너에서도 지원된다.

      <MyComponent @some-event.once="callback" />

    컴포넌트 및 props와 마찬가지로 이벤트 이름은 자동 대소문자 변환을 제공한다.
    우리는 camelCase 형식으로 이벤트를 발신했지만, 부모에서 kebab-case 표기로 리스너를 사용하여 이를 수신할 수 있다.
    props 케이싱과 마찬가지로 템플릿에서 kebab-case 형식의 이벤트 리스너를 사용하는 것이 좋다.

    TIP
      네이티브 DOM 이벤트와 달리 컴포넌트에서 발신되는 이벤트는 버블이 발생하지 않는다. 
      직접 자식 컴포넌트에서 발생하는 이벤트만 수신할 수 있다. 형제 또는 깊게 중첩된 컴포넌트 간에 통신이 필요한 경우
      외부 이벤트 버스 또는 글로벌 상태 관리 솔루션을 사용해야한다.

  ----------------------------------------------------------------------------------------------------------------------------

    이벤트 인자

      이벤트와 함께 특정 값을 내보내는 것이 때때로 유용하다. 예를 들어, <BlogPost> 컴포넌트가 텍스트를 얼마나 크게 확대할지 결정할 수 있다.
      이러한 경우에 $emit에 추가 인자를 전달하여 이 값을 제공할 수 있다.
      
        <button @click="$emit('increaseBy', 1)">
          Increase by 1
        </button>

      그런 다음 부모가 이벤트를 수신할 때 인라인 화살표 함수를 리스너로 사용할 수 있다.
      이를 통해 이벤트 인자에 접근할 수 있따.

        <MyButton @increase-by="(n) => count += n" />

      또는 이벤트 핸들러가 메서드인 경우

        <MyButton @increase-by="increaseCount" />

      그러면 인자 값이 해당 메서드의 첫 번째 파라미터로 전달된다.

        function increaseCount(n) {
          count.value += n
        }

      TIP
        $emit() 에서 이벤트 이름 뒤에 전달된 모든 추가 인자는 리스너로 전달된다.
        예를 들어, $emit("foo", 1, 2, 3)을 사용하면 리스너 함수는 세 개의 인자를 받는다.

  ----------------------------------------------------------------------------------------------------------------------------

    발신되는 이벤트 선언하기

      컴포넌트는 defineEmits() 메크로를 사용하여 명시적으로 발신할 이벤트를 선언할 수 있다.

        <script setup>
        defineEmits(['inFocus', 'submit'])
        </script>

      <template>에서 사용한 $emit 메서드는 컴포넌트의 <script setup> 섹션 내에서 접근할 수 없지만,
      defineEmits()는 $emit 대신에 사용할 수 있는 동등한 함수를 반환한다.

        <script setup>
        const emit = defineEmits(['inFocus', 'submit'])

        function buttonClick() {
          emit('submit')
        }
        </script>

      defineEmits() 메크로는 함수 내에서 사용할 수 없으므로, 위의 예제처럼 <script setup> 내에 직접 배치해야 한다.

      <script setup> 대신 명시적으로 setup 함수를 사용하는 경우, 
      이벤트는 emits 옵션을 사용하여 선언되어야 하며 emit 함수는 setup() 컨텍스트에 노출되어야 한다.

        export default {
          emits: ['inFocus', 'submit'],
          setup(props, ctx) {
            ctx.emit('submit')
          }
        }

      setup() 컨텍스트의 다른 속성과 마찬가지로 emit은 안전하게 분해할당할 수 있다.

        export default {
          emits: ['inFocus', 'submit'],
          setup(props, { emit }) {
            emit('submit')
          }
        }

      emits 옵션과 defineEmits() 매크로도 객체 구문을 지원한다. TypeScript를 사용하는 경우 인수의 유형을 지정할 수 있으며,
      이를 통해 발생한 이벤트의 페이로드를 런타임에서 유효성 검사할 수 있다.

        <script setup>
        const emit = defineEmits({
          submit(payload: { email: string, passward: string }){
            //true 또는 false 값을 반환하여
            //유효성 검사 통과/실패 여부를 알려줌

            //페이로드는 전달되는 인자를 나타내는 것으로
            //emit('submit', 'a', 'b', 'c')와 같이 3개의 인자를 전달하는 경우
            //submit(pl1, pl2, pl3) {유효성 검사 반환 로직}과 같이
            //유효성 검사에 페이로드를 사용할 수 있다.
          }
        })

      TypeScript를 <script setup>과 함께 사용하는 경우, 순수 타입스크립트 문법을 사용하여 발신되는 이벤트를 선언할 수도 있다.

        <script setup lang="ts">
        const emit = defineEmits<{
          (e: 'change', id: number): void
          (e: 'update', value: string): void
        }>()
        </script>

      선택 사항으로 컴포넌트가 작동하는 방식을 더 잘 문서화하기 위해 발신되는 모든 이벤트를 정의하는 것이 좋다.
      또한 상위로부터 전달된 리스너는 폴스루 속성에 의해 제외할 수 있다.

      TIP
        네이티브 이벤트(예: click)가 emits 옵션에 정의된 경우 리스너는 이제 컴포넌트에서 발생하는 click 이벤트만 수신 대기하고
        네이티브 click 이벤트에 더 이상 응답하지 않는다.

  ----------------------------------------------------------------------------------------------------------------------------

    이벤트 유효성 검사

      props 타입 유효성 검사와 유사하게, 발신되는 이벤트는 배열 대신 객체 구문으로 정의된 경우 유효성을 검사할 수 있다.

      유효성 검사를 추가하기 위해 이벤트에는 emit 호출 시 전달되는 인자를 수신하고, 이벤트가 유효한지 여부를 나타내는 불리언 값을 반환하는 함수가 할당된다.

        <script setup>
        const emit = defineEmits({
          // 유효성 검사 없음
          click: null,

          // submit 이벤트 유효성 검사
          submit: ({ email, password }) => {
            if (email && password) {
              return true
            } else {
              console.warn('submit 이벤트 페이로드가 옳지 않음!')
              return false
            }
          }
        })

        function submitForm(email, password) {
          emit('submit', { email, password })
        }
        </script>
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
