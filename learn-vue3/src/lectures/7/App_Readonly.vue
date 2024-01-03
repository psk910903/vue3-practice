<template>
  <div>

  </div>
</template>

<script>
import { reactive, readonly } from 'vue'
export default {
  setup () {
    const original = reactive({
      count: 0,
    })
    const copy = readonly(original)
    original.count++;
    copy.count++; //readonly로 생성된 객체는 값을 변경할 수 없다.
    console.log(original.count)
    console.log(copy.count)
    return {}
  }
}
//readonly를 이용하여 반응형 객체의 변경 방지
//때때로 반응형 객체(ref나 reactive)의 변화를 추적하기 원하지만, 또한 특정 부분에서는 변화를 막아야 한다.

</script>

<style lang="scss" scoped>

</style>