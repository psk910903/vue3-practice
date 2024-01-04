<template>
  <div>
    <p>author: {{ author }}</p>
    <p>title: {{ title }}</p>
  </div>
</template>

<script>
import { reactive, toRef } from 'vue'
export default {
  setup () {
    const book = reactive({
      author: 'Vue Team',
      year: '2020',
      title: 'Vue 3 Guide',
      description: '당신은 지금 바로 이 책을 읽습니다.',
      price: '무료',
    })

    // const { author, title } = book
    // console.log(typeof author) //string
    // console.log(typeof title) //string
    // 그냥 구조분해할당하면 반응형을 읽어버림

    //여러개 반응형 구조분해할당
    // const { author, title } = toRefs(book)

    //한개만 반응형 구조분해할당 한다고 하면 toRef()를 사용하고 두번째 매개변수로 key 값 입력
    const author = toRef(book, 'author')
    const title = toRef(book, 'title')
    return {
      author,
      title,
      book,
    }
  }
}
//반응형 상태 구조분해(Destructuring)
//큰 반응형 객체의 몇몇 속성을 사용하길 원할 때, 원하는 속성을 얻기 위해 ES6 구조 분해 할당을 사용하는 것은 매우 일반적이다.
//하지만 그러한 구조 분해로 두 속성은 반응형을 잃게 된다.
//이런 경우, 반응형 객체를 일련의 ref 들로 변환해야 한다.
//이러한 ref 들은 소스 객체에 대한 반응형 연결을 유지한다.

//toRefs, toRef 를 사용하면 반응형 객체의 속성과 동기화 된다. 
//그래서 원본 속성을 변경하면 ref 객체가 업데이트되고 그 반대의 경우도 마찬가지이다.
</script>

<style lang="scss" scoped>

</style>