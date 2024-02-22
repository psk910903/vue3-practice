<template>
	<div></div>
</template>
<!--  

  isRef()

    값이 ref 객체인지 확인한다.

    타입

      function isRef<T>(r: Ref<T> | unknown): r is Ref<T>

    반환 타입은 type predicate이므로, isRef를 타입 가드로 사용할 수 있다.

      let foo: unknown
      if (isRef(foo)) {
        // foo의 타입은 Ref<unknown>으로 한정됨
        foo.value
      }

    isRef() 함수는 주어진 값이 Vue 3의 반응형 참조(ref) 객체인지 여부를 판별하는 함수다.
    반응형 참조(ref) 객체는 Vue3의 Composition API에서 사용되는 반응형 데이터 객체이다.

    isRef() 함수는 매개변수로 전달된 값이 반응형 참조(ref) 객체인지 여부를 불리언(true/false)값으로 반환한다.
    만약 주어진 값이 반응형 참조(ref)객체이면 true를 반환하고, 그렇지 않으면 false를 반환한다.

    주로 isRef() 함수는 특정 값이 반응형 데이터인지 여부를 확인할 때 사용된다.
    이는 개발자가 반응형 데이터를 다루는 과정에서 유용하게 사용될 수 있다.

    예를 들어, 다음은 isRef() 함수를 사용하여 값이 반응형 참조(ref) 객체인지 확인하는 예제이다.

      import { isRef, ref } from 'vue';

      const count = ref(0);

      console.log(isRef(count)); // true

      const name = 'John';

      console.log(isRef(name)); // false

    위의 예제에서 count는 반응형 참조(ref)객체이므로, isRef() 함수를 호출하면 true가 반환된다.
    그러나 name은 반응형 참조(ref)객체가 아니므로 false가 반환된다.


  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  unref()

    인자가 ref이면 내부 값을 반환하고, 그렇지 않으면 인자 자체를 반환한다.
    이것은 val - isRef(val) ? val.value: val과 같다.

    타입

      function unref<T>(ref: T | Ref<T>): T

    예제

      function useFoo(x: number | Ref<number>) {
        const unwrapped = unref(x)
        // unwrapped는 이제 확실히 숫자 입니다
      }

    unref() 함수는 Vue 3의 Composition API에서 사용되는 함수로, 반응형 데이터 객체를 일반 JavaScript값으로 변환하는 데 사용된다.

    반응형 데이터 객체는 ref() 함수나 reactive() 함수 등을 사용하여 생성된 객체로,
    Vue의 반응성 시스템에 의해 추적되고 있는 객체이다. 이러한 반응형 데이터 객체를 unref() 함수를 사용하여 일반 JavaScript 값으로 변환할 수 있다.

    만약 주어진 값이 반응형 데이터 객체라면 unref() 함수는 해당 객체의 내부 값에 접근하여 일반 JavaScript 값으로 반환한다.
    그러나 만약 주어진 값이 반응형 데이터 객체가 아니라면, 그 값을 그대로 반환하다.

    주로 unref() 함수는 템플릿에서 사용되는 반응형 데이터를 Composition API에서 사용할 때 유용하다.
    Composition API에서 템플릿의 반응형 데이터를 사용하려면 unref() 함수를 통해 일반 JavaScript 값으로 변환해야 한다.

    예를 들어, 다음은 unref() 함수를 사용하여 반응형 데이터 객체를 일반 JavaScript 값으로 변환하는 예제이다.

      import { ref, unref } from 'vue';

      const count = ref(0);

      console.log(unref(count)); // 0

    위의 예제에서 count는 반응형 데이터 객체이므로 unref() 함수를 호출하여 해당 객체의 내부값인 0을 반환한다.


  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    toRef()

    값 / ref / getter들을 정규화하는 데 사용할 수 있다.(3.3+)

    또는 반응형 객체의 속성에 대한 ref를 만드는 데 사용할 수도 있다.
    생성된 ref는 소스 속성과 동기화된다. 소스 속성을 변경하면 ref가 업데이트되고 그 반대의 경우도 마찬가지다.

    타입

      // 정규화 시그니처 (3.3+)
      function toRef<T>(
        value: T
      ): T extends () => infer R
        ? Readonly<Ref<R>>
        : T extends Ref
        ? T
        : Ref<UnwrapRef<T>>

      // 객체 속성 시그니처
      function toRef<T extends object, K extends keyof T>(
        object: T,
        key: K,
        defaultValue?: T[K]
      ): ToRef<T[K]>

      type ToRef<T> = T extends Ref ? T : Ref<T>

    예제

      정규화 시그니처(3.3+)

        //  기존 참조를 있는 그대로 반환함
        toRef(existingRef)

        // .value로 접근했을 때 getter를 호출하는 읽기 전용 ref를 만듦
        toRef(() => props.foo)

        // 함수가 아닌 값으로부터 일반적인 ref들을 만듦
        // ref(1) 과 동일
        toRef(1)

      객체 속성 시그니처

        const state = reactive({
          foo: 1,
          bar: 2
        })

        // 원래 속성과 동기화되는 양방향 ref
        const fooRef = toRef(state, 'foo')

        // ref를 변경하면 원본도 업데이트 됨
        fooRef.value++
        console.log(state.foo) // 2

        // 원본을 변경하면 ref도 업데이트 됨
        state.foo++
        console.log(fooRef.value) // 3

      이것은 다음과 다름에 주의해야 한다.

        const fooRef = ref(state.foo)

      위의 ref는 state.foo와 동기화되지 않는다. ref()가 일반 숫자 값을 수신하기 때문이다.

      toRef()는 컴포저블 함수에 prop을 ref로 전달하려는 경우에 유용하다.

        <script setup>
        import { toRef } from 'vue'

        const props = defineProps(/* ... */)

        // `props.foo`를 ref로 변환한 다음 컴포저블 함수에 전달
        useSomeFeature(toRef(props, 'foo'))

        // getter 문법 - 3.3 이상 버전에서 권장됨
        useSomeFeature(toRef(() => props.foo))
        </script>

      toRef가 컴포넌트 props와 함께 사용되면, props 변경에 대한 일반적인 제한 사항이 계속 적용된다.
      ref에 새 값을 할당하려는 시도는 prop을 직접 수정하려는 것과 동일하며 허용되지 않는다.
      이런 경우에는 computed()에 get과 set을 선언하여 사용하는 것으로 구현할 수 있다.

      객체 속성 시그니처를 사용할 때 toRef()는 원본 속성이 현재 존재하지 않더라도 사용 가능한 ref를 반환하다.
      이를 통해 toRefs에서 감지되지 않는 선택적 속성들을 처리할 수 있게된다.

    
    toRef() 함수는 Vue 3에서 사용되는 함수 중 하나로, 기존의 반응형 데이터를 참조하는 새로운 반응형 데이터를 생성하는 데 사용된다.

    일반적으로 toRef() 함수는 컴포넌트의 반응형 데이터 중에서 특정 속성을 참조하여 새로운 반응형 데이터를 만들 때 사용된다.
    이때 생성된 새로운 반응형 데이터는 원본 데이터의 변경을 반영하며, 원본 데이터와 동일한 반응성을 유지한다.

    toRef() 함수의 첫 번째 매개변수로는 원본 반응형 데이터 객체를 전달하고,
    두 번째 매개변수로는 원본 데이터 객체에서 참조할 속성 이름을 전달한다.
    그러면 toRef() 함수는 이 속성을 참조하는 새로운 반응형 데이터 객체를 반환한다.

    예를 들어, 다음 toRef() 함수를 사용하여 컴포넌트의 반응형 데이터에서 특정 속성을 참조하는 새로운 반응형 데이터를 생성하는 예제이다.

      import { ref, toRef } from 'vue';

      const user = ref({
        name: 'John',
        age: 30
      });

      const userName = toRef(user.value, 'name');

      console.log(userName.value); // 'John'

      // 원본 데이터 변경
      user.value.name = 'Alice';

      console.log(userName.value); // 'Alice' (원본 데이터 변경을 반영함)

    위의 예제에서 userName은 user객체의 name 속성을 참조하는 새로운 반응형 데이터다.
    user 객체의 name 속성이 변경되면 userName도 변경 사항을 반영하여 동일한 값을 가지게 된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  toRefs()

    반응형 객체를 일반 객체로 변환하고, 반환된 일반 객체의 각 속성은 원본 객체(반응형 객체)의 속성이 ref된 것이다.
    각 개별 ref는 toRef()를 사용하여 생성된다.

    타입

      function toRefs<T extends object>(
        object: T
      ): {
        [K in keyof T]: ToRef<T[K]>
      }

      type ToRef = T extends Ref ? T : Ref<T>

    예제

      const state = reactive({
        foo: 1,
        bar: 2
      })

      const stateAsRefs = toRefs(state)
      /*
      stateAsRefs의 타입: {
        foo: Ref<number>,
        bar: Ref<number>
      }
      */

      // 원본 속성이 ref와 "연결됨"
      state.foo++
      console.log(stateAsRefs.foo.value) // 2

      stateAsRefs.foo.value++
      console.log(state.foo) // 3

    toRefs는 컴포저블 함수에서 반응형 객체를 반환하면, 이것을 사용하는 컴포넌트가 반응형을 잃지 않고 분해 할당 및 확장 할 수 있어 유용하다.

      function useFeatureX() {
        const state = reactive({
          foo: 1,
          bar: 2
        })

        // ...state를 사용하여 작동하는 로직

        // 반환할 때 refs로 변환
        return toRefs(state)
      }

      // 반응형을 잃지 않고 분해 할당 가능
      const { foo, bar } = useFeatureX()

      toRefs는 호출 시 소스 객체에서 열거 가능한 속성만 참조로 생성한다. 
      아직 존재하지 않을 수 있는 속성에 대한 참조를 생성하려면 toRef를 사용해야 한다.

    toRefs() 함수는 Vue 3에서 제공하는 함수 중 하나로, 반응형 데이터 객체를 일반 JavaScript 객체로 변환하는 데 사용된다.
    이 함수는 객체의 각 속성을 반응형 데이터에서 참조할 수 있는 형태로 변환하여 반환한다.

    일반적으로 Vue 3에서는 reactive() 함수를 사용하여 반응형 데이터 객체를 생성한다. 
    그러나 때로는 외부에서 가져온 데이터를 컴포넌트 내부에서 다룰 때 이러한 데이터를 반응형으로 유지하면서 컴포넌트 내부에서 사용해야할 때가 있다.

    이때 toRefs() 함수를 사용하여 외부 데이터를 컴포넌트 내부에서 반응형으로 다룰 수 있다.
    toRefs()함수는 외부 데이터를 받아서 각 속성을 ref() 함수로 감싸서 반환하므로, 반환된 객체의 각 속성은 반응형 데이터의 형태를 가지게 된다.
    이를 통해 외부 데이터의 변경을 추적하면서 컴포넌트의 반응성을 유지할 수 있다.
    

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
