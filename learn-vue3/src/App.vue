<template>
  <div>
    <p v-on:click="incrment">
      {{ counter }}
    </p>
    <p>
      {{ message }}
    </p>
  </div>
</template>

<script>
import { ref } from 'vue'
  export default {
    //setup() 함수(hook)는 Composition API 사용을 위한 진입점 역할을 하고, 
    //컴포넌트 인스턴스가 생성되기 전에 실행됨
    //반응형 API(Reactive API)를 사용하여 반응형 상태를 선언하고
    //setup()에서 객체를 반환하여 <template>에 노출할 수 있다.
    //반환된 객체의 속성은 구성 요소 인스턴스에서도 사용할 수 있다.

    //Props 접근
    //setup 함수의 첫번째 매개변수는 props이다. props는 반응형 객체이다.

    //Setup Context
    //setup 함수의 두번째 매개변수는 Setup Context 객체이다.
    //컨텍스트 객체는 setup 함수내에서 유용하게 사용할 수 있는 속성을 가지고 있다.
    //context.attrs : 속성($attrs와 동일한 비반응형 객체)
    //context.slots : 슬롯($slots에 해당하는 비반응형 객체)
    //context.emit : 이벤트 발생($emit에 해당하는 함수)
    //context.expose : Public한 속성, 함수를 외부에 노출시 사용

    setup() {
      const counter = ref(0)
      const message = ref('Hello Veu3')
      const incrment = () => {
        counter.value++ 
      }
      return {
        counter,
        message,
        incrment
      }
    }
  }
</script>

<style lang="scss" scoped>

</style>