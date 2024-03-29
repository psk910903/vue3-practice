<template>
	<div></div>
</template>
<!--  
  
  Getters

    게터는 스토어의 상태에 대한 계산된 값과 정확히 동일하다. defineStore() 내에서 getters 속성으로 정의할 수 있다.
    화살표 함수의 사용을 권장하기 위해, 첫 번째 인자로 state를 받는다.

      export const useCounterStore = defineStore('counter', {
        state: () => ({
          count: 0,
        }),
        getters: {
          doubleCount: (state) => state.count * 2,
        },
      })

    대부분의 경우, 게터는 오직 상태에만 의존하지만, 다른 게터를 사용해야 할 수도 있다.
    이 때문에 일반 함수를 정의할 때, this를 통해 전체 스토어 인스턴스에 접근할 수 있지만, 반환 유형을 정의해야 한다(TypeScript에서).
    이것은 TypeScript의 알려진 제한 사항 때문이며, 화살표 함수로 정의된 게터나 this를 사용하지 않는 게터에 영향을 미치지 않는다.

      export const useCounterStore = defineStore('counter', {
        state: () => ({
          count: 0
        }),
        getters: {
          // 자동으로 반환 유형을 숫자로 유추함.
          doubleCount(state) {
            return state.count * 2
          },
          // 반환 유형은 반드시 명시적으로 설정되어야 함.
          doublePlusOne(): number {
            // 전체 스토어에 대한 자동 완성 및 타이핑
            return this.doubleCount + 1
          },
        },
      })

    그런 다음 스토어 인스턴스에서 직접 게터에 접근할 수 있다.

      <script setup>
      import { useCounterStore } from './counterStore'

      const store = useCountersStore()
      </script>

      <template>
        <p>Double count is {{ store.doubleCount }}</p>
      </template>

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  다른 getter에 접근

    계산된 속성과 마찬가지로 여러 개의 getter를 결합할 수 있다. this를 통해 다른 getter에 접근해야 한다.
    이 시나리오에서는 getter에 반환 타입을 지정해야 한다.

      // counterStore.ts
      export const useCounterStore = defineStore('counter', {
        state: () => ({
          count: 0
        }),
        getters: {
          doubleCount(state) {
            return state.count * 2
          },
          doubleCountPlusOne(): number {
            return this.doubleCount + 1
          },
        },
      })

      // counterStore.js
      // 자바스크립트에서 JSDoc (https://jsdoc.app/tags-returns.html)을 사용할 수 있다.
      export const useCounterStore = defineStore('counter', {
        state: () => ({
          count: 0
        }),
        getters: {
          // this를 사용하지 않기 때문에 타입이 자동으로 추론된다.
          doubleCount: (state) => state.count * 2,
          // 여기서는 타입을 직접 추가해야 한다. (JS에서 JSDoc 사용)
          // 이를 통해 getter를 문서화할 수도 있다.
          // 카운트 값을 두 배로 하고 1을 더한 값을 반환한다.
          doubleCountPlusOne() {
            // 자동완성
            return this.doubleCount + 1
          },
        },
      })

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  getter에 인자 전달

    게터는 내부적으로 계산된 속성일 뿐이라 파라미터를 전달할 수 없다. 그러나 게터에서 함수를 반환하여 모든 인자를 받을 수 있다.

      export const useStore = defineStore('main', {
        getters: {
          getUserById: (state) => {
            return (userId) => state.user.find((user) => userid === userId)
          },
        },
      })

    그리고 컴포넌트에서 사용

      <script setup>
      import { storeToRefs } from 'pinia'
      import { useUserListStore } from './store'

      const userList = useUserListStroe()
      const { getUserById } = storeToRef(userList)
      // <script setup> 내에서 함수에 엑세스하려면
      // getUserById.value를 사용해야 한다.
      </script>

      <template>
        <p>유저 2: {{ getUserById(2) }}</p>
      </template>

    이 작업을 수행할 때 게터는 더 이상 캐시되지 않고, 단순히 호출하는 함수이다. 그러나 게터 자체 내부에 일부 결과를 캐시할 수 있다.
    이는 흔하지 않지만 성능이 더 우수하다.

      export const useStore = defineStore('main', {
        getters: {
          getActiveUserById(state) P
          const activeUsers = state.users.filter((user) => user.active)
          return (userId) => activeUsers.find((user) => user.id === userId)
        }
      })

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  다른 스토어 getter에 접근

    다른 스토어 게터를 사용하려면, 게터 내부에서 직접 사용할 술 있다.

      import { useOthersStore } from './other-store'

      export const useStore = defineStore('main', {
        state: () => ({
          // ...
        }),
        getters: {
          otherGetter(state) {
            const otherStore = useOtherStore()
            return state.localData + otherStore.data
          },
        },
      })

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  setup()에서 사용

    스토어의 모든 게터를 상태 속성처럼 직접 접근할 수 있다.

      <script setup>
      const store = useCounterStore()

      store.count = 3
      store.doubleCount // 6
      </script>

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  옵션 API에서 사용

    다음 예제는 저장소가 생성되었다고 가정한다.

      // 예제 파일 경로:
      // ./src/stores/count.js

      import { defineStore } from 'pinia'

      exprot const useCounterStore = defineSTore('counter', {
        state: () => ({
          count: 0
        }),
        getters: {
          doubleCount(state) {
            return state.count * 2
          }
        }
      })

    setup() 에서
    
      컴포지션 API가 모든 사람을 위한 것은 아니지만, setup() 훅을 사용하면 옵션 API에서 피니아를 더 쉽게 사용할 수 있다.
      추가 맵 헬퍼 함수가 필요하지 않다.

        <script>
        import { useCounterStore } from '../stores/counter'

        export default defineComponent({
          setup() {
            const sounterStore = useCounterStore()

            // 구조를 파괴하는 대신 전체 장소만 반환한다.
            return { counterStore }
          },
          computed: {
            quadrupleCounter() {
              return this.countertStore.doubleCount * 2
            },
          },
        })
        </script>

      이것은 컴포넌트를 Options API에서 Composition API로 마이그레이션하는 동안 유용하지만, 마이그레이션 단계일 뿐이므로 항상 동일한 컴포넌트 내에서 두 API 스타일을 혼합하면 안된다.

    setup() 없이

      이전 섹션인 상태에서 사용한 것과 동일한 mapState() 함수를 사용하여 게터에 매핑할 수 있다.

        import { mapState } from 'pinia'
        import { useCounterStore } from '../stores/counter'

        export default {
          computed: {
            // 컴포넌트 내부에서 this.doubleCount로 접근할 수 있게 함.
            // store.doubleCount로 읽는 것과 동일
            ...mapState(useCounterStore, ['doubleCount']),
            // 위와 같지만 this.myOwnName으로 등록
            ...mapState(useCounterStore, {
              myOwnName: 'doubleCount',
              // 스토어에 접근하는 함수를 작성할 수도 있음.
              double: (store) => store.doubleCount,
            }),
          },
        }

      
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
