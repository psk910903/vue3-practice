<template>
	<div></div>
</template>
<!--  

  소개

    피니아는 2019년 11월경에 컴포지션 API로 Vue용 스토어가 어떻게 생겼는지 재설계하기 위한 실험으로 시작했다.
    그 이후로 초기 원칙은 여전히 동일하지만, 피니아는 컴포지션 API를 사용할 필요가 없으며, Vue 2와 Vue 3 모두에서 작동한다.
    API는 설치와 SSR을 제외하고 모두 동일하며, 이 문서는 Vue 2 및 Vue 3 사용자가 읽을 수 있도록 필요할 때마다 Vue 2에 대한 메모와 함께 Vue 3 대상으로 한다.
    
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  왜 피니아를 사용해야 할까?

    피니아는 Vue의 스토어 라이브러리로 컴포넌트/페이지 간에 상태를 공유할 수 있다. 
    컴포지션 API에 익수가다면 간단한 export const state = reactive({})로 전역 상태를 공유할 수 있다고 생각할 수 있다.
    이는 SPA에는 해당되지만 SSR의 경우, 앱이 보안 취약성에 노출된다.
    그러나 작은 SPA에서도 피니아를 사용하면 많은 이점이 있다.

    1.Devtools 지원
      - actions, mutations을 추적하는 타임라인
      - 스토어는 사용되는 컴포넌트에 표시됨.
      - 시간 추적 및 더 쉬운 디버깅
    2.핫 모듈 교체(HMR)
      - 페이지를 새로고침하지 않고 스토어 수정
      - 개발하는 동안 기존 상태 유지
    3.플러그인: 플러그인으로 피니아 기능 확장
    4.TypeScript 지원 또는 JS 사용자를 위한 적절한 자동 완성
    5.SSR 지원

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  기본 예제

    이것은 API 측면에서 피니아를 사용하는 것과 같다. 스토어를 만드는 것으로 시작한다.

      // stores/counter.js
      import { defineStore } from 'pinia'

      export const useCounterStore = defineStore('counter', {
        state: () => {
          return { count: 0 }
        },
        // 다음과 같이 정의할 수도 있음
        // state: () => ({ count: 0 })

        actions: {
          increment() {
            this.count++
          }
        }
      })

    그런 다음 컴포넌트에서 사용한다.

      <script setup>
      import { useCounterStore } from '@/stores/counter'

      const counter = useCounterStore()

      counter.count++
      // 자동 완성 기능
      counter.$patch({ count: counter.count + 1})
      // 또는 actions 사용
      counter.increment()
      </script>

      <template>
        // 스토어에서 직접 상태에 엑세스
        <div>현재 카운트: {{ counter.count }}</div>
      </template>

    고급 사용 목적으로 함수(컴포넌트 setup()과 유사)를 사용하여 스토어를 정의할 수도 있다.

      export const useCounterStore = defineStore('counter', () => {
        const count = ref(0)
        function increment() {
          count.value++
        }

        return { count, increment }
      })

    아직 setup() 및 컴포지션 API에 익숙하지 않더라도 피니아는 Vuex와 같은 맵 헬퍼와 같은 세트를 지원한다.
    스토어 정의 방식은 같지만, mapStores(), mapState() 또는 mapActions()를 사용한다.

      const useCounterStore = defineStore('counter', {
        state: () => ({ count: 0 }),
        getters: {
          double: (state) => state.count * 2,
        },
        actions: {
          increment() {
            this.count++
          },
        },
      })

      const useUserStore = defineStore('user', {
        // ...
      })

      export useUserStore = defineComponent({
        computed: {
          // 다른 계산된 속성
          // ...
          // this.counterStore와 this.userStore로 접근 가능.
          ...mapStores(useCounterStore, useUserStore),
          // this.count와 this.double의 읽기 접근 권한을 부여.
          ...mapState(useCounterStore, ['count', 'double']),
        },
        methods: {
          // this.increment()에 접근 제공.
          ...mapActions(useCounterStore, ['increment'])
        }
      })

    핵심 개념에서 각 "맵 헬퍼"에 대한 자세한 정보를 찾을 수 있다.
    
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  왜 피니아인가?

    피니아는 스페인어 _pineapple_의 영어 발음과 가장 유사한 _piña_이다. 파인애플은 실제로 각각의 꽃들이 하나의 그룹으로 된 과일이다.
    꽃은 각각 피어나지만, 결국 모두 합쳐지는 모습이 마치 스토어 같다. 남아메리카가 원산지인 맛있는 열대 과일이기도 하다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  좀 더 현실적인 예제

    다음은 JavaScript에서도 유형이 있는 피니아를 사용할 수 있는 예제이다. 일부 개발자에게는 이 설명으로 충분할 수 있다.
    이해되지 않는다면, 이 예제를 건너뛰고 다른 모든 핵심 개념에 대해 읽은 후에 다시 오는 것이 좋다.

      import { defineStore } from 'pinia'

      export const useTodos = defineStore('todos', {
        state: () => ({
          /** @type {{ text: string, id: number, isFinished: boolean }[]} */
          todos: [],
          /** @type {'all' | 'finished' | 'unfinished'} */
          filter: 'all',
          // 유형은 자동으로 숫자로 유추됨.
          nextId: 0,
        }),
        getters: {
          finishedTodos(state) {
            // 자동 완성
            return state.todos.filter((todo) => todo.isFinished)
          },
          unfinishedTodos(state) {
            return state.todos.filter((todo) => !todo.isFinished)
          },
          /**
          * @returns {{ text: string, id: number, isFinished: boolean }[]}
          */
          filteredTodos(state) {
            if (this.filer === 'finished') {
              // 자동 완성 기능으로 다른 getters 호출
              return this. finishedTodos
            } else if (this.filter === 'unfinished') {
              return this.unfinishedTodos
            }
            return this.todos
          },
        },
        actions: {
          // 인자의 양에 관계없이 promise를 반환할지 여부
          addTodo(text) {
            // 상태를 직접 변경할 수 있음
            this.todos.push({ text, id: this.nextId++, isFinished: false })
          },
        },
      })

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  Vuex와 비교

    Vuex와 비교할 때, 피니아는 더 간단한 API를 제공하고, 컴포지션 API 스타일을 제공하며, 가장 중요한 것은 TypeScript와 함께 사용할 때 견고한 유형 추론을 지원한다.

    # RFCs
    
      처음에 피니아는 RFC 프로세스를 거치지 않았다. 피니아는 기본 상태 관리 솔루션이며, Vue 생태계의 다른 핵심 라이브러리와 동일한 RFC 프로세스를 거치며 API는 안정적인 상태에 진입했다.

    Vuex3.x/4.x와 비교(Vuex 3.x는 Vue 2용이고, Vuex 4.x는 Vue 3용입니다.)

      피니아 API는 Vuex 4와 매우 다르다.

        - mutations는 더 이상 존재하지 않는다. 이것은 종종 매우 장황한 것으로 인식되었다. 이것은 처음에 devtools 통합을 제공했지만, 더 이상 문제가 되지 않는다.
        - TypeScript를 지원하기 위해 복잡한 커스텀 래퍼를 만들 필요가 없으며, 모든 것이 입력되며 API는 TS 유형 추론을 최대한 활용하는 방식으로 설계되었다.
        - 더 이상 매직 문자열을 주입없이, 함수를 가져오고, 호출하고, 자동 완성할 수 있다.
        - 스토어를 동적으로 추가할 필요가 없다. 기본적으로 모두 동적으로 작동하므로 눈치채지 못할 것이다. 원할 때마다 수동으로 스토어를 사용하여 등록할 수 있지만, 자동이기 때문에 걱정할 필요가 없다.
        - 더 이상 모듈의 중첩 구조가 없다. 스토어를 가져오고 다른 스토어 내부에 사용하여 여전히 스토어를 암시적으로 중첩할 수 있지만, 피니아는 수평적으로 디자인한 구조를 제공하는 동시에 스토어 간에 교차 구성 방법을 여전히 가능하게 한다. 스토어의 순환 종속성을 가질 수도 있다.
        - 네임스페이스 모듈이 없다. 스토어의 수평적 아키텍처를 고려할 때, "네임스페이스" 스토어는 정의된 방식에 내재되어 있으며, 모든 스토어가 네임스페이스라고 말할 수 있다.

      

      

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
