<template>
	<div></div>
</template>
<!--  

  <script setup>

    <script setup>은 싱글 파일 컴포넌트(SFC) 내에서 컴포지션 API를 더 쉽게 읽거나 사용하기 위한 컴파일 타임 문법이다.
    SFC에서 컴포지션 API를 사용하는 경우, 권장되는 문법이다.
    일반적인 <script> 문법에 비해 많은 이점을 제공한다.

    1. 더 적은 상용구로 더 간결한 코드
    2. 순수 TypeScript를 사용하여 props 및 내보낼(emit) 이벤트를 선언하는 기능
    3. 더 나은 런타임 성능(템플릿은 중간 프록시 없이 동일한 범위의 렌더 함수로 컴파일됨)
    4. 더 나은 IDE 타입 추론 성능(언어 서버가 코드에서 타입을 추출하는 작업 감소)

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  기본 문법

    문법을 선택하려면 <script> 블록에 setup 속성을 추가해야 한다.

      <script setup>
      console.log('안녕, script setup!')
      </script>

    내부 코드는 컴포넌트의 setup() 함수 내용으로 컴파일된다. 즉, 컴포넌트를 처음 가져올 때 한 번만 실행되는 일반적인 <script>와 달리,
    <script setup> 내부의 코드는 컴포넌트의 인스턴스가 생성될 때마다 실행한다.

    템플릿에 노출되는 최상위 바인딩

      <script setup>을 사용할 때, 내부에서 선언된 모든 최상위 바인딩(변수, 함수 및 import포함)은 템플릿에서 직접 사용할 수 있다.

        <script setup>
        // 변수
        const msg = '안녕!'

        // 함수
        function log() {
          console.log(msg)
        }
        </script>

        <template>
          <button @click="log">{{ msg }}</button>
        </template>

      import도 같은 방식으로 노출된다. 즉, methods 옵션을 통해 노출하지 않고도 템플릿 표현식에서 import한 헬터 함수를 직접 사용할 수 있다.

        <script setup>
        import { capitalize } from './helpers'
        </script>

        <template>
          <div>{{ capitalize('hello') }}</div>
        </template>

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  반응형

    반응형 상태는 반응형 API를 사용하여 명시적으로 생성되어야 한다. setup() 함수에서 반환된 값과 유사하게, 템플릿에서 참조될 때 ref는 자동으로 언래핑된다.

      <script setup>
      import { ref } from 'vue'

      const count = ref(0)
      </script>

      <template>
        //템플릿에서 ref는 언래핑되어 .value 없이 접근 가능
        <button @click="count++">{{ count }}</button>
      </template>

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  컴포넌트 사용하기

    <script setup> 범위의 커스텀 컴포넌트 값은 태그 이름으로 사용할 수 있다.

      <script setup>
      import MyComponent from './MyComponent.vue'
      </script>

      <template>
        <MyComponent />
      </template>

    MyComponent는 변수처럼 참조된다고 생각해야한다. JSX를 사용한 적이 있는 경우, 멘탈 모델(mental model)과 유사하다.
    kebab-case에 해당하는 <my-component>도 템플릿에서 작동하지만, 일관성을 위해 PascalCase 컴포넌트 태그를 강력히 권장한다.
    또한 네이티브 커스텀 엘리먼트와 구별하는 데 도움이 된다.

    동적 컴포넌트

      컴포넌트는 문자열 키로 등록되는 대신 변수로 참조되므로, <script setup> 내에서 동적 컴포넌트를 사용할 때, 동적인 :is 바인딩을 사용해야 한다.

        <script setup>
        import Foo from './Foo.vue'
        import Bar from './Bar.vue'
        </script>

        <template>
          <component :is="Foo" />
          <component :is="someCondition ? Foo : Bar" />
        </template>

      삼항 표현식에서 컴포넌트를 변수로 사용할 수 있다.

    재귀 컴포넌트

      SFC는 파일 이름을 통해 암시적으로 자신을 참조할 수 있다. 예를 들어 FooBar.vue라는 파일을 템플릿에서 <FooBar/>로 자신을 참조할 수 있다.

      import한 컴포넌트보다 우선 순위가 낮다. 컴포넌트의 유추된 이름과 충돌하는 명명된 가져오기가 있는 경우, import할 때 별칭을 지정할 수 있다.

      import { FooBar as FooBarChild} from './components'

      * 재귀 컴포넌트
      컴포넌트 자체가 자신을 참조하는 상황을 말한다. 예를 들어, 파일 이름이 FooBar.vue인 컴포넌트는 템플릿에서 <FooBar/>로 자신을 참조할 수 있다.
      이는 Vue.js의 파일 이름을 통해 컴포넌트를 참조하는 암시적인 방식이다.
      이러한 방식으로 컴포넌트를 참조할 때, 해당 컴포넌트를 재귀적으로 사용할 수 있다.

    네임스페이스 컴포넌트

      <Foo.Bar>와 같이 점이 있는 컴포넌트 태그를 사용하여, 객체 내부에 중첩된 속성으로 컴포넌트를 참조할 수 있다.
      단일 파일에서 여러 컴포넌트를 가져올 때 유용하다.

        <script setup>
        import * as Form from './form-components'
        </script>

        <template>
          <Form.Input>
            <Form.Label>레이블</Form.Label>
          </Form.Input>
        </template>

      * 네임스페이스 컴포넌트
        점 표기법을 사용하여 객체 내부에 중첩된 속성으로 컴포넌트를 참조하는 것을 말한다.
        이것은 한 파일에서 여러 컴포넌트를 가져올 때 유용하다. 예를 들어, <Foo.Bar>와 같이 사용하여 Foo라는 객체 내에 있는 Bar 컴포넌트를 참조할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  커스텀 디렉티브 사용

    전역적으로 등록된 커스텀 디렉티브는 정상적으로 작동한다. 로컬 커스텀 디렉티브는 <script setup>으로 명시적으로 등록할 필요는 없지만, vNameOfDirective라는 네이밍 스키마를 따라야 한다.

      <script setup>
      const vMyDirective = {
        beforeMount: (el) => {
          // 엘리먼트(el)로 작업을 할 수 있음
        }
      }
      </script>
      <template>
        <h1 v-my-directive>이것은 제목입니다!</h1>
      </template>

    다른 곳에서 디렉티브를 가져오는 경우, 필요한 명명 체계에 맞게 이름을 바꿀 수 있다.

      <script setup>
      import { myDirective as vMyDirective } from './MyDirective.js'
      </script>

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  defineProps() & defineEmits()

    완전한 타입 추론을 지원하는 props및 emits와 같은 옵션을 선언하려면, <script setup> 내에서 자동으로 사용할 수 있는 defineProps 및 defineEmits API를 사용한다.

      <script setup>
      const props = defineProps({
        foo: String
      })

      const emit = defineEmits(['change', 'delete'])
      // ... setup 코드
      </script>

      - defineProps 및 defineEmits는 <script setup>내에서만 사용할 수 있는 컴파일 매크로이다.
        import 할 필요가 없으며, <script setup>이 처리될 때 컴파일 된다.

      - defineProps는 props 옵션과 동일한 값을 허용하고, defineEmits는 emits 옵션과 동일한 값을 허용한다.

      - defineProps 및 defineEmits는 전달된 옵션을 기반으로 적절한 타입 추론을 제공한다.

      - defineProps 및 defineEmits에 전달된 옵션은 setup에서 모듈 범위로 호이스트된다.
        따라서 옵션은 setup 범위에서 전달된 로컬 페이지 변수를 참조할 수 없다.
        그렇게 하면 컴파일에 에러가 발생한 Import한 바인딩의 모듈 범위에 있으므로 참조 할 수 있다.

    타입 전용 props/emit 선언

      props 및 emits는 리터럴 타입 인자를 defineProps 또는 defineEmits에 전달하여 순수 타입 문법을 사용하여 선언할 수도 있다.

        const props = defineProps<{
          foo: string
          bar?: number
        }>()

        const emit = defineEmits<{
          (e: 'change', id: number): void
          (e: 'update', value: string): void
        }>()

        // 3.3+: alternative, more succinct syntax
        const emit = defineEmits<{
          change: [id: number] // named tuple syntax
          update: [value: string]
        }>()

      - defineProps 또는 defineEmits는 런타임 선언 또는 타입 선언만 사용할 수 있다. 두 가지를 동시에 사용하면 컴파일 에러가 발생한다.

      - 타입 선언을 사용할 때 정적 분석에서 동등한 런타임 선언이 자동으로 생성되어, 이중 선언의 필요성을 제거하고 여전히 올바른 런타임 동작을 보장한다.

        - 개발 모드에서 컴파일러는 타입에서 해당 런타임 유효성 검사를 유추하려고 시도한다.
          예를 들어 foo: String은 foo: string 타입에서 유추된다. 타입이 가져온 타입의 참조인 경우, 컴파일러에 외부 파일 정보가 없기 때문에 추론된 결과는 foo: null(any 타입과 동일)이 된다.

        - prod 모드에서 컴파일러는 번들 크기를 줄이기 위해 배열 형식 선언을 생성한다.(여기서 props는 ['foo', 'bar']로 컴파일된다.)

        - 내보낼(emit) 코드는 여전히 유효한 타이핑이 있는 TypeScript이며, 다른 도구에서 추가로 처리할 수 있다.

      버전 3.2 이하에서 타입 파라미터는 타입 리터럴 또는 로컬 타입에 대한 참조로 제한된다.
      이 제한은 3.3에서 제거되었다. 3.3부터 Vue는 외부에서 가져온 것을 포함하여 가장 일반적인 타입에서 런타임 props를 유추할 수 있다.

    타입 선언을 사용할 때 기본 props 값

      타입 전용 defineProps 선언의 한 가진 단점은 props에 대한 기본값을 제공할 방법이 없다는 것이다. 이 문제를 해결하기 위해 withDefaults 컴파일러 메크로를 제공한다.

        export interface Props {
          msg?: string
          labels?: string[]
        }

        const props = withDefaults(defineProps<Props>(), {
          msg: '안녕!',
          labels: () => ['하나', '둘']
        })

      이것은 동등한 런타임 props default 옵션으로 컴파일된다. 
      또한 withDefaults 헬퍼는 기본값에 대한 타입 검사를 제공하고, 반환된 props 타입에 기본값이 선언된 속성의 선택적 플래그가 제거되었는지 확인한다.

    요약
    
      defineProps() 및 defineEmits()는 Vue 3의 <script setup> 블록에서 사용할 수 있는 컴파일러 매크로이다.
      이 매크로를 사용하면 props 및 emits를 강력하게 타입 지정할 수 있다.

      defineProps()는 props를 정의하는 데 사용된다. 이 매크로를 props 객체를 반환하며, 인자로는 props의 형태를 지정하는 객체를 받는다.
      이를 통해 완전한 추론을 제공하며, 타입 안전성을 보장한다.

      defineEmits()는 emits를 정의하는 데 사용된다. 이 매크로는 emit 함수를 반환하며, 인자로는 가능한 이벤트 이름을 포함하는 배열을 받는다.
      이를 통해 이벤트 이름에 대한 타입 검사를 제공하고, 타입 안전성을 보장한다.

      두 매크로 모두 <script setup> 블록 내에서만 사용할 수 있다. 이러한 매크로를 사용하면 Vue 컴포넌트의 props와 emits을 안전하게 처리할 수 있으며, 코드의 가독성과 유지 보수성을 향상시킨다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  defineModel() (3.4+)

    이 매크로는 브모 컴포넌트에서 v-model을 통해 사용될 수 있는 양방향 바인딩 prop을 선언하는 데 사용될 수 있다.
    예시 사용 방법은 컴포넌트 v-model 가이드 참고.

    내부적으로, 이 매크로는 모델 prop과 해당하는 값 업데이트 이벤트를 선언한다. 첫 번째 인수가 리터럴 문자열인 경우, 그것은 prop 이름으로 사용될 것이다.
    그렇지 않으며 prop 이름은 기본적으로 modelValue로 설정된다. 두 경우 모두 추가적인 객체를 전달할 수 있으며, 이는 prop의 옵션과 모델 ref의 값 변환 옵션을 포함할 수 있다.

      // "modelValue" prop 선언, 부모에 의해 v-model을 통해 사용됨
      const model = defineModel()
      // 또는: 옵션을 포함한 "modelValue" prop 선언
      const model = defineModel({ type: String })

      // 변형될 때 "update:modelValue" 이벤트 발생
      model.value = 'hello'

      // "count" prop 선언, 부모에 의해 v-model:count를 통해 사용됨
      const count = defineModel('count')
      // 또는: 옵션을 포함한 "count" prop 선언
      const count = defineModel('count', { type: Number, default: 0 })

      function inc() {
        // 변형될 때 "update:count" 이벤트 발생
        count.value++
      }

      * 경고
        defaultModel prop에 default 값을 설정하고, 부모 컴포넌트에서 이 prop에 대한 값을 제공하지 않으면, 부모와 자식 컴포넌트 간의 동기화 문제가 발생할 수 있다.
        아래 예시에서, 부모의 myRef는 값이 정의되지 않았지만 (undefined) 자식의 model은 1이다.

          //자식 컴포넌트
          const model = defineModel({ default: 1 })

          //부모 컴포넌트
          const myRef = ref()

          <Child v-model="myRef"></Child>

    Modifiers and Transformers

      v-model 지시문과 함께 사용되는 수정자에 접근하려면, definModel()의 변환 값을 구조 분해하는 방식을 사용할 수 있다.

        const [modelValue, modelModifiers] = defineModel()

        //v-model.trim에 해당
        if (modelModifiers.trim) {
          // ...
        }

      수정자가 존재할 때, 부모에게 값을 읽거나 동기화할 때 값을 변환할 필요가 있을 수 있다.
      이를 위해 get과 set 변환기 옵션을 사용하여 이를 달성할 수 있다.

        const [modelValue, modelModifiers] = defineModel({
          // 여기서는 get()이 필요하지 않으므로 생략
          set(value) {
            // .trim 수정자가 사용되면, 공백을 제거한 값을 반환
            if (modelModifiers.trim) {
              return value.trim()
            }
            // 그렇지 않으면, 값을 그대로 반환
            return value
          }
        })

    TypeScript와 함께 사용하기 (TS)

      defineProps및 defineEmits와 마찬가지로, defineMode은 모델 값과 수정자의 유형을 지정하기 위해 타입 인수를 받을 수 있다.

        const modelValue = defineModel<string>()
        //    ^? Ref<string | undefined>

        // 기본 모델 옵션, 'required'는 가능한 undefined 값을 제거함
        const modelValue = defineModel<string>({ required: true })
        //    ^? Ref<string>

        const [modelValue, modifiers] = defineModel<string, 'trim' | 'uppercase'>()
        //                 ^? Record<'trim' | 'uppercase', true | undefined>

    요약

      defineModel()은 Vue 3.4이상에서 사용할 수 있는 컴파일 매크로로, 양방향 데이터 바인딩을 제공하는 v-model 디렉티브와 함께 사용할 수 있는 prop을 선언하는 데 사용된다.
      이 매크로를 사용하면 모델 prop과 해당하는 값 업데이트 이벤트를 선언할 수 있다.

      기본적으로 defineModel()은 modelValue라는 이름의 prop을 선언하며, 값의 변경을 감지하고 부모 컴포넌트와 통신하기 위한 이벤트인 update:modelValue를 생성한다.
      예를 들어, <Child v-model="myRef"></Child>와 같이 사용할 수 있다.

      만약 prop 이름을 명시적으로 지정하고 싶다면, 첫 번째 매개변수로 prop의 이름을 전달할 수 있다.
      또한 두 번째 매개변수로는 prop의 옵션을 설정할 수 있다.

      특정 v-model 수정자에 접근하려면 defineModel()의 변환값을 구조 분해하여 사용할 수 있다. 또한 prop값을 변환해야 하는 경우 get과 set 변환기 옵션을 사용하여 이를 처리할 수 있다.

      defineModel()은 TypeScript와 함꼐 사용할 수 있으며, 모델 값 및 수정자의 유형을 지정하기 위해 타입 인수를 전달할 수 있다.
      예를 들어, const modelValue = defineModel<string>()와 같이 사용할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  defineExpose()

    <script setup>을 사용하는 컴포넌트는 기본적으로 닫혀 있다. 
    즉, 템플릿 참조 또는 $parent 체인을 통해 검색되는 컴포넌트의 공개 인스턴스는 <script setup> 내부에서 선언된 바인딩을 노출하지 않는다.

    <script setup> 컴포넌트의 속성을 명시적으로 노출하려면 defineExpose 컴파일러 매크로를 사용해야 한다.

      <script setup>
      import { ref } from 'vue'

      const a = 1
      const b = ref(2)

      defineExpose({
        a,
        b
      })
      </script>

    부모가 템플릿 참조를 통해 이 컴포넌트의 인스턴스를 가져오면, 검색된 인스턴스는 { a: number, b: number }모양이 된다.(참조는 일반 인스턴스와 마찬가지로 자동으로 언래핑됨)

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  defineOptions()

    이 매크로는 별도의 <script> 블록을 사용하지 않고 <script setup> 내에서 직접 컴포넌트 옵션을 선언하는 데 사용될 수 있다.

      <script setup>
      defineOptions({
        inheritAttrs: false,
        customOptions: {
          /* ... */
        }
      })
      </script>

    - 3.3+에서만 지원된다.
    - 이것은 매크로이다. 옵션은 모듈 범위로 상향 조정되며 <script setup>의 리터럴 상수가 아닌 로컬 변수에 접근할 수 없다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  defineSlots() (TS)

    이 매크로는 IDE에 슬롯 이름 및 porp 유형 확인을 위한 타입 힌트를 제공하는 데 사용될 수 있다.

    defineSlots()는 유형 매개변수만 받으며 런타임 인수는 받지 않는다. 유형 매개변수는 속성키가 슬롯 이름이고 값 유형이 슬롯 함수인 유형 리터럴이어야 한다.
    함수의 첫 번째 인수는 슬롯이 받기를 기대한는 prop이며, 템플릿에서 슬롯 prop의 유형으로 사용될 것이다.
    반환 유형은 현재 무시되며, any일 수 있지만, 향후 슬롯 콘텐츠 확인에 활용할 수 있다.

    또한 slots 객체를 반환하는데, 이것은 setup 컨텐스트에 노출되거나 useSlots()에 의해 반환된 slots 객체와 동일하다.

      <script setup lang="ts">
      const slots = defineSlots<{
        default(props: { msg: string }): any
      }>()
      </script>

    - 3.3+ 이상에서만 지원된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  useSlots() & useAttrs()

    <script setup> 내부에서 slots 및 attrs 사용은 템플릿에서 $slots 및 $attrs로 직접 접근할 수 있으므로 비교적 드물게 사용해야 한다.
    드물게 필요한 경우 useSlots 및 useAttrs 헬퍼를 각각 사용한다.

      <script setup>
      import { useSlots, useAttrs } from 'vue'

      const slots = useSlots()
      const attrs = useAttrs()
      </script>

    useSlots 및 useAttrs는 setupContext.slots 및 setupContext.attrs에 해당하는 항목을 반환하는 실제 런타임 함수이다.
    일반 컴포지션 API 함수에서도 사용할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  일반 <script>와 함께 사용

    <script setup>은 일반 <script>와 함께 사용할 수 있다. 다음을 수행해야 하는 경우 일반 <script>가 필요할 수 있다.

    - <script setup>에서 표현할 수 없는 옵션을 선엏나는 경우, 예를 들어 inheritAttrs 또는 플러그인을 통해 활성화된 커스텀 옵션이 있는 경우 (3.3 이상에서 defineOptions로 대체가능)
    - 명명된 export를 선언하는 경우
    - 사이드 이펙트를 실행하거나 한 번만 실행되어야 하는 객체를 만드는 경우

      <script>
      // 일반 <script>, 모듈 범위에서 실행(한 번만)
      runSideEffectOnce()

      // 추가 옵션 선언
      export default {
        inheritAttrs: false,
        customOptions: {}
      }
      </script>

      <script setup>
      // setup() 범위에서 실행(각 인스턴스에 대해)
      </script>

    동일한 컴포넌트에서 <script setup>과 <script>를 결합하는 지원은 위에서 설명한 시나리오로 제한된다.
    구체적으로,

    - prop 및 emits와 같이 <script setup>을 사용하여 이미 정의할 수 있는 옵션에 대해서는 별도의 <script> 섹션을 사용하지 않아야 한다.
    - <script setup> 내에서 생성된 변수는 컴포넌트 인스턴스에 프로퍼티로 추가되지 않으므로 옵션 API에서 엑세스할 수 없다. 이런식으로 API를 혼합하는 것은 강력히 권장하지 않는다.

    지원되지 않는 시나리오 중 하나에 해당하는 경우 <script setup>을 사용하는 대신 명시적인 setup() 함수로 전환하는 것을 고려해야 한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  최상위 await

    최상위 await는 <script setup> 내에서 사용할 수 있다. 결과 코드는 async setup()으로 컴파일된다.

      <script setup>
      const post = await fetch(`/api/post/1`).then((r) => r.json())
      </script>

    또한 이러한 표현식은 await 이후의 현재 컴포넌트 인스턴스 컨텍스트를 유지하는 형식으로 자동 컴파일된다.

    참고
      async setup()은 현재 실험적인 기능인 Suspense와 함께 사용해야 한다.
      향후 릴리스에서 이를 마무리하고 문서화될 계획이다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  Generics (TS)

    일반 유형 매개변수는 <script> 태그의 generic 속성을 사용하여 선언할 수 있다.

      <script setup lang="ts" generic="T">
      defineProps<{
        id: T
        list: T[]
      }>()
      </script>

    generic의 값은 TypeScript의 <...> 사이의 매개변수 목록과 정확히 동일하게 작동한다.
    예를 들어, 여러 매개변수, extend 제약 조건, 기본 유형을 사용하거나, 가져온 유형을 참조할 수 있다.

      <script
        setup
        lang="ts"
        generic="T extends string | number, U extends Item"
      >
      import type { Item } from './types'
      defineProps<{
        id: T
        list: U[]
      }>()
      </script>

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  제한사항

    - 모듈 실행 의미 체계의 차이로 인해 <script setup> 내부의 코드는 SFC의 컨텍스트에 의존한다. 이를 외부 .js 또는 .ts 파일로 옮기면 개발자와 도구 모두에게 혼란은 초래할 수 있다.
      따라서 <script setup>은 src 속성과 함께 사용할 수 없다.
    - <script setup>은 In-DOM Root Component Template을 지원하지 않는다.


-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
