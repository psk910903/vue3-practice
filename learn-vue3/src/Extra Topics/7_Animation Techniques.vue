<template>
	<div></div>
</template>
<!--  

  애니메이션 기법

    Vue는 진입/퇴장 및 목록 전환을 처리하기 위한 <Transition> 및 <TransitionGroup> 컴포넌트를 제공한다.
    그러나 웹에서 애니메이션을 사용하는 다른 많은 방법들이 있으며, 심지어 Vue 애플리케이션에서도 마찬가지이다.
    여기서는 몇 가지 추가 기술에 대해 설명하겠다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    클래스 기반 애니메이션

      DOM에 진입/진출하지 않는 엘리먼트의 경우, CSS 클래스를 동적으로 추가하여 애니메이션을 트리거할 수 있다.

        //js
        const disabled = ref(false)

        function warnDisabled() {
          disabled.value = true
          setTimeout(() => {
            disabled.value = false
          }, 1500)
        }

        //template
        <div :class="{ shake: disabled }">
          <button @click="warnDisabled">Click me</button>
          <span v-if="disabled">This feature is disabled!</span>
        </div>

        //css
        .shake {
          animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
          transform: translate3d(0, 0, 0);
        }

        @keyframes shake {
          10%,
          90% {
            transform: translate3d(-1px, 0, 0);
          }

          20%,
          80% {
            transform: translate3d(2px, 0, 0);
          }

          30%,
          50%,
          70% {
            transform: translate3d(-4px, 0, 0);
          }

          40%,
          60% {
            transform: translate3d(4px, 0, 0);
          }
        }

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    상태 기반 애니메이션

      예를 들어 상호 작용이 발생하는 동안 엘리먼트에 스타일을 바인딩하여 값을 보간하여 일부 트랜지션 효과를 적용할 수 있다.
      예를 들면 다음과 같다.

        //js
        const x = ref(0)

        function onMousemove(e) {
          x.value = e.clientX
        }

        //template
        <div
          @mousemove="onMousemove"
          :style="{ backgroundColor: `hsl(${x}, 80%, 50%)` }"
          class="movearea"
        >
          <p>Move your mouse across this div...</p>
          <p>x: {{ x }}</p>
        </div>

        //css
        .movearea {
          transition: 0.3s background-color ease;
        }

      색상 외에도 스타일 바인딩을 사용하여 transform, width 또는 height에 애니메이션을 적용할 수도 있다.
      스프링 물리학을 사용하여 SVG path에 애니메이션할 수도 있다.
      결국 모두 속성 데이터 바인딩이다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    감시자로 애니메이션 만들기

      약간의 창의성으로 우리는 감시자를 사용하여 수치 상태를 기반으로 무엇이든 애니메이션할 수 있다.
      예를 들어 숫자 자체에 애니메이션을 적용할 수 있다.

        //js
        import { ref, reactive, watch } from 'vue'
        import gsap from 'gsap'

        const number = ref(0)
        const tweened = reactive({
          number: 0
        })

        watch(number, (n) => {
          gsap.to(tweened, { duration: 0.5, number: Number(n) || 0 })
        })

        //template
        Type a number: <input v-model.number="number" />
        <p>{{ tweened.number.toFixed(0) }}</p>


-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
