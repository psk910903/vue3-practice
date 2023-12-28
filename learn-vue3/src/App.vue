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