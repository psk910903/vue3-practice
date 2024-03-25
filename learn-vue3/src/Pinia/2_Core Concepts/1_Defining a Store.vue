<template>
	<div></div>
</template>
<!--  

  스토어 정의하기

    핵심 개념에 대해 알아보기 전에 스토어가 defineStore()를 사용해 정의되고, 고유한 이름이 첫 번째 인자로 전달되어야 한다는 것을 알아야 한다.

      import { defineStore } from 'pinia'

        // defineStore()의 반환 값(함수)을 할당할 변수의 이름을 원하는 대로 지정할 수 있지만,
        // 스토어 이름을 사용하고 use와 Store로 묶는 것이 가장 좋다.
        // 예: useUserStore, useCartStore, useProductStore
        // 첫 번째 인자는 앱 전체에서 스토어의 고유 ID이다.
        export const useAlertsStore = defineStore('alerts', {
          // 다른 옵션...
        })

      ID라고도 하는 이 name은 필요하며, 피니아에서 스토어와 devtools를 연결하는 데 사용한다.
      반환된 함수의 이름을 use...으로 지정하는 것은, 사용법을 관용적으로 만들기 위한 컴포저블 전반에 걸친 규칙이다.

      defineStore()의 두 번째 인자는 셋업 함수 또는 옵션 객체라는 두 개의 고유한 값을 허용한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
  옵션 스토어

    Vue의 옵션 API와 유사하게 state, actions, 및 getters 속성을 사용하여 옵션 객체를 전달할 수 있다.

      export const useCounterStore = defineStore('counter', {
        state: () => ({ count: 9, name: 'Eduardo' }),
        getters: {
          doubleCount: (state) => state.count * 2,
        },
        actions: {
          increment() {
            this.count++
          },
        },
      })

    state는 스토어의 data, getters는 스토어의 computed 속성, actions는 methods로 생각할 수 있다.

    옵션 스토어는 시작하기 쉽고 직관적이다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  셋업 스토어

    스토어를 정의하는 또 다른 문법이 있다. Vue 컴포지션 API의 셋업 함수와 유사하게, 반응형 속성 및 메서드를 정의하고, 노출하려는 속성 메서드가 있는 객체를 반환하는 함수를 전달할 수 있다.

      export const useCounterStore = defineStore('counter', () => {
        const count = ref(0)
        const name = ref('Eduardo')
        const doubleCount = computed(() => count.value * 2)
        function increment() {
          count.value++
        }

        return { count, name, doubleCount, increment }
      })

    셋업 스토어 내에서
    - ref()는 state 속성이 됨.
    - computed()는 getters가 됨.
    - function()은 actions가 됨.

    Pinia가 그것들을 상태로 인식하게 하려면, 셋업 스토어에서 모든 상태 속성을 반환해야 한다.
    다시 말해, 스토어에서 비공개 상태 속성을 가질 수 없다. 모든 상태 속성을 반환하지 않으면 SSR, 개발 도구, 그리고 다른 플러그인에서 문제가 발생할 수 있다.

    셋업 스토어는 스토어 내에서 감시자를 생성하고 컴포저블을 자유롭게 사용할 수 있어 옵션 스토어보다 훨씬 더 많은 유연성을 제공한다.
    하지만 SSR을 사용할 때 컴포저블 사용이 더 복잡해질 수 있음을 염두에 두어야 한다.

    셋업 스토어는 또한 Router나 Route와 같은 전역적으로 제공된 속성에 의존할 수 있다. 
    앱 수준에서 제공됨 어떠한 속성이라도 컴포넌트와 마찬가지로 inject()를 사용하여 스토어에서 접근할 수 있다.

      import { inject } from 'vue'
      import { useRoute } from 'vue-router'

      export const useSearchFilters = defineStore('search-filters', () => {
        const route = useRoute()
        // 이것은 app.provide('appProvided', 'value')가 호출되었다고 가정함.
        const appProvided = inject('appProvided')

        // ... 

        return{
          // ...
        }
      })

    경고
      route 또는 appProvided(위의 예에서)와 같은 속성은 스토어 자체에 속하지 않으며, 
      useRoute()및 inject('appProvided')를 사용하여 컴포넌트 내에서 직접 엑세스할 수 있으므로 반환하면 안된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  어떤 문법을 선택해야 할까?

    Vue의 컴포지션 API 및 옵션 API 처럼 가장 편한 것을 선택하면 된다. 잘 모르겠다면 먼저 옵션 스토어 스타일로 사용해보자.
    
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  스토어 이용하기

    스토어는 <scripte setup> 구성요소 내에서 (또는 모든 컴포저블과 마찬가지로 setup() 내에서) use...Store()가 호출될 때까지 스토어가 생성되지 않기 때문에 스토어를 정의한다.

      <script setup>
      import { useCountStore } from '@/store/counter'

      // 컴포넌트 어디에서나 'store' 변수에 액세스
      const store = useCounterStore()
      </script>

    TIP
      아직 setup 컴포넌트를 사용하지 않는 경우, "맵 헬퍼"로 피니아를 사용할 수 있다.

    원하는 만큼 스토어를 정의할 수 있다. 피니아를 최대한 활용하려면(예: 번들이나 코드분할 및 TypeScript 추론을 자동으로 허용),
    각 스토어는 다른 파일에 정의해야 한다.

    스토어가 인스턴스화되면, 스토어에서 직접 state, getters, actions에 정의된 모든 속성에 접근할 수 있다.
    다음 페이지에서 자세히 살펴보겠지만 자동 완성이 도움이 될 것이다.

    store는 reactive로 래핑된 객체이다. 즉, getter뒤에 .value를 쓸 필요가 없지만, setup의 props와 같이 구조화할 수 없다.

      <script setup>
      import { useCounterStore } from '@/stores/counter'
      const store = useCounterStore()
      // 반응성을 깨뜨리기 때문에 작동하지 않는다.
      // props에서 구조화하는 것과 동일하다.
      const { name, doubleCount } = store
      name // 언제나 'Eduardo'
      doubleCount // 언제나 0

      setTime(() => {
        store.increment()
      }, 1000)
      
      // 이것은 반응적일 것이다.
      // 또한 store.doubleCount로 직접 사용할 수도 있다.
      const doubleVlaue = computed(() => store.doubleCount)
      </script>

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  저장소에서 디스트럭처링

    반응형을 유지하면서 스토어에서 속성을 추출하려면, storeToRefs()를 사용해야 한다. 모든 반응형 속성에 대한 참조를 생성한다.
    이것은 스토어 상태만 사용하고, 액션을 호출하지 않을 때 유용하다. 스토어 자체에도 바인딩되므로, 스토어에서 직접 액션을 구조화할 수 있다.

      <script setup>
      import { useCounterStore } from '@/stores/counter'
      import { storeToRefs } from 'pinia'

      const store = useCounterStore()
      // name과 doubleCount는 반응형 refs임
      // 이것은 플러그인에 의해 추가된 속성에 대한 refs도 추출함.
      // 그러나 모든 액션 또는 비반응형 (ref/반응형이 아닌) 속성을 건너뜀.
      const { name, doubleCount } = storeToRefs(store)

      // increment 액션은 그냥 구조화 기능
      const { increment } = store
      </script>

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
