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

    

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
