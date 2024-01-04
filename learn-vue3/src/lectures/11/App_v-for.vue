<template>
  <div>
    <!-- v-for 디렉티브를 사용하면 배열인 목록을 렌더링 할 수 있다. -->
    <li v-for="(item) in items" :key="item.id">
      {{ item.message }}
    </li>
    <!--
      v-for="item in items" 문법을 사용해서 배열에서 항목을 순차적으로 할당한다. 
      v-for="(item, index) in items" 문법을 사용해서 배열 인덱스를 가져올 수 있다.
      항목을 나열할 때 각 :key 속성에는 고유한 값을 지정해야 한다.(vue2.2.0부터 필수)
    -->
    <hr>
    <!-- v-for를 사용하여 객체의 속성을 반복할 수도 있다. -->
    <li v-for="(value, key, index) in myObject" :key="key">
      {{ key }} - {{ value }} - {{ index }}
    </li>
    <hr>
    <!-- v-if 와 v-for를 함께 쓰면 안되기 때문에 <template>으로 감싸준다 -->
    <template v-for="(item, index) in items" :key="item.id">
      <li v-if="index % 2 == 0">
        {{ index }}
      </li>
    </template>
    <hr>
    <!-- v-if 와 v-for를 함께 쓰면 안되기 때문에 computed 사용 -->
    <li v-for="(item) in evenItems" :key="item.id">
      {{ item.id }} - 
      {{ item.message }}
    </li>
  </div>
</template>

<script>
import { reactive, computed } from 'vue'
export default {
  setup () {
    const items = reactive([
      {id:1, message: "Java"},
      {id:2, message: "Html"},
      {id:3, message: "CSS"},
      {id:4, message: "JavaScript"},
    ])
    
    const myObject = reactive({
      title: "title",
      author: "홍길동",
      publishedAt: "2024-01-04"
    })

    // v-if 와 v-for를 함께 쓰면 안되기 때문에 computed 사용
    const evenItems = computed(() => items.filter(item => item.id % 2 == 0))
    return {
      items,
      myObject,
      evenItems,
    }
  }
}
</script>

<style lang="scss" scoped>

</style>