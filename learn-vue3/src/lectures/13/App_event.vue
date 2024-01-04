<template>
  <div>
    <!-- 
      이벤트 처리
      이벤트 처리는 v-on 디렉티브로 사용할 수 있다. @ 단축표현으로 사용 가능함 
    -->
    <div @click="counter++">counter : {{ counter }}</div>
    <hr>
    <!-- 
      메소드 이벤트 핸들러
      v-on 디렉티브에서 메소드 호출 가능함. 이 때 매개변수로 event 객체를 받음 
    -->
    <div>
      <button @click="printEventInfo">printEventInfo</button>
    </div>
    <hr>
    <!-- 
      이벤트 객체 접근
      인라인 핸들링에서 event 객체에 접근할 수 있다. 접근하는 방법은 $event 키워드를 사용한다. 
    -->
    <button @click="printEventInfo2('hello world', $event)">inline event handler</button>
    <hr>
    <!--  
      이벤트 수식어
      우리는 이벤트를 조작할 때 이벤트 내부에서 event.preventDefault() 
      또는 event.stopPropagation() 메서드를 호출할 수 있다.
      메소드에서 이러한 메소드의 호출은 어렵지 않지만 메소드 안에서 비즈니스 외에 이러한 코드는 비효율적이다.

      이 문제를 해결하기 위해 Vue는 v-on 이벤트에 다양한 이벤트 수식어를 제공한다.
      .stop = e.stopPropagatiuon()
      .prevent = e.preventDefault()
      .capture = 캡처 모드를 사용할 때 이벤트 리스너를 사용 가능
      .self = 자기 자신만 호출할 수 있다. 즉, 타깃오소가 self일 때 발동
      .once = 해당 이벤트는 한 번만 실행됨
      .passive = 일반적으로 모바일 장치의 성능을 개선하기 위해 터치 이벤트 리스너와 함께 사용된다.
    -->

      <!-- 클릭 이벤트 전파가 중단됨 -->
      <a href="" @click.stop=""></a>

      <!-- 제출 이벤트가 페이지를 다시 로드하지 않음 -->
      <form action="" @submit.prevent=""></form>

      <!-- 수정자는 체이닝이 가능 -->
      <a href="" @click.stop.prevent=""></a>

      <!-- 단순히 수식어만 사용이 가능 -->
      <form action="" @submit.prevent=""></form>

      <!--  
        캡처 모드를 사용할 때 이벤트 리스너를 사용 가능
        즉, 내부 엘리먼트를 대상으로 하는 이벤트가 해당 엘리먼트에서 처리되기 전에 여기서 처리
      -->
      <div @click.capture="test"></div>

      <!-- event.target이 엘리먼트 자체인 경우만 트리거를 처리 -->
      <!-- 자식 엘리먼트에서는 처리되지 않음 -->
      <div @click.self="test"></div>

      <div @scroll.passive="test"></div>
      
      <hr>
      <!--  
        키 수식어
        키보드 이벤트를 수신할 때 종종 특정 키를 확인해야 하는 경우가 있다.
        그래서 Vue에서는 v-on 또는 @ 디렉티브에 키 수식어를 제공한다.

        .enter
        .tab
        .delete("Delete"와 "Backspace" 키 모두 수신)
        .esc
        .space
        .up
        .down
        .left
        .right
      -->
      <input type="text" @keyup.enter="test">

      <hr>
      <!--  
        시스템 키 수식어
        다음 수식어를 사용해 해당 키가 눌러진 경우에만 마우스 또는 키보드 이벤트 리스너를 트리거 할 수 있다.
        .ctrl
        .alt
        .shift
        .meta(Mac에서 meta는 command key, Window에서 meta는 window key)
      -->
      <!-- alt + enter -->
      <input type="text" @keyup.alt.enter="test">

      <!-- ctrl + enter -->
      <input type="text" @keyup.ctrl.enter="test">

      <!-- ctrl + click -->
      <input type="text" @click.ctrl="test">
      <hr>
      <!--  
        .exact 수식어
        정확한 조합이 눌려야하는 것을 요구한다
      -->
      <!-- 아래코드는 alt 또는 shift와 함께 눌렸을 때도 실행된다. -->
      <button @click.ctrl="test"></button>

      <!-- 아래코드는 ctrl키만 눌려져 있을 때 실행된다. -->
      <button @click.ctrl.exact="test"></button>

      <!-- 아래코드는 시스템 키가 눌리지 않은 상태인 경우에만 작동한다. -->
      <button @click.exact="test"></button>
      <hr>
      <!--  
        마우스 버튼 수식어
        .left
        .right
        .middle
      -->
  </div>
</template>

<script>
import { ref } from 'vue'
export default {
  setup () {
    const counter = ref(0)
    const printEventInfo = (e) => {
      console.log(e.target)
      console.log(e.target.tagName)
    }
    const printEventInfo2 = (message, e) => {
      console.log('message: ', message)
      console.log(e.target)
      console.log(e.target.tagName)
    }
    const test = () => {

    }
    return {
      counter,
      printEventInfo,
      printEventInfo2,
      test,
    }
  }
}
</script>

<style lang="scss" scoped>

</style>