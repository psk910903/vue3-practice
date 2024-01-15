<template>
	<div></div>
</template>
<!--  
  vue란
    Vue(view와 같은 발음)는 사용자 인터페이스를 구축하기 위한 JavaScript 프레임워크이다.
    표준 HTML, CSS 및 JavaScript를 기반으로 구축되며, 단순한 것 부터 복잡한 것 까지 
    사용자 인터페이스를 효율적으로 개발할 수 있는 컴포넌트 기반 프로그래밍 모델을 제공한다.

    import { createApp, ref } from 'vue'

    createApp({
      setup() {
        return {
          count: ref(0)
        }
      }
    }).mount('#app')

    <div id="app">
      <button @click="count++">
        숫자 세기: {{ count }}
      </button>
    </div>

    위의 예는 Vue의 두 가지 핵심 기능을 보여준다.
      1. 선언적 렌더링(Declarative Rendering)
        Vue는 표준 HTML을 템플릿 문법으로 확장하여 JavaScript 상태(state)를 기반으로 화면에 출력될 HTML을 선언적(declaratively)으로 작성할 수 있다.
      2. 반응성(Reactivity)
        Vue는 JavaScript 상태(State) 변경을 추적하고, 변경이 발생하면 DOM을 효율적으로 업데이트하는 것을 자동으로 수행한다.
    
  ----------------------------------------------------------------------------------------------------------------------------

  프로그레시브 프레임워크

    Vue는 프론트엔드 개발에 필요한 대부분의 공통 기능을 다루는 프레임워크이자 생태계이다.
    그러나 웹은 매우 다양해 구축하려는 것의 형태와 규모가 크게 다를 수 있다. 이를 염두해 두고 Vue는 유연하고 점진적으로 채택할 수 있도록 설계되었다.
    사용 사례에 따라 Vue를 다양한 방식으로 사용할 수 있다.

    - 빌드 과정 없이 정적 HTML에 적용
    - 모든 페이지에 웹 컴포넌트로 추가
    - 싱글 페이지 어플리케이션(SPA: Single-Page Application)
    - Fullstack / 서버 사이드 렌더링(SSR: Server-Side Rendering)
    - Jamstack / 정적 사이트 생성(SSG: Static-Site-Generation)
    - 데스크톱, 모바일, WebGL 또는 터미널을 대상으로 하는 경우

    이러한 유연성에도 불구하고 Vue 작동 방식에 대한 핵심 지식은 이러한 모든 사용 사례에서 공유된다.
    비록 지금은 초보자일지라도 미래에 더 야심 찬 목표를 달성하기 위해 성장함에 따라 그 과정에서 얻은 지식은 계속 유용할 것이다.
    베테랑이라면 해결하려는 문제를 기반으로 Vue를 활용하는 최적의 방법을 선택하면서 같은 생산성을 유지할 수 있다.
    이러한 이유로 Vue를 "프로그레시브 프레임워크(Progressive Framework)"라고 부른다.
    이것은 나와 함께 성장하고 나의 요구에 적응할 수 있는 프레임워크이다.

  ----------------------------------------------------------------------------------------------------------------------------

  싱글 파일 컴포넌트
  
    빌드 도구를 사용하는 대부분의 Vue 프로젝트에서는 HTML과 유사한 싱글 파일 컴포넌트(Single-File Component: SFC, *.vue 파일이라고도 함)라는 파일 형식을 사용하여 
    Vue 컴포넌트를 작성한다. Vue SFC는 이름에서도 알 수 있듯이 컴포넌트의 논리(JavaScript), 템플릿(HTML) 및 스타일(CSS)을 하나의 파일에 캡슐화한다.
    이전에 보았던 예제는 다음과 같이 SFC 형식으로 작성할 수 있다.

    <script setup>
    import { ref } from 'vue'
    const count = ref(0)
    </script>

    <template>
      <button @click="count++">Count is: {{ count }}</button>
    </template>

    <style scoped>
    button {
      font-weight: bold;
    }
    </style>

    SFC는 Vue를 빌드 방식으로 사용하는 경우, 컴포넌트를 만들고 정의하는데 권장되는 방법이다.
    지금은 Vue가 모든 빌두 도구 설정을 처리한다는 점만 알고있자.

  ----------------------------------------------------------------------------------------------------------------------------

  API 스타일

    Vue 컴포넌트는 옵션(Options) API와 컴포지션(Composition) API 두 가지 스타일로 작성할 수 있다.

    1. 옵션 API
      옵션 API를 사용하여 옵션의 data, methods 및 mounted 같은 객체를 사용하여 컴포넌트의 로직을 정의한다.
      옵션으로 정의된 속성은 컴포넌트 인스턴스를 가리키는 함수 내부의 this에 노출된다.
      <script>
      export default {
        // data()에서 반환된 속성들은 반응적인 상태가 되어
        // `this`에 노출된다.
        data() {
          return {
            count: 0
          }
        },

        // methods는 속성 값을 변경하고 업데이트 할 수 있는 함수.
        // 템플릿 내에서 이벤트 헨들러로 바인딩 될 수 있음.
        methods: {
          increment() {
            this.count++
          }
        },

        // 생명주기 훅(Lifecycle hooks)은 컴포넌트 생명주기의
        // 여러 단계에서 호출된다.
        // 이 함수는 컴포넌트가 마운트 된 후 호출된다.
        mounted() {
          console.log(`숫자 세기의 초기값은 ${ this.count } 입니다.`)
        }
      }
      </script>

      <template>
        <button @click="increment">숫자 세기: {{ count }}</button>
      </template>

    2. 컴포지션 API
      컴포지션 API를 사용하는 경우, import해서 가져온 API 함수들을 사용하여 컴포넌트의 로직을 정의한다.
      SFC에서 컴포지션 API는 일반적으로 <script setup>과 함께 사용된다.
      setup 속성은 Vue가 더 적은 코드 문맥으로 컴포지션 API를 사용하고, 컴파일 시 의도한대로 올바르게 동작할 수 있게 코드를 변환하도록 하는 힌트이다.
      예를 들어 <scrtip setup>에 import 되어 가져온 객체들과 선언된 최상위 변수 및 함수는 템플릿에서 직접 사용할 수 있다.

      아래 예제는 위 예제와 완전히 동일한 템플릿을 사용하는 컴포넌트 이지만, 
      Composition API와 <script setup>을 사용하였다.

      <script setup>
      import { ref, onMounted } from 'vue'

      // 반응적인 상태의 속성
      const count = ref(0)

      // 속성 값을 변경하고 업데이트 할 수 있는 함수.
      function increment() {
        count.value++
      }

      // 생명 주기 훅
      onMounted(() => {
        console.log(`숫자 세기의 초기값은 ${ count.value } 입니다.`)
      })
      </script>

      <template>
        <button @click="increment">숫자 세기: {{ count }}</button>
      </template>


      무엇을 선택해야 할까?
        두 API 스타일 모두 일반적인 사용 사례를 완벽하게 다룰 수 있다. 이것들은 정확히 동일한 기본 시스템에 의해 구동되는 서로 다른 인터페이스이다.
        사실, 옵션 API는 컴포지션 API 위에 구현된다. Vue에 대한 기본 개념과 지식은 두 스타일과 상관없이 동일하다.

        옵션 API는 일반적으로 OOP 언어 배경을 가진 사용자를 위한 클래스 기반 모델과 더 잘 맞는 "컴포넌트 인스턴스"(예제에서 볼 수 있는 this)의 개념을 중심으로 한다.
        또한 반응형 세부사항을 추상화하고 옵션 그룹을 통해 코드 구조를 실행하여 초보자에게 더 친숙하다.

        컴포지션 API는 함수 범위에서 직접 반응형 변수를 선언하고 복잡성을 처리하기 위해 여러 함수의 상태를 함께 구성하는데 중점을 둔다.
        보다 자유로운 형식이며 Vue에서 반응형이 효과적으로 사용되는 방식에 대한 이해가 필요하다.
        그 대가로 이 유연성은 로직을 구성하고 재사용하기 위한 보다 강력한 패턴을 가능하게 한다.

        Vue를 처음 사용하는 경우 일반적인 권장 사항
        - 학습을 목적으로 하는 경우, 내가 쉽게 이해할 수 있어보이는 스타일로 가야한다.
          다시 말하지만, 대부분의 핵심 개념은 두 스타일 간에 공유된다. 나중에 언제든지 다른 스타일을 선택할 수 있다.
        - 재품용(production)으로 사용하는 경우
          - 빌드 도구를 사용하지 않거나 Vue를 주로 복잡성이 낮은 시나리오에서 사용할 계획이라면 옵션 API를 사용해라.
          - Vue로 규모가 있는 앱의 전체를 구축하려는 경우 컴포지션 API + 단일파일 컴포넌트(SFC)를 사용해라

        학습 단계에서 한 가지 스타일만 고집할 필요는 없다. 

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
