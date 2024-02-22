<template>
	<div></div>
</template>
<!--  

  ref()

    전달된 값을 갖게 되고, 이것을 가리키는 단일 속성 .value가 있는 변경 가능한 반응형 ref 객체를 반환한다.

    타입
    
      function ref<T>(value: T): Ref<UnwrapRef<T>>

      interface Ref<T> {
        value: T
      }

    세부 사항

      ref 객체는 반응형이며, .value 속성에 새 값을 할당할 수 있다. 
      즉, value에 대한 모든 읽기 작업이 추적되고, 쓰기 작업은 관련 이펙트를 트리거한다.

      객체가 ref의 값(.value)으로 할당되면, reactive()로 내부 깊숙이(deeply) 반응하게 된다.
      이러한 동작은 객체 내부 깊숙이 ref가 포함되어 있으면, 언래핑됨을 의미한다.

      내부 깊숙이까지 반환되는 것을 방지하려면, shallowRef()를 사용해야 한다.

    예제

      const count = ref(0)
      console.log(count.value) // 0

      count.value = 1
      console.log(count.value) // 1

    ref() 함수는 Vue 3에서 사용되는 리엑티브 참조(reactive reference)를 생성하는 함수다.
    이 함수를 사용하면 Vue 컴포넌트 내에서 DOM 요소나 데이터에 쉽게 접근할 수 있다.
    ref() 함수로 생성된 리엑티브 참조는 .value 속성을 통해 해당 값에 접근할 수 있다.

    ref()함수를 사용하면 원시 데이터나 객체를 감싸서 리액티브한 값으로 변환할 수 있다. 
    이는 Vue3의 반응형 시스템과 연동되어 Vue의 리액티브 반응성을 제공한다.
    즉, 해당 값이 변경될 때 Vue는 자동으로 컴포넌트를 다시 렌더링한다.

    예를 들어, 다음은 ref() 함수를 사용하여 간단한 숫자 데이터를 리액티브한 값으로 변환하는 예제이다.

      import { ref } from 'vue';

      export default {
        setup() {
          const count = ref(0);

          function increment() {
            count.value++;
          }

          return {
            count,
            increment
          };
        }
      };

    위의 예제에서 count 변수는 ref() 함수를 사용하여 리액티브한 값으로 만들어졌다.
    increment()함수는 count값을 증가시키는데, 이때 .value 속성을 사용하여 리액티브한 값을 읽고 쓸 수 있다.

    ref() 함수를 사용하여 DOM 요소에도 접근할 수 있다. 예를 들어, 특정 DOM 요소에 참조를 만들어 다음과 같이 접근할 수 있다.

      import { ref, onMounted } from 'vue';

      export default {
        setup() {
          const myElement = ref(null);

          onMounted(() => {
            console.log(myElement.value); // 해당 DOM 요소에 접근 가능
          });

          return {
            myElement
          };
        }
      };

    위의 예제에서는 conMounted() 훅을 사용하여 컴포넌트가 마운트되었을 때 해당 DOM 요소에 접근하고 있다.
    이때 ref() 함수를 사용하여 DOM 요소에 참조를 만들어 사용한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  computed()

    getter 함수를 사용하며, getter로부터 반환된 값을 읽기 전용 반응형 ref객체로 반환한다.
    get과 set함수가 있는 객체를 사용하면, 쓰기 가능한 ref 객체를 반환한다.

    타입

      // 읽기 전용
      function computed<T>(
        getter: (oldValue: T | undefined) => T,
        // 아래의 "계산된 속성 디버깅" 링크를 참고하세요
        debuggerOptions?: DebuggerOptions
      ): Readonly<Ref<Readonly<T>>>

      // 쓰기 가능
      function computed<T>(
        options: {
          get: (oldValue: T | undefined) => T
          set: (value: T) => void
        },
        debuggerOptions?: DebuggerOptions
      ): Ref<T>

    예제

      읽기 전용 계산된 속성 ref 만들기

        const count = ref(1)
        const plusOne = computed(() => count.value + 1)

        console.log(plusOne.value) // 2

        plusOne.value++ // error

      쓰기 가능한 계산된 속성 ref 만들기

        const count = ref(1)
        const plusOne = computed({
          get: () => count.value + 1,
          set: (val) => {
            count.value = val - 1
          }
        })

        plusOne.value = 1
        console.log(count.value) // 0

      디버깅

        const plusOne = computed(() => count.value + 1, {
          onTrack(e) {
            debugger
          },
          onTrigger(e) {
            debugger
          }
        })


    computed() 함수는 Vue 3에서 사용되는 계산된 속성(computed property)을 생성하는 함수다.
    계산된 속성은 기존의 데이터 속성을 기반으로 새로운 값을 계산하고, 이 값을 캐시하여 필요할 때마다 다시 계산하지 않는다.
    이는 Vue의 리액티브 시스템과 연동되어 변경된 데이터에 따라 자동으로 다시 계산된다.

    computed() 함수를 사용하여 생성된 계산된 속성은 ref() 함수로 생성된 리액티브 참조와 유사하게 사용된다.
    그러나 계산된 속성은 함수이므로 값을 읽기 위해서는 함수를 호출해야 한다.
    Vue는 해당 함수가 의존하는 데이터가 변경될 때마다 자동으로 함수를 다시 실행하고, 이를 통해 캐시된 값을 업데이트한다.

    예를 들어, 다음은 computed() 함수를 사용하여 이름과 성을 합친 전체 이름을 계산하는 예제이다.

      import { computed } from 'vue';

      export default {
        setup() {
          const firstName = 'John';
          const lastName = 'Doe';

          const fullName = computed(() => {
            return `${firstName} ${lastName}`;
          });

          return {
            fullName
          };
        }
      };

    위의 예제에서 fullName은 computed() 함수를 사용하여 계산된 속성으로 만들어졌다.
    fullName은 firstName과 lastName을 기반으로 계산되며, 의존하는 데이터가 변경될 때마다 자동으로 새로운 값을 계산한다.

    컴포넌트 템플릿에서 이를 사용하는 예제는 다음과 같다.

      <template>
        <div>
          <p>First Name: {{ firstName }}</p>
          <p>Last Name: {{ lastName }}</p>
          <p>Full Name: {{ fullName }}</p>
        </div>
      </template>

      <script>
      import { computed } from 'vue';

      export default {
        setup() {
          const firstName = 'John';
          const lastName = 'Doe';

          const fullName = computed(() => {
            return `${firstName} ${lastName}`;
          });

          return {
            firstName,
            lastName,
            fullName
          };
        }
      };
      </script>

    위의 템플릿에서는 fullName을 사용하여 전체 이름을 표시하고 있다. 이 때 fullName은 계산된 속성으로
    firstName과 lastName이 변경될 때마다 자동으로 새로운 값으로 업데이트된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  reactive()

    객체의 반응형 프록시(proxy)를 반환한다.

    타입

      function reactive<T extends object>(target: T): UnwrapNestedRefs<T>

    세부 사항

      반응형 변환은 "내부 깊숙이 있는(deep)" 모든 중첩 속성에 영향을 준다. 
      또한 반응형 객체는 내부 깊숙이 있는 모든 refs 속성의 반응형을 유지하면서 언래핑한다.

      그러나 Map 같은 네이티브 컬렉션 타입 또는 반응형 배열의 요소인 ref로 접근할 때는 언래핑 되지 않음에 유의해야 한다.

      내부 깊은 곳까지의 변환은 피하고 루트 수준에서만 반응형을 유지하려면 shallowReactive()를 사용해야 한다.

      반환된 객체와 중첩된 객체는 프록시(Proxy)로 래핑되므로, 원본 객체와 동일하지 않다.
      그로므로 반응형 프록시로만 작업하고, 원본 객체에 의존하지 않는 것이 좋다.

    예제

      반응형 객체 생성

        const obj = reactive({ count: 0 })
        obj.count++

      Ref 언래핑

        const count = ref(1)
        const obj = reactive({ count })

        // obj.count인 ref는 언래핑 됨
        console.log(obj.count === count.value) // true

        // `obj.count`도 업데이트 됨
        count.value++
        console.log(count.value) // 2
        console.log(obj.count) // 2

        // `count` ref도 업데이트 됨
        obj.count++
        console.log(obj.count) // 3
        console.log(count.value) // 3

      배열 또는 컬렉션 엘리먼트인 ref에 접근시, 언래핑 되지 않는다.

        const books = reactive([ref('Vue 3 Guide')])
        // .value 필요
        console.log(books[0].value)

        const map = reactive(new Map([['count', ref(0)]]))
        // .value 필요
        console.log(map.get('count').value)

      ref를 reactive 속성에 할당하면, 해당 ref도 자동으로 언래핑된다.

        const count = ref(1)
        const obj = reactive({})

        obj.count = count

        console.log(obj.count) // 1
        console.log(obj.count === count.value) // true

    
    reactive() 함수는 Vue3에서 사용되는 리액티브 객체(reactive object)를 생성하는 함수이다. 
    리액티브 객체는 Vue의 반응형 시스템을 활용하여 객체의 속성이 변경될 때마다 자동으로 뷰를 업데이트할 수 있도록 한다.

    reactive() 함수를 사용하면 일반 객체를 리액티브 객체로 변환할 수 있다. 변환된 리액티브 객체는 객체의 각 속성이 Vue의 반응성을 가지며,
    해당 속성이 변경될 때 Vue는 자동으로 해당 컴포넌트를 다시 렌더링한다.

    예를 들어, 다음은 reactive() 함수를 사용하여 리액티브 객체를 생성하는 예제이다.

      import { reactive } from 'vue';

      export default {
        setup() {
          const user = reactive({
            name: 'John',
            age: 30
          });

          return {
            user
          };
        }
      };

    위의 예제에서 user객체는 reactive() 함수를 사용하여 리액티브 객체로 만들어졌다.
    따라서 user 객체의 name과 age속성이 변경될 때마다 Vue는 자동으로 해당 컴포넌트를 다시 랜더링한다.

    리액티브 객체를 사용하면 일반 객체와 마찬가지로 속성을 읽고 쓸 수 있다.
    다만, 변경된 속성은 Vue의 반응성 시스템과 연동되어 컴포넌트의 뷰를 업데이트할 수 있다.

      <template>
        <div>
          <p>Name: {{ user.name }}</p>
          <p>Age: {{ user.age }}</p>
        </div>
      </template>

      <script>
      import { reactive } from 'vue';

      export default {
        setup() {
          const user = reactive({
            name: 'John',
            age: 30
          });

          setTimeout(() => {
            user.name = 'Jane'; // 이 코드가 실행되면 화면에 표시되는 내용이 자동으로 업데이트됨
          }, 2000);

          return {
            user
          };
        }
      };
      </script>

    위의 템플릿에서는 user 객체의 name과 age 속성을 표시하고 있다. setTimeout()함수를 사용하여 2초 후에 user.name의 값을 변경하면,
    해당 변경 사항이 화면에 자동으로 반영된다. 이는 user 객체가 리액티브하게 만들어졌기 때문에 가능한 것이다.

    reactive() 함수는 객체를 래핑(wrapping)하여 프록시(proxy)객체를 생성한다. 여기서 래핑은 객체를 감싸는 것을 의미하며,
    프록시는 원본 객체에 대한 대리자 역할을 하는 객체를 말한다.

    프록시 객체는 원본 객체를 래핑하여 원본 객체의 속성에 접근할 때 중간에서 작업을 수행할 수 있는 기능을 제공한다.
    이를 통해 Vue는 객체의 속성에 대한 접근을 가로채어 해당 속성의 변경사항을 감지하고, 변경사항이 발생하면 관련된 컴포넌트를 다시 렌더링할 수 있다.

    또한, 리액티브 객체는 래핑된 객체와 프록시 사이에 대응 관계를 가지고 있다.
    따라서 리액티브 객체를 래핑하면 래핑된 객체와 프록시 객체 간의 언래핑이 가능해진다.
    언래핑은 반대로 객체를 감싸고 있는 프록시 객체를 다시 제거하여 원본 객체를 얻는 것을 의미한다.

    이러한 프록시와 래핑, 언래핑 개념은 Vue의 반응형 시스템의 핵심이다.
    이를 통해 Vue는 객체의 변경을 감지하고 이에 대응하여 UI를 업데이트할 수 있다.

    간단한 예제

      import { reactive } from 'vue';

      const originalObject = { count: 0 };

      // 리액티브 객체 생성
      const reactiveObject = reactive(originalObject);

      // 프록시 객체 확인
      console.log(reactiveObject); // Proxy { count: 0 }

      // 래핑된 객체 확인
      console.log(reactiveObject.__v_raw); // { count: 0 }

      // 래핑된 객체 언래핑
      const unwrappedObject = reactiveObject.__v_raw;

      console.log(unwrappedObject === originalObject); // true

    위의 예제에서는 reactive() 함수를 사용하여 originalObject를 리액티브 객체로 만든 후,
    해당 리액티브 객체를 reactiveObject에 할당했다.
    reactiveObject는 프록시 객체이며, reactiveObject.__v_raw를 통해 래핑된 객체를 확인할 수 있다.
    이후 언래핑을 통해 래핑된 객체를 다시 제거하여 원본 객체를 얻을 수 있다.

      *__v_raw 란

        __v_raw는 Vue3에서 내부적으로 사용되는 특수한 속성이다. 이 속성은 reactive() 함수나 ref() 함수로 생성된 리액티브 객체의 원본 데이터에 대한 접근을 제공한다.

        Vue3에서는 리액티브 객체를 사용하여 데이터를 추적하고 변경 사항을 감지한다.
        그런데 때로는 리액티브 객체의 실제 데이터에 직접 접근해야 할 때가 있다.
        이 때 __v_raw 속성을 사용하면 리액티브 객체의 원본 데이터에 직접 접근할 수 있다.

        일반적으로 개발자가 직접 사용하는 것은 권장되지 않는다.
        이 속성은 주로 디버깅이나 특정한 상황에서 내부적으로 사용된다.
        개발자가 리액티브 객체의 원본 데이터에 접근해야 할 때는 보통 다른 방법을 사용하는 것이 좋다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  readonly()

    객체(반응형 또는 일반) 또는 ref를 가져와서 원본에 대한 읽기 전용 프록시를 반환한다.

    타입

      function readonly<T extends object>(
        target: T
      ): DeepReadonly<UnwrapNestedRefs<T>>

    세부 사항

      읽기 전용 프록시는 접근하게 될 모든 중첩 속성 깊숙이까지 읽기 전용이다.
      또한 reactive() 처럼 ref를 언래핑하며, 언래핑 값도 읽기 전용으로 변환된다.

      내부 깊은 곳까지의 변환을 피하려면, shallowReadonly()를 사용해야 한다.

    예제

      const original = reactive({ count: 0 })

      const copy = readonly(original)

      watchEffect(() => {
        // 반응형 추적으로 작동됨
        console.log(copy.count)
      })

      // original을 변경하면 copy를 의존하는 감시자가 트리거됨
      original.count++

      // copy를 변경하려 해도 변경되지 않으며, 경고가 발생함
      copy.count++ // 경고!


    readonly() 함수는 Vue3에서 사용되는 함수 중 하나로, 읽기 전용(readonly)인 반응형 데이터를 생성하는 데 사용된다.
    이 함수는 주어진 데이터를 읽기 전용으로 만들어 Vue의 반응성 시스템과 연동된다.
    읽기 전용 데이터는 변경될 수 없으며, Vue는 해당 데이터의 변경을 감지하지 않는다.

    readonly() 함수는 기본적으로 데이터를 반응형으로 만들기 위한 reacive() 함수와 유사하지만,
    읽기 전용으로 만들어진다는 점에서 다르다.
    읽기 전용 데이터는 일반적으로 컴포넌트 내부에서 변경되지 않는 데이터를 관리할 때 사용된다.

    예를 들어, 다음은 readonly() 함수를 사용하여 읽기 전용 데이터를 생성하는 예제이다.

      import { readonly } from 'vue';

      const data = { name: 'John', age: 30 };

      const readOnlyData = readonly(data);

      console.log(readOnlyData.name); // 'John'

      // 읽기 전용 데이터는 변경할 수 없으므로 아래와 같은 코드는 에러를 발생시킨다.
      // readOnlyData.name = 'Jane'; // Error: Cannot assign to read only property 'name' of object '#<Object>'

    위의 예제에서 readonly()함수를 사용하여 data 객체를 읽기 전용으로 만들었다.
    이제 readOnlyData는 읽기 전용으로 설정된 반응형 데이터이다.
    따라서 readOnlyData의 속성을 변경하려고 시도하면 에러가 발생한다.

    readonly() 함수는 일반적으로 컴포넌트 내부에서 변경되지 않는 데이터를 관리할 때 사용된다.
    예를 들어, API 응답 데이터나 상수 값 등을 관리할 때 유용하게 사용할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  watchEffect()

    즉시 함수를 실행하고 의존성을 반응적으로 추적하며, 의존성이 변경될 때마다 다시 실행한다.

    타입

      function watchEffect(
        effect: (onCleanup: OnCleanup) => void,
        options?: WatchEffectOptions
      ): StopHandle

      type OnCleanup = (cleanupFn: () => void) => void

      interface WatchEffectOptions {
        flush?: 'pre' | 'post' | 'sync' // 기본 값: 'pre'
        onTrack?: (event: DebuggerEvent) => void
        onTrigger?: (event: DebuggerEvent) => void
      }

      type StopHandle = () => void

    세부 사항

      첫 번째 인자는 실행할 이펙트 함수다. 
      이펙트 함수는 인자로 클린업(cleanup) 콜백을 등록해 실행할 수 있는 함수를 받는다.
      클린업 콜백은 다음에 이펙트가 다시 실행되기 직전에 호출되며, 무효화된 사이드 이펙트를 정리하는 데 사용할 수 있다.
      예를들어 비동기 요청의 결과 대기중(pending)에 사용할 수 있다.

      두 번째 인자는 이펙트의 발생(flush) 타이밍을 조정하거나, 이펙트의 의존석을 디버그하는 데 사용할 수 있는 선택적 옵션 객체이다.

      기본적으로 와처는 컴포넌트 렌더링 직전에 실행된다. flush: 'post'를 설정하면 컴포넌트 렌더링 이후까지 와처가 지연된다.
      
        * 콜백 실행(flush) 타이밍
          반응형 상태를 변경하면 Vue 컴포넌트 업데이트와 사용자가 만든 감시자 콜백이 모두 실행된다.
          컴포넌트 업데이트와 유사하게, 사용자가 만든 감시자(watcher)콜백은 중복 호출을 방지하기 위해 일괄 처리된다.
          예를 들어, 감시하고 있는 배열에 동기적으로 천 개의 항목을 푸시할 때 감시자가 천 번 실행되는 것을 원하지 않을 것이다.

          기본적으로, 감시자의 콜백은 부모 컴포넌트 업데이트가 있을 경우 이후에 호출되고, 소유 컴포넌트의 DOM 업데이트 이전에 호출된다.
          이는 감시자 콜백 내에서 소유 컴포넌트의 DOM에 접근하려고 할 때, DOM이 업데이트 전 상태에 있을 것임을 의미한다.

          후처리 감시자

            Vue가 업데이트한 후에 감시자 콜백에서 소유 컴포넌트의 DOM에 접근하고자 한다면,
            flush: 'post' 옵션을 지정해야 한다.

              watch(source, callback, {
                flush: 'post'
              })

              watchEffect(callback, {
                flush: 'post'
              })

            flush: 'post' 옵션이 적용된  watchEffect()를 보다 간편하게 사용하기 위해서 watchPostEffect()를 사용할 수 있다.

              import { watchPostEffect } from 'vue'

              watchPostEffect(() => {
                /* Vue가 업데이트 된 후 실행됩니다 */
              })

            기본적으로 flush: 'pre'|"post" 옵션은 콜백을 버퍼링하여, 동일한 "틱(tick)"에서 여러 번 상태 변경이 되더라도, 마지막에 한 번만 호출된다.

              * 틱(tick)이란
                Vue.js에서의 tick은 Vue의 다음 렌더링 사이클에 특정 작업을 스케줄링하기 위한 메커니즘이다.
                Vue가 상태 변경을 응답하여 화면을 다시 그릴 때마다 렌더링 사이클이 발생한다.
                tick은 다음 렌더링 사이클이 시작될 때까지 기다리는 비동기 작업을 수행하는데 사용된다.

                보통 tick은 다음과 같은 상황에서 사용된다.
                1. 비동기 작업이 완료된 후에 UI 업데이트해야 할 때
                2. 다음 렌더링 사이클에서 데이터 변경 사항에 대한 후속 작업을 수행해야 할 때

                일반적으로 Vue에서는 nextTick() 함수를 사용하여 tick을 처리한다.
                nextTick() 함수를 사용하면 현재의 JavaScript 실행 컨텍스트가 완료된 후에 다음 렌더링 사이클에서의 작업을 스케줄링할 수 있다.

            동일한 틱 내에서 여러 번 상태 변경 시 마다 동기적으로 콜백을 호출해야 하는 경우,
            flush: 'sync' 옵션을 사용해야 한다. 단, 일반적으로 이러한 동작은 비효율적이므로 사용하려는 경우, 정말 필요한지 다시 한번 고민해봐야 한다.

              const count = ref(0)
              const callback = (val, preVal) => console.log('변경이 감지됨!', val, preVal)
              const options = { flush: 'sync' }

              watch(count, callback, options)

              count.value++
              // 이어서 callback이 실행됨
              count.value++
              // 역시 callback이 실행됨
              count.value++
              // 또 callback이 실행됨

          동기적 감시자

            Vue에서 동기적으로 실행되는 감시자(watcher)를 만드는 것도 가능하다.
            이 감시자는 Vue가 관리하는 업데이트 이전에 동기적으로 발동된다.

              watch(source, callback, {
                flush: 'sync'
              })

              watchEffect(callback, {
                flush: 'sync'
              })

            동기적 watchEffet()는 또한 watchSyncEffect()라는 편리한 별칭도 가지고 있다.

              import { watchSyncEffect } from 'vue'

              watchSyncEffect(() => {
                /* 반응형 데이터 변경 시 동기적으로 실행됨 */
              })

            * 주의해서 사용
              동기적 감시자는 배치 처리가 없으며, 반응형 변경이 감지될 때마다 트리거된다.
              간단한 boolean 값들을 감시하는 데 사용하는 것은 괜찮지만, 예를 들어 배열과 같이 동기적으로 여러 번 변경될 수 있는 데이터 소스에는 사용을 피해야 한다.

      드물지만 캐시 무효화와 같이 반응형 의존성이 변경될 때 즉시 와처를 트리거해야 하는 경우가 있을 수 있다.
      이는 flush: 'sync'를 사용하여 수행할 수 있다. 그러나 이 설정은 여러 프로퍼티가 동시에 업데이트되는 경우 성능 및 데이터 일관성 문제를 일으킬 수 있으므로 주의해서 사용해야 한다.

      반환 값은 이펙트가 다시 실행되지 않도록 호출할 수 있는 핸들 함수다.

    예제

      const count = ref(0)

      watchEffect(() => console.log(count.value))
      // -> logs 0

      count.value++
      // -> logs 1

      사이드 이펙트 정리

        watchEffect(async (onCleanup) => {
          const { response, cancel } = doAsyncWork(id.value)
          // `id`가 변경되면 `cancel`이 호출된다.
          // 이럴경우, 이전 요청이 완료되지 않고 여전히 대기중이라면,
          // 취소할 수 있다.(cancel 콜백 로직을 그렇게 정의한 경우)
          onCleanup(cancel)
          data.value = await response
        })
      
      감시자 중지하기

        const stop = watchEffect(() => {})

        // 감시자가 더 이상 필요하지 않을 때:
        stop()

      옵션

        watchEffect(() => {}, {
          flush: 'post',
          onTrack(e) {
            debugger
          },
          onTrigger(e) {
            debugger
          }
        })

      
      watchEffect()함수는 Vue 3에서 사용되는 반응형 이펙트(effect)를 생성하는 함수다.
      이 함수는 주어진 콜백 함수를 실행하고, 해당 함수 내에서 참조하는 반응형 데이터의 변경을 자동으로 감지하여 이후의 업데이트를 처리한다.

      watchEffect() 함수는 주어진 콜백 함수 내에서 참조하는 모든 반응형 데이터를 추적하며, 해당 데이터가 변경될 때마다 콜백 함수를 재실행한다.
      이를 통해 자동으로 데이터의 변경을 감지하고 콜백 함수를 실행하여 UI를 업데이트할 수 있다.

      예를 들어, 다음은 watchEffect() 함수를 사용하여 반응형 데이터의 변경을 감지하는 예제이다.

        import { reactive, watchEffect } from 'vue';

        export default {
          setup() {
            const state = reactive({
              count: 0
            });

            watchEffect(() => {
              console.log('Count changed:', state.count);
            });

            const increment = () => {
              state.count++;
            };

            return {
              state,
              increment
            };
          }
        };

      위의 예제에서 watchEffect() 함수는 state.count를 감시하고 있다. state.count가 변경될 때마다 콘솔에 메시지가 출력된다.
      이를 통해 watchEffect() 함수는 데이터의 변경을 감지하고 이에 따라 콜백 함수를 실행하여 부수적인 작업을 수행할 수 있다.

      watchEffect() 함수는 주로 컴포넌트 내에서 반응형 데이터의 변경을 감지하고 이에 따라 필요한 동작을 수행하는 데 사용된다.
      예를 들어, 데이터의 변경에 따라 UI를 업데이트하거나 외부 API호출을 트리거하는 등의 작업을 수행할 수 있다.

      watchEffect() 함수를 사용해야 하는 구체적인 사례

        컴포넌트 라이플사이클 후킹
          Vue3에서는 watchEffect()를 사용하여 컴포넌트가 마운트될 때나 언마운트될 때 등의 특정 시점에서 자동으로 실행되는 로직을 작성할 수 있다.
          이는 Vue2에서 사용되던 라이프사이클 훅과 유사한 기능을 제공한다.

          예를 들어, 다음은 컴포넌트가 마운트될 때마다 콘솔에 메시지를 출력하는 예제이다.

            import { watchEffect, onMounted } from 'vue';

            export default {
              setup() {
                onMounted(() => {
                  console.log('컴포넌트가 마운트되었습니다.');
                });

                watchEffect(() => {
                  console.log('반응형 데이터 변경됨');
                });

                return {};
              }
            };

          위의 예제에서 onMounted() 함수를 사용하여 컴포넌트가 마운트될 때 콘솔에 메시지를 출력하고, watchEffect() 함수를 사용하여 반응형 데이터의 변경을 감지하여
          해당 데이터가 변경될 때마다 콘솔에 메시지를 출력한다.

          이러한 패턴은 컴포넌트 라이프사이클 후킹과 관련된 작업을 수행할 때 사용될 수 있다.
          예를 들어, 컴포넌트가 마운트되었을 때 데이터를 초기화하거나, 언마운트될 때 리소스를 정리하는 등의 작업을 수행할 수 있다.
          watchEffect()를 사용하면 데이터의 변경을 감지하여 필요한 작업을 자동으로 수행할 수 있으므로 코드를 간결하고 유지보수하기 쉽게 만들 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  watchPostEffect()

    flush: 'post'옵션 값을 사용하는 watchEffect()의 별칭

    watchPostEffect() 함수는 Vue3에서 사용할 수 있는 함수 중 하나로, watchEffect()와 유사하게 동작하지만
    콜백 함수가 비동기적으로 실행된 후에 새로운 렌더링을 트리거한다. 이 함수는 Vue 3.2.0 버전에 도입되었다.

    watchPostEffect() 함수를 사용하면 비동기 작업을 수행한 후에 반응형 데이터의 변경을 감지하여 UI를 업데이트할 수 있다.
    주로 비동기 작업을 트리거한 후에 UI를 업데이트해야 할 때 사용된다.

    예를 들어, 다음은 watchPostEffect() 함수를 사용하여 비동기 작업을 수행한 후에 UI를 업데이트하는 예제이다.

      import { reactive, watchPostEffect } from 'vue';

      export default {
        setup() {
          const state = reactive({
            isLoading: true,
            data: null
          });

          watchPostEffect(async () => {
            state.data = await fetchData();
            state.isLoading = false;
          });

          return {
            state
          };
        }
      };

    위의 예제에서 watchPostEffect() 함수는 비동기 콜백 함수를 받아 해당 함수가 실행된 후에 반응형 데이터의 변경을 감지한다.
    비동기 함수가 실행된 후에 데이터가 변경되어 UI 업데이트가 필요한 경우에 사용된다.

    watchPostEffect() 함수는 watchEffect() 함수와 유사하지만 콜백 함수의 실행 타이밍이 다르다.
    watchEffect() 는 즉시 실행되지만, watchPostEffect() 는 비동기 함수가 완료된 후에 실행된다.

    참고로, watchPostEffect() 함수는 Vue 3.2.0 이상에서만 사용할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  watchSyncEffect()

    flush: 'sync' 옵션 값을 사용하는 watchEffect()의 별칭

    watchSyncEffect() 함수는 Vue 3.2.0 버전에서 도입된 함수로, 
    비동기 콜백 함수가 완료된 후에 새로운 렌더링을 트리거하지 않고 동기적으로 실행된다.
    이 함수는 watchEffect()와 비슷하게 동작하지만 비동기 콜백 함수가 완료된 후에 즉시 실행된다.

    watchSyncEffect() 함수를 사용하면 비동기 작업을 수행한 후에 즉시 UI를 업데이트할 수 있다.
    비동기 작업이 완료된 후에 UI 업데이트를 트리거하지 않고 즉시 업데이트를 수행해야 할 때 유용하다.

    예를 들어, 다음은 watchSyncEffect() 함수를 사용하여 비동기 작업을 수행한 후에 UI를 업데이트하는 예제이다.

      import { reactive, watchSyncEffect } from 'vue';

      export default {
        setup() {
          const state = reactive({
            isLoading: true,
            data: null
          });

          watchSyncEffect(() => {
            fetchData().then((result) => {
              state.data = result;
              state.isLoading = false;
            });
          });

          return {
            state
          };
        }
      };

    위의 예제에서 watchSyncEffect() 함수는 비동기 콜백 함수를 받아 해당 함수가 실행된 후에 즉시 실행된다.
    비동기 함수가 완료된 후에 데이터가 변경되어 UI 업데이트가 필요한 경우에 사용된다.

    watchSyncEffect() 함수는 비동기 작업이 완료된 후에 즉시 UI를 업데이트해야 할 때 사용된다.
    참고로 watchSyncEffect() 함수는 Vue 3.2.0 이상에서만 사용할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  watch()

  하나 이상의 반응형 데이터 소스를 감시하고, 소스가 변경되면 콜백 함수를 호출한다.

  타입

    // 하나의 소스 감시
    function watch<T>(
      source: WatchSource<T>,
      callback: WatchCallback<T>,
      options?: WatchOptions
    ): StopHandle

    // 여러 개의 소스 감시
    function watch<T>(
      sources: WatchSource<T>[],
      callback: WatchCallback<T[]>,
      options?: WatchOptions
    ): StopHandle

    type WatchCallback<T> = (
      value: T,
      oldValue: T,
      onCleanup: (cleanupFn: () => void) => void
    ) => void

    type WatchSource<T> =
      | Ref<T> // ref
      | (() => T) // getter
      | T extends object
      ? T
      : never // 반응형 객체

    interface WatchOptions extends WatchEffectOptions {
      immediate?: boolean // 기본 값: false
      deep?: boolean // 기본 값: false
      flush?: 'pre' | 'post' | 'sync' // 기본 값: 'pre'
      onTrack?: (event: DebuggerEvent) => void
      onTrigger?: (event: DebuggerEvent) => void
      once?: boolean // 기본 값: false (3.4+)
    }

  세부 사항

    watch()는 기본적으로 게이르다(lazy) 그러므로 콜백은 감시된 소스가 변경되었을 때만 호출된다.

    첫 번째 인자는 감시될 소스다. 소스는 다음 중 하나일 수 있다.
      1. 값을 반환하는 getter 함수
      2. ref
      3. 반응형 객체
      4. 또는 위에 나열한 것들의 배열

    두 번째 인자는 소스가 변경될 때 호출될 콜백이다.
      콜백은 "변경된 값", "이전 값", "사이드 이펙트 클린업 콜백을 등록하기 위한 함수" 이렇게 세 가지 인자를 받는다.
      클린업 콜백은 다음에 이펙트가 다시 실행되기 직전에 호출되며, 무효화된 사이드 이펙트를 정리하는 데 사용할 수 있다.
      예를 들어 비동기 요청의 결과 대기중(pending)에 사용할 수 있다.

      여러 소스를 감시할 때, 콜백은 소스 배열에 해당하는 변경된 값 배열, 이전 값 배열 두 개를 수신한다.

    세 번째 인자는 선택적이며, 다음을 지원하는 옵션 객체이다.
      1. immediate: 감시자가 생성되는 즉시 콜백이 호출된다. 최초 호출 시 이전 값은 undefined이다.
      2. deep: 소스가 객체인 경우, 깊은 변경사항에서도 콜백이 실행되도록 한다.
      3. flush: 콜백의 발생(flush) 타이밍을 조정한다.
      4. onTrack / onTrigger: 감시자의 의존성을 디버그한다. 
      5. once: 콜백을 한 번만 실행한다. 감시자는 첫 번째 콜백 실행 후 자동으로 중지한다.

    watchEffect()와 비교하여 watch()를 사용하면 다음을 수행할 수 있다.

      1. 사이드 이펙트를 게이르게(lazily) 실행한다.
      2. 어떤 상태가 변경되면 감시자가 다시 실행될지 더 자세하게 정의할 수 있다.
      3. 감시자는 상태의 전후 값을 알 수 있다.

    예제

      getter 를 감시

        const state = reactive({ count: 0 })
        watch(
          () => state.count,
          (count, prevCount) => {
            /* ... */
          }
        )

      ref를 감시

        const count = ref(0)
        watch(count, (count, prevCount) => {
          /* ... */
        })

      여러 소스를 감시할 때, 콜백은 소스 배열에 해당하는 변경된 값 배열, 이전 값 배열 두개를 수신한다.

        watch([fooRef, barRef], ([foo, bar], [prevFoo, prevBar]) => {
          /* ... */
        })

      getter 소스를 사용할 때, 감시자는 getter의 반환 값이 변경된 경우에만 실행된다.
      깊은 변경사항에서도 콜백을 실행하려면 { deep: true }를 사용하여 감시자를 딥 모드로 설정해야 한다.
      딥 모드에서 콜백이 깊은 변경사항으로 트리거가 된 경우, 세 값과 이전 겂은 동일한 객체이다.

        const state = reactive({ count: 0 })
        watch(
          () => state,
          (newValue, oldValue) => {
            // newValue === oldValue
          },
          { deep: true }
        )

      반응형 객체를 직접 감시할 때, 감시자는 자동으로 딥 모드가 된다.

        const state = reactive({ count: 0 })
        watch(state, () => {
          상태의 깊은 변경사항에도 트러거 됨
        })

      watch()는 watchEffect()와 동일한 발생(flush) 타이밍 및 디버깅 옵션을 공유한다.

        watch(source, callback, {
          flush: 'post',
          onTrack(e) {
            debugger
          },
          onTrigger(e) {
            debugger
          }
        })

      와처 종료하기

        const stop = watch(source, callback)

        // 더이상 필요하지 않을때:
        stop()

      부작용(Side effect) 제거

        watch(id, async (newId, oldId, onCleanup) => {
          const { response, cancel } = doAsyncWork(newId)
          //id가 변경되면 cancel이 호출되어 이전 요청이 아직 완료되지 않은 경우 취소한다.
          onCleanup(cancel)
          data.value = await response
        })


    watch() 함수는 Vue 3에서 사용되는 반응형 데이터의 변경을 감시하고, 해당 데이터가 변경될 때마다 특정 동작을 수행하는 함수다.
    주로 특정 데이터의 변경을 감지하여 이에 대응하는 동작을 수행할 때 사용된다.

    watch() 함수는 두 가지 형태로 사용할 수 있다.

    1. 단일 대상 감시
      특정 데이터를 감시하여 그 값의 변화를 확인하고, 그에 따라 콜백 함수를 실행한다.
      이 경우 첫 번째 매개변수로 감시할 데이터를 전달하고, 두 번째 매개변수로는 데이터가 변경될 때마다 실행할 콜백 함수를 전달한다.

    2. 여러 대상 감시
      여러 데이터를 감시하여 그 값의 변화를 확인하고, 그에 따라 콜백 함수를 실행한다.
      이 경우 첫 번째 매개변수로 감시할 여러 데이터의 객체를 전달하고, 두 번째 매개변수로는 실행 할 콜백 함수를 전달한다.
      콜백 함수의 인수는 감시할 데이터의 변경된 값을 포함한다.

    예를 들어, 다음은 watch() 함수를 사용하여 단일 대상 감시를 수행하는 예제이다.

      import { reactive, watch } from 'vue';

      const state = reactive({
        count: 0
      });

      watch(
        () => state.count,
        (newValue, oldValue) => {
          console.log(`count가 변경되었습니다: ${oldValue} → ${newValue}`);
        }
      );

      state.count++; // 콘솔 출력: "count가 변경되었습니다: 0 → 1"

    위의 예제에서 watch() 함수는 state.count 의 변경을 감시하고, 값이 변경될 때마다 해당 값을 출력하는 콜백 함수를 실행한다.

    여러 대상 감시를 수행하는 경우에는 객체 형태로 여러 데이터를 전달할 수 있다. 이러한 경우에는 콜백 함수의 첫 번째 인수로 변경된 데이터의 객체가 전달된다.
    







-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
