<template>
  <div>

  </div>
</template>

<script>
import { reactive, ref } from 'vue';
export default {
  setup () {
    //ref -> Object
    const count = ref(0);
    const state = reactive({
      // count: count, key와 value가 같으면 단축 속성명으로 생략 가능
      count
    })
    console.log(count.value) // 0
    console.log("state.count : ", state.count) //0
    console.log("state.count : ", state.count.value) //undefined

    //ref -> Array
    const message = ref('Hello');
    const arr = reactive([message])
    console.log('arr[0]', arr[0]) //반응형
    console.log('arr[0].value', arr[0].value) //Hello
    return {
    }
  }
}
</script>

<style lang="scss" scoped>

</style>