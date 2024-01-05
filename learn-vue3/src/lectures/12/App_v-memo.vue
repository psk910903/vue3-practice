<template>
  <div>
    <!-- 
      v-once는 최초 1번만 랜더링 후 반응성이 사라지지만
      v-memo는 배열 안에 반응형데이터를 넣어주면 값이 변경될 때 해당 데이터만 재 랜더링 된다.
      공식문서에 따르면 일반적으로 계속 사용하는 기능은 아니고,
      성능 최적화 할 때 주로 사용하면 된다.
      일반적인 예로 v-for로 랜더링할 때 그 목록의 길이가 1000개 이상일 때 사용한다.
    -->
    <div v-memo="[subscribes]">
      <p>subscribes : {{ subscribes }}</p>
      <p>views : {{ views }}</p>
      <p>likes : {{ likes }}</p>
    </div>
    <button @click="subscribes++">Sub++</button>
    <button @click="views++">Views++</button>
    <button @click="likes++">Likes++</button>
  </div>
</template>

<script>
import { ref } from 'vue'
export default {
  setup () {
    const subscribes = ref(4000)
    const views = ref(400)
    const likes = ref(20)

    return {subscribes, views, likes}
  }
}
</script>

<style lang="scss" scoped>

</style>