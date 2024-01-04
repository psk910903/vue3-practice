<template>
  <div>
    <!-- v-if 디렉티브는 조건부로 블록을 텐더링 할 때 사용한다. -->
    <h1 v-if="visible">Hello Vue3</h1>

    <!-- v-else 디렉티브는 v-if가 거짓(false)일 때 렌더링 하는 블록이다. -->
    <h1 v-else>Good bye!</h1>
    <button @click="visible = !visible">toggle</button>

    <hr>
    <!-- v-else-if는 이름에서 알 수 있듯이 v-if에 대한 'else-if 블록'이다. 여러 조건을 연결할 수 있다. -->
    <h1 v-if="type === 'A'">A</h1>
    <h1 v-else-if="type === 'B'">B</h1>
    <h1 v-else-if="type === 'C'">C</h1>
    <h1 v-else>Not A/B/C</h1>
    <button @click="type = 'A'">A</button>
    <button @click="type = 'B'">B</button>
    <button @click="type = 'C'">C</button>
    <button @click="type = 'D'">D</button>
    <hr>
    <!-- 여러개의 HTML 요소를 v-if 디렉티브로 연결하고 싶다면 <template>을 사용할 수 있다. -->
    <template v-if="visible">
      <h1>Title</h1>
      <p>Paragraph 1</p>
      <p>Paragraph 2</p>
    </template>
    <hr>
    <!-- 요소를 조건부로 표시하는 또 다른 옵션은 v-show 디렉티브이다. -->
    <h1 v-show="show">Title</h1>
    <button @click="show = !show">toggle show</button>

    <!-- 
      v-if vs v-show

      v-if는 "실제(real)"로 렌더링된다. 전환 할 때 블록 내부의 컴포넌트들이 제거되고 다시 생성되기 때문이다.
      또한 v-if는 게으르다(lazy). 초기 렌더링 시, 조건이 거짓(false)이면 아무 작업도 하지 않는다.
      조건부 블록은 조건이 처음으로 참(true)이 될 때까지 렌더링되지 않는다.

      이에 비해 v-show는 훨씬 간단하다. 엘리먼트는 css 기반 전환으로 초기 조건과 관계 없이 항상 렌더링된다.
      (역자 주: v-show는 엘리먼트를 DOM에 우선 렌더링하고 조건에 따라 CSS display:block/display:none 속성을 전환한다)

      일반적으로 v-if는 전환 비용이 높은 반면, v-show는 초기 렌더링 비용이 높다.
      그러므로 무언가를 자주 전환해야 한다면 v-show를 사용하는게 좋고, 런타임 시 조건이 변경되지 않는다면 v-if를 사용하는게 더 좋다.
    -->

    <!--  
      v-if 와 v-for

      v-if와 v-for를 함께 쓰는 것을 권장하지 않는다.
      동일한 엘리먼트에 v-if와 v-for를 함께 사용할 때, v-if가 더 높은 우선순위를 갖는다.
    -->
  </div>
</template>

<script>
import { ref } from 'vue'
export default {
  setup () {
    const show = ref(true)
    const visible = ref(true)
    const type = ref("A")

    return {
      type,
      visible,
      show,
    }
  }
}
</script>

<style lang="scss" scoped>

</style>