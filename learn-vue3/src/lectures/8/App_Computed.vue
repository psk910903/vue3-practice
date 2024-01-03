<template>
  <div>
    <h2>{{ teacher.name }}</h2>
    <h3>강의가 있습니까?</h3>
    <p>{{ teacher.lectures.length > 0 ? '있음' : '없음' }}</p>
    <p>{{ hasLecture }}</p>
    <p>{{ existLecture() }}</p>
    <button @click="counter++">Counter: {{ counter }}</button>
    <p>{{ fullName }}</p>
  </div>
</template>

<script>
import { computed, reactive, ref } from 'vue'
export default {
  setup () {
    const teacher = reactive({
      name: '박선교',
      lectures: ['HTML/CSS', 'JavaScript', 'Vue3']
    });

    const hasLecture = computed( () =>  teacher.lectures.length > 0 ? '있음' : '없음');
    const existLecture = () =>  teacher.lectures.length > 0 ? '있음' : '없음';
    //위 두가지 방법 모두 결과값은 같지만 computed가 성능면에서 비용이 적게 든다.
    //computed는 결과 값이 캐시되기 때문
    //<template> 구간에서 <p>{{ hasLecture }}</p> 와 <p>{{ existLecture() }}</p>를 여러번 사용한다고 했을 때
    //computed의 반환값은 캐시하고 있다가 다음 요청이오면 캐시된 데이터를 반환해준다.
    //method는 요청이 올 때마다 재실행한다.
    //computed의 캐시된 값이 다시 계산되는 시점은 computed 안의 반응형 데이터가 변경되는 시점이다.

    const counter = ref(0)
    
    //Writable Computed
    //Computed는 기본적으로 getter 전용이다. 계산된 속성에 새 값을 할당하려고 하면 런타임 경고가 표시된다.
    //새로운 계산된 속성이 필요한 경우에 getter와 setter를 모두 제공하여 속성을 만들 수 있다.
    const firstName = ref('홍')
    const lastName = ref('길동')
    // const fullName = computed(() => {
    //   return firstName.value + " " + lastName.value
    // })
    // console.log('console 출력', fullName.value)
    // fullName.value = '박 선교'; //console 창에 경고표시
    
    const fullName = computed({
      get() {
        return firstName.value + " " + lastName.value
      },
      set(value){
        [firstName.value, lastName.value] = value.split(" ");
      }
    })
    fullName.value = '박 선교';

    return {
      teacher,
      hasLecture,
      existLecture,
      counter,
      fullName,
    }
  }
}
//Computed
//템플릿 문법 {{ }}은 간단히 사용하면 매우 편리하다.
//하지만 템플릿 표현식 내 코드가 길어질 경우 가독성이 떨어지고 유지보수가 어려워질 수 있다.
//템플릿 표현식 내 코드가 길어질 경우 템플릿 표현식은 복잡해지며 선언적이지 않다.
//또 이러한 코드를 여러곳에서 반복적으로 사용해야한다면 더더욱 비효율 적일 것이다.
//이럴 때 사용하는 것이 계산된 속성(computed property)이다.
//computed안에 callback 함수를 정의함으로써 반환된 값을 읽기 전용으로 사용할 수 있다.

</script>

<style lang="scss" scoped>

</style>