<template>
  <div>

  </div>
</template>

<script>

import { ref, watch, computed } from 'vue'
export default {
  setup () {   
    //immediate 즉시실행
    //immediate 옵션을 사용하여 최초에 즉시실행 할 수 있다.
    const message = ref('Hello World')
    const reversMessage = ref('')
    watch(message, (newValue) => {
      reversMessage.value = newValue.split('').reverse().join('')
    },{immediate: true})
    //또는 함수를 외부에서 선언하여 즉시실행 할 수 있음.(WatchEffect로 하면 더 단순화 할 수 있음)
    const message3 = ref('Hello World')
    const reverseFn = () => {
      reversMessage.value = message3.value.split('').reverse().join('')
    }
    watch(message3, reverseFn)
    //즉시실행
    reverseFn()


    /* eslint-disable no-unused-vars */
    //Computed vs Watch 공식문서
    //computed와 watch 둘 다 비슷한 역할을 하고 있다.
    //computed 
    const reversMessage2 = computed( () => 
      message.value.split('').reverse().join('')
    )

    // watch
    watch(message, (newValue) => {reversMessage.value = newValue.split('').reverse().join('')})

    //어떻게 사용할 것인가.
    //computed
    //Vue 인스턴스의 상태(ref, reactive 변수)의 종속 관계를 자동으로 세팅하고자 할 때는 computed로 구현하는 것이 좋다.
    //위 예시처럼 reverMessage2는 message 값에 따라 결정되어지는 종속관계에 있다.
    //이 종속관계 코드가 복잡해지면 watch로 구현할 경우 더 복잡해지거나 중복계산 또는 오류를 발생시킬 수 있다.

    //watch
    //Vue 인스턴스의 상태(ref, reactive 변수)의 변경 시점에 특정 액션(call api, push route 등)을 취하고자 할 때 적합하다.
    //대게의 경우 computed로 구현 가능한 것이라면 watch가 아니라 computed로 구현하는게 대부분 옳다.


    //WatchEffect
    //WatchEffect는 콜백 함수 안의 반응성 데이터에 변화가 감지되면 자동으로 반응하여 실행한다.
    //그리고 WatchEffect 코드는 컴포넌트가 생성될 때 즉시 실행된다.

    //watch vs watchEffect
    //watch 와 watchEffect 둘 다 관련 작업(api call, push route 등)을 반응적으로 수행할 수 있게 해준다.
    //하지만 주요한 차이점은 관련된 반응형 데이터를 추적하는 방식이다.
    //watch : 명시적으로 관찰된 소스만 추적한다. 콜백 데이터 내에서 액세스한 항목은 추적하지 않는다.
    //        또한 콜백은 소스가 실제로 변경된 경우에만 트리거된다. watch 종속성 추적을 부작용과 분리하여 콜백이 실행되어야 하는 시기를 보다 정확하게 제어할 수 있다.
    //watchEffect : 반면에 종속성 추적과 부작용을 한 단계로 결합한다. 동기 실행 중에 액세스되는 모든 반응 속성을 자동으로 추적한다.
    //              이것은 더 편리하고 일반적으로 더 간결한 코드를 생서하지만 반응성 종속성을 덜 명시적으로 만든다.
    return {}
  }
}
</script>

<style lang="scss" scoped>

</style>