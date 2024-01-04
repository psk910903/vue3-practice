<template>
  <div>
    <!-- 클래스를 동적으로 바인딩 하기위해서는 :class(v-bind:class)를 사용할 수 있다. -->

    <div class="text" :class="{active: isActive, 'text-danger' : hasError}">텍스트 입니다.</div>
    <!-- text-danger : 가운데 특수문자가 있기 때문에 싱글코테이션으로 감쌌음 -->
    <!-- 
      위 예시처럼 v-bind:class 디렉티브는 일반 class 속성과 공존할 수 있다. (다른 속성들은 공존할 수 없음)
      그리고 객체를 반환하는 computed 에 바인딩 할 수도 있다.
    -->
    <div class="text" :class="classObject">classObject</div>
    <!-- 바인딩 할 데이터가 많다면 object로 바인딩 할 수 있다. -->
    <button @click="toggle">toggle</button>

    <!-- 배열로 사용 가능 -->
    <div class="text" :class="[activeClass, errorClass]">텍스트 입니다.</div>
    <button @click="toggleError">Error</button>

    <!-- 배열 안에 3항 연산자 가능 -->
    <div class="text" :class="[isActive ? 'active-class' : 'class', classObject]">배열 안의 3항 연산자</div>
  </div>
</template>

<script>
import { computed, ref } from 'vue'
export default {
  setup () {
    const isActive = ref(true)
    const hasError = ref(false)

    // const classObjectReactive = reactive({
    //   active: isActive.value ,
    //   'text-danger':  hasError.value
    // })

    //active되는 상태가 여러개가 필요하면 computed를 사용하는게 더 좋음
    const classObject = computed(() => {
      return{
        active: isActive.value && !hasError.value,
        'text-danger': !isActive.value && hasError.value,
        'text-blue:' : true
      }
    })
    const toggle = () => {
      isActive.value = !isActive.value
      hasError.value = !hasError.value
    }

    const activeClass = ref('active')
    const errorClass = ref('error')
    const toggleError = () => {
      errorClass.value = "text-danger"
    }
    return {
      classObject,
      toggle,
      isActive,
      hasError,
      activeClass,
      errorClass,
      toggleError,
      // classObjectReactive,
    }
  }
}

</script>

<style scoped>
  .active {
    font-weight: 900;
  }
  .text-danger{
    color: red
  }
  .active-class {
    color: green
  }
</style>