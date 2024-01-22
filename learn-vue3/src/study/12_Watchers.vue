<template>
	<div></div>
</template>
<!--  

  감시자

  ----------------------------------------------------------------------------------------------------------------------------

    기본 예제

      계산된 속성은 계산되어 파생된 값을 선언적으로 사용할 수 있게 한다. 
      그러나 상태 변경에 대한 반응으로 "사이드 이펙트"(예: DOM을 변경하거나 비동기 작업의 결과를 기반으로 다른 상태를 변경하는 것)을 수행해야 하는 경우가 있다.

      Composition API를 사용하는 경우, watch 함수를 사용하여 반응형 속성이 변경될 때마다 함수를 실행할 수 있다.

        <script setup>
        import { ref, watch } from 'vue'

        const question = ref('')
        const answer = ref('질문에는 일반적으로 물음표가 포함됩니다.')
        const loading = ref(false)

        // watch는 ref에서 직접 작동합니다
        watch(question, async (newQuestion, oldQuestion) => {
          if (newQuestion.includes('?')) {
            loading.value = true
            answer.value = '생각 중...'
            try {
              const res = await fetch('https://yesno.wtf/api')
              answer.value = (await res.json()).answer === 'yes' ? '네' : '아니오'
            } catch (error) {
              answer.value = '에러! API에 연결할 수 없습니다. ' + error
            } finally {
              loading.value = false
            }
          }
        })
        </script>

        <template>
          <p>
            예/아니오 질문:
            <input v-model="question" :disabled="loading" />
          </p>
          <p>{{ answer }}</p>
        </template>

    
    감시 대상 타입

      watch의 첫 번째 인자는 다양한 유형의 반응형 "소스"일 수 있다. 참조(계산된 참조 포함),
      반응형 객체, getter 함수 또는 여러 소스의 배열이 될 수 있다.

        const x = ref(0)
        const y = ref(0)

        // 단일 ref
        watch(x, (newX) => {
          console.log(`x값: ${newX}`)
        })

        // getter
        watch(
          () => x.value + y.value,
          (sum) => {
            console.log(`x + y: ${sum}`)
          }
        )

        // 여러 소스의 배열
        watch([x, () => y.value], ([newX, newY]) => {
          console.log(`x는 ${newX}이고, y는 ${newY} 입니다.`)
        })

      다음과 같이 반응형 객체의 속성을 감시할 수는 없다.

        const obj = reactive({ count: 0 })

        이것은 watch()에 숫자를 전달하기 때문에 작동하지 않는다.
        watch(obj.count, (count) => {
          console.log(`count 값: ${count}`)
        })

      대신 getter를 사용해야한다.

        watch(
          () => obj.count,
          (count) => {
            console.log(`count 값: ${count}`)
          }
        )

  ----------------------------------------------------------------------------------------------------------------------------

    깊은 감시자

      반응형 객체에서 watch()를 직접 호출하면 암시적으로 심층 감시자가 생성되며,
      콜백은 중첩된 모든 변경에서 트리거된다.

        const someObject = reactive({count: 0})

        watch(someObject, (newValue, oldValue) =>{
          중첩된 속성 변경사항이 있을 경우 실행된다.
          참고 : newValue와 oldValue는 같다. 둘 다 동일한 객체를 참고하고 있기 때문이다.
        })

        someObject.count++

      반응형 객체를 반환하는 getter와 구별해야 한다. 후자의 경우 콜백은 getter가 다른 객체를 반환하는 경우에만 실행된다.

        const state = reactive({
          someObject: {count: 0}
        })

        watch(
          () => state.someObject,
          () => {
            state.someObject가 교체될 때만 실행된다.
          }
        )

      그러나 deep 옵션을 명시적으로 사용하여 두 번째 경우(위 예제)를 깊은 감시자로 강제할 수 있다.

        watch(
          () => state.someObject,
          (newValue, oldValue) => {
            참고: state.someObject가 교체되지 않는 한 여기에서 newValue와 oldValue는 같다.
          },
          {deep: true}
        )

      사용 시 주의사항
        깊은 감시자는 감시된 객체의 모든 중첩 속성을 탐색하므로, 큰 데이터 구조에서 사용할 때 비용이 많이 들 수 있다.
        성능에 영향을 주는지 고려해서 필요한 경우에만 사용해야한다.

  ----------------------------------------------------------------------------------------------------------------------------

    열성적인 감시자

      watch는 기본적으로 게으르다(lazy). 콜백은 감시된 소스가 변경되기 전까지 호출되지 않는다.
      그러나 어떤 경우에는 동일한 콜백 로직이 열성적으로 실행되기를 원할 수 있다. 예를 들어 최초 데이터가 구성된 후 콜백이 실행되기를 원할 수 있다.

      immediate: true 옵션을 전달하여 위처의 콜백이 즉시 실행되도록 강제할 수 있다.

        watch(
          source,
          (newValue, oldValue) => {
            즉시 실행된 다음 source가 변경되면 다시 실행된다.
          },
          { immediate: true}
        )

  ----------------------------------------------------------------------------------------------------------------------------

    watchEffect()

      워처 콜백이 소스와 정확히 동일한 반응형 상태를 사용하는 것은 일반적이다.
      예를 들어, 다음 코드를 고려해 봤을 때, 이 코드는 todoId ref가 변경될 때마다 원격 리소스를 로드하기 위해 워처를 사용한다.

        const todoId = ref(1)
        const data = ref(null)

        watch(
          todoId,
          async () => {
            const response = await fetch(
              `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
            )
            data.value = await response.json()
          },
          { immediate: true }
        )

      특히 워처가 todoId를 소스로 한 번, 콜백 내에서 다시 한 번 사용하는 것에 주목해야한다.

      이는 watchEffect()로 단순화될 수 있다. watchEffect()는 콜백의 반응형 종속성을 자동으로 추적할 수 있다.
      위의 워처는 다음과 같이 다시 작성될 수 있다.

        watchEffect(async () => {
          const response = await fetch(
            `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
          )
          data.value = await response.json()
        })

      여기서, 콜백은 즉시 실행되며, immediate: true를 명시할 필요가 없다. 실행 중에 자동으로 todoId.value를 종속성으로 추적한다.(계산된 속성과 유사하게)
      todoId.value가 변경될 때마다 콜백이 다시 실행된다. watchEffect()를 사용하면 todoId를 소스 값으로 명시적으로 전달할 필요가 없다.

      watchEffect()와 반응형 데이터 가져오기가 작동하는 경우에서 단일 종속성만 있는 경우, watchEffect()의 이점은 상대적으로 작다.
      그러나 여러 종속성을 가진 워처의 경우, watchEffect()를 사용하면 종속성 목록을 수동으로 유지하는 부담을 없앨 수 있다.
      또한, 중첩된 데이터 구조에서 여러 속성을 감시해야 하는 경우, watchEffect()는 깊은 워처보다 더 효율적일 수 있다.
      콜백에서 사용되는 속성만 추적하는 반면, 모든 속성을 재귀적으로 추적하는 것이 아니기 때문이다.

      TIP
        watchEffect는 동기적 실행 중에만 종속성을 추적한다. 
        비동기 콜백을 사용할 때는 첫 번째 await 틱 전에 접근한 속성만 추적된다.
      
      watch vs watchEffect

        watch와 'watchEffect' 둘 다 사이드 이펙트를 반응적으로 실행할 수 있게 해준다.
        주요 차이점은 반응형 의존성을 추적하는 방식이다.

          - watch는 명시적으로 감시된 소스만 추적한다. 콜백 내에서 조회하는 항목은 추적하지 않는다.
            또한 콜백은 소스가 실제로 변경된 경우에만 트리거된다. watch는 의존성 추적을 사이드 이펙트와 분리하여,
            콜백이 실행되어야 하는 시기를 보다 정확하게 제어할 수 있다.

          - 반면 watchEffect는 의존성 추적과 사이드 이펙트를 하나의 단계로 결합한다.
            동기적(snyc) 실행 중에 조회되는 모든 반응형 속성을 자동으로 추적한다.
            이것은 더 편리하고 일반적으로 더 간결한 코드를 생성하지만, 콜백이 실행되어야 하는 시기가 덜 명시적이다.

  ----------------------------------------------------------------------------------------------------------------------------

    콜백 실행 타이밍

      반응형 상태를 변경하면 Vue 컴포넌트 업데이트와 사용자가 만든 감시자 콜백이 모두 실행된다.

      기본적으로 개발자가 생성한 감시자 콜백은 Vue 컴포넌트가 업데이트되기 전에 실행된다.
      따라서 감시자 콜백 내에서 DOM에 접근하면 DOM이 Vue에 의해 업데이트되기 전의 상태이다.

      Vue에 의해 업데이트된 후의 DOM을 감시자 콜백에 접근하려면, flush: 'post' 옵션을 정의해야 한다.

        watch(source, callback, {
          flush: 'post'
        })

        watchEffect(callback, {
          flush: 'post'
        })

      flush: 'post' 옵션이 적용된 watchEffect()를 보다 간편하게 사용하기 위해서 watchPostEffect()를 사용할 수 있다.

        import { watchPostEffct } from 'vue'

        watchPostEffect(() => {
          Vue가 업데이트 된 후 실행
        })

      기본적으로 flush: 'pre' | 'post' 옵션은 콜백의 버퍼링하여, 동일한 "틱(tick)"에서 여러 번 상태 변경이 되더라도, 마지막에 한번만 호출된다.

      동일한 틱 내에 여러 번 상태 변경 시 마다 동기적으로 콜백을 호출해야 하는 경우,
      flush: 'sync'옵션을 사용해야 한다. 단, 일반적으로 이러한 동작은 비효율적이므로 사용하려는 경우, 정말 필요한지 다시 한번 고민해봐야 한다.

        const count = ref(0)
        const callback = (val, preVal) =? console.log('변경이 감지됨', val, preVal)
        const options = { flush: true }

        watch(count, callback, options)

        count.value++ // 이어서 callback이 실행됨
        count.Value++ // 역시 callback이 실행됨
        count.value++ // 또 callback이 실행됨

  ----------------------------------------------------------------------------------------------------------------------------

    감시자 중지하기

    setup()또는 <script setup>내부에서 동기적으로 선언된 감시자는 해당 컴포넌트 인스턴스에 바인됭되며, 해당 컴포넌트가 마운트 해제되면 자동으로 중지된다.
    대부분의 경우 감시자를 직접 중지하는 것에 대해 고민할 필요가 없다.

    여기서 핵심은 감시자가 동기적(synchronously)으로 생성되어야 한다는 것이다.
    감시자가 비동기 콜백에서 생성된 경우, 감시자는 해당 컴포넌트에 바인딩되지 않으며 메모리 누수를 방지하기 위해 수동으로 중지해야 한다.
    다음은 이러한 케이스에 해당하는 예제이다.

      <script setup>
      import { watchEffect } from 'vue'

      이 감시자는 컴포넌트가 마운트 해제되면 자동으로 중지된다.
      watchEffect( () => {} )

      하지만 이것은 자동으로 중지되지 않는다.
      setTimeout(() => {
        watchEffect(() => {} , 100)
      })

    감시자를 수동으로 중지하려면 반환된 함수를 사용해라. 이것은 watch와 watchEffect 모두에서 작동한다.

      const unwatch = watchEffect(() => {})
      나중에 감시자가 더 필요하지 않을 때 
      unwatch()

    감시자를 비동기식으로 생성해야 하는 경우는 거의 없으며, 가능하면 동기적 생성을 해야 한다.
    일부 비동기 데이터를 기다려야 하는 경우, 감시자 로직을 조건부로 만들 수 있다.

      비동기적으로 로드할 데이터
      const data =ref(null)

      watchEffect(() => {
        if(data.value){
          데이터가 로드될 때 실행될 로직
        }
      })




-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
