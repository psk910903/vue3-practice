<template>
  <div>
    <div :style="styleObject">
      Lorem ipsum dolor sit amet consectetur, adipisicing elit. Quia magni amet temporibus. Perferendis, suscipit quos repellat necessitatibus molestias, inventore perspiciatis nulla incidunt dolor odio quidem cumque impedit beatae aut? Totam.
    </div>
    <br>
    <button @click="fontSize--">-</button>
    <button @click="fontSize++">+</button>
    <div :style="styleObject2">
      Lorem ipsum dolor sit amet consectetur, adipisicing elit. Quia magni amet temporibus. Perferendis, suscipit quos repellat necessitatibus molestias, inventore perspiciatis nulla incidunt dolor odio quidem cumque impedit beatae aut? Totam.
    </div>
  </div>
  
</template>

<script>
import { reactive, computed, ref } from 'vue'
export default {
  setup () {
    const styleObject = reactive({
      color: 'red',
      fontSize: "13px"
    })

    const fontSize = ref(13)
    const styleObject2 = computed(() => {
      return{
        color: 'green',
        fontSize: fontSize.value + "px"
      }
    })
    return {
      styleObject,
      styleObject2,
      fontSize,
    }
  }
}
</script>

<style lang="scss" scoped>

</style>