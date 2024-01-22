<template>
	<div></div>
</template>
<!--  
  
  템플릿 참조

    Vue의 선언적 렌더링 모델은 개발자가 직접 DOM에 접근해야 해야하는 대부분을 추상화하지만,
    개발자가 DOM 엘리먼트에 직접 접근해야 하는 경우가 여전히 있을 수 있다.
    이러한 필요성을 충족시키기 위해 ref라는 특별한 속성을 사용할 수 있다.

      <input ref="input">

    ref는 v-for 장에서 언급한 key 속성과 유사한 특수 속성이다. 이를 통해 마운트된 특정 DOM 엘리먼트 또는 자식 컴포넌트 인스턴스에 직접적인 참조를 얻을 수 있다.
    예를 들어 컴포넌트 마운트의 인풋에 초점을 맞추거나 엘리먼트에서 타사 라이브러리를 초기화하려는 경우, 프로그래밍 방식으로 조작하기 편리하다.

  ----------------------------------------------------------------------------------------------------------------------------

    ref로 접근하기

      Composition API를 사용하여 참조를 얻으려면, 템플릿 ref 속성 값과 일지하는 이름으로 ref를 선언해야 한다.

        <script setup>
        import { ref, onMounted } from 'vue'

        // 엘리먼트 참조를 위해 ref를 선언
        // 이름은 템플릿 ref 값과 일치해야 한다.
        const input = ref(null)

        onMounted(() => {
          input.value.focus()
        })
        </script>

        <template>
          <input ref="input" />
        </template>

      <script setup>을 사용하지 않는 경우, setup()에서 참조를 반환해야 한다.

        export default {
          setup() {
            const input = ref(null)
            // ...
            return {
              input
            }
          }
        }

      컴포넌트가 마운트된 이후에만 ref에 접근할 수 있다. 템플릿 표현식에서 input에 접근하려고 하면, 첫 번째 렌더링에서는 null이 된다.
      이는 요소가 첫 번째 렌더링 이후에야 존재하기 때문이다.

      템플릿 ref의 변경 사항을 감시하려는 경우 ref에 null 값이 있는 경우를 고려해야 한다.

        watchEffect(() => {
          if (input.value) {
            input.value.focus()
          }else {
            // v-if에 의해 아직 마운트 되지 않았거나, 마운트 해제된 경우
          }
        })

  ----------------------------------------------------------------------------------------------------------------------------

    v-for 내부에서 ref 사용하기(버전이 v3.2.25 이상)

      ref가 v-for 내부에서 사용되면, 해당 ref는 마운트 후 엘리먼트로 채워지므로 배열 값이어야 한다.

        <script setup>
        import { ref, onMounted } from 'vue'

        const list = ref([ ])

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
