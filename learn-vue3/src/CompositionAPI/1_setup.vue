<template>
	<div></div>
</template>
<!--  

  setup()

  이 페이지의 컴포넌트의 setup 옵션 사용법을 설명한다.
  싱글 파일 컴포넌트에 컴포지션 API를 사용하는 경우, 보다 간결하고 인체공학적인 문법을 위해 <script setup> 사용을 권장한다.

  setup() 훅은 다음과 같은 경우, 컴포넌트에서 컴포지션 API 사용을 위한 진입점 역할을 한다.

  1. 빌드 과정 없이 컴포지션 API 사용.
  2. 옵션 API 컴포넌트에서 컴포지션 API 기반 코드와 통합

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  기본 사용법

    반응형 API를 사용하여 반응형 상태를 선언하고 setup()에서 객체를 반환하여 템플릿에 노출할 수 있다.
    반환된 객체의 속성은 컴포넌트 인스턴스에서 사용할 수 있다.(옵션 API가 사용되는 경우)

      <script>
      import { ref } from 'vue'

      export default {
        setup() {
          const count = ref(0)

          // 템플릿 및 기타 옵션 API 훅에 노출
          return {
            count
          }
        },

        mounted() {
          console.log(this.count) // 0
        }
      }
      </script>

      <template>
        <button @click="count++">{{ count }}</button>
      </template>

    setup에서 반환된 refs는 템플릿에서 접근할 때, 자동으로 얕은 언래핑되므로, 접근할 때 .value를 사용할 필요가 없다.
    또한 this에서 접근할 때, 같은 방식으로 언래핑 된다.

    setup()은 객체를 동기적으로 반환해야 한다. async setup()을 사용할 수 있는 유일한 경우는 컴포넌트가 Suspense 컴포넌트의 자손인 경우다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  Props에 접근하기

    setup 함수의 첫 번째 인자는 props이다. setup 함수 내부의 props는 반응형이며, 새 props가 전달되면 업데이트된다.

      export default {
        props: {
          title: String
        },
        setup(props) {
          console.log(props.title)
        }
      }

    props 객체를 분해할 경우, 분해 된 변수는 반응성을 잃게 된다. 따라서 항상 props.xxx 처럼 접근하는 것이 좋다.

    Props를 분해해야 하거나, 반응성을 유지하면서 외부 함수에 props를 전달해야 하는 경우 toRefs() 또는 toRef() 유틸리티 API를 사용하여 구현할 수 있다.

      import { toRefs, toRef } from 'vue'

      export default {
        setup(props) {
          // refs 객체로 `props`를 변환한 후, 분해 할당
          const { title } = toRefs(props)
          // `title`은 `props.title`을 추적하는 ref 입니다.
          console.log(title.value)

          // 또는, 하나의 `props` 속성만 ref로 변환할 수 있습니다.
          const title = toRef(props, 'title')
        }
      }

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  Setup Context

    setup 함수에 전달되는 두 번째 인자는 Setup Context 객체이다.
    컨텍스트 객체는 setup내부에서 유용할 수 있는 다른 값을 노출한다.

      export default {
        setup(props, context) {
          // 속성 (비-반응형 객체, $attrs에 해당함)
          console.log(context.attrs)

          // 슬롯 (비-반응형 객체, $slots에 해당함)
          console.log(context.slots)

          // 이벤트 발송 (함수, $emit에 해당함)
          console.log(context.emit)

          // 로컬 속성 노출 (함수)
          console.log(context.expose)
        }
      }

    컨텍스트 객체는 반응형이 아니며, 안전하게 분해 할당될 수 있다.

      export default {
        setup(props, { attrs, slots, emit, expose }) {
          ...
        }
      }

    1. context.attrs
      부모 컴포넌트에서 전달된 HTML 속성(attribute)등을 포함하는 객체이다.
      이를 통해서 자식 컴포넌트에서 부모 컴포넌트로 전달된 속성들을 동적으로 접근할 수 있다.

      사용 사례: 부모 컴포넌트에서 자식 컴포넌트로 HTML 속성을 전달할 때 사용된다.

      사용법: attrs 객체를 통해 부모 컴포넌트에서 전달된 속성들을 읽어올 수 있다. 
        예를 들어 <ChildComponent v-bind:title="titleProp" />와 같이 부모 컴포넌트에서 title 속성을 전달한 경우 
        setup() 함수 내에서 attrs.title을 통해 해당 속성 값을 읽어올 수 있다.

    2. context.slots
      부모 컴포넌트에서 전달된 하위 컴포넌트의 콘텐츠(slot)들을 포함하는 객체이다.
      이를 통해 하위 컴포넌트의 콘텐츠를 동적으로 조작하거나 렌더링할 수 있다.

      사용 사례: 부모 컴포넌트에서 자식 컴포넌트에게 콘텐츠를 전달할 때 사용된다.
        부모 컴포넌트에서 자식 컴포넌트의 특정 위치에 콘텐츠를 삽입하고자 할 때 유용하다.

      사용법: slots 객체를 통해 부모 컴포넌트에서 전달된 콘텐츠를 읽어올 수 있다.
        예를 들어, <ChildComponent>Hello, World!</ChildComponent>와 같이 부모 컴포넌트에서 'Hello, World!' 콘텐츠를 전달한 경우 
        'setup()' 함수 내에서 'slots.default()'를 호출하여 해당 콘텐츠를 렌더링할 수 있다.

    3. context.emit
      이벤트를 발생시키는 함수다. 자식 컴포넌트에서 이를 사용하여 부모 컴포넌트에게 이벤트를 전달할 수 있다.

      사용 사례: 자식 컴포넌트에서 부모 컴포넌트에게 이벤트를 전달할 때 사용된다.
        사용자의 상호작용이나 내부 상태 변경을 부모 컴포넌트로 전달할 때 유용하다.

      사용법: 'emit'함수를 사용하여 이벤트를 발생시킬 수 있다.
        예를 들어, 'emit('eventName', eventData)'와 같이 호출하여 'eventName'이벤트를 발생시킬 수 있다.

    4.expose
      부모 컴포넌트에게 노출할 함수 또는 속성을 정의하는 함수다.
      부모 컴포넌트에서 expose에 정의된 함수나 속성을 사용하여 자식 컴포넌트에게 접근할 수 있다.

      사용 사례: 자식 컴포넌트에서 부모 컴포넌트에게 노출할 함수나 속성을 정의할 때 사용된다.
        부모 컴포넌트에서 자식 컴포넌트의 특정 기능에 접근할 때 유용하다.

      사용법: 'expose()'함수를 사용하여 부모 컴포넌트에게 노출할 함수나 속성을 정의할 수 있다.
        자식 컴포넌트의 내부 상태나 함수를 부모 컴포넌트에게 노출하고자 할 때 사용된다.

    attr와 slots는 컴포넌트가 업데이트될 때, 항상 업데이트되는 스테이트풀(stateful) 객체다.
    즉, 구조를 분해하지 말고 attrs.x나 slots.x와 같이 속성을 참조해야 한다.
    또한 props와 달리 attrs와 slots의 속성은 반응형이 아니다.
    따라서 attrs 또는 slots의 변경 사항을 기반으로 하는 사이드 이펙트 onBeforeUpdate 생명 주기 훅 내에서 구현해야 한다.

    *스테이트풀(stateful) 객체란

      스테이트풀(stateful) 객체는 객체가 내부적으로 상태(state)를 유지하고 있는 객체를 가리킨다.
      이 상태는 객체의 메서드 호출, 사용자의 상호 작용 또는 외부 이벤트에 의해 변경될 수 있다.

      스테이트풀 객체는 주로 소프트웨어 개발에서 사용되며, 객체가 특정 상태에 있을 때 기능을 수행하거나 특정 동작을 수행하는 데 중점을 둔다.
      객체의 상태가 변경되면 해당 객체는 다른 동작을 수행하거나 다른 상태로 전이될 수 있다.

    퍼블릭 속성 노출하기

      expose 함수는 부모 컴포넌트가 템플릿 참조를 통해 자식 컴포넌트 인스턴스에 접근할 때, 노출되는 속성을 며시적으로 제한하기 위해 사용한다.

        export default {
          setup(props, { expose }) {
            // 인스턴스를 "닫힘" 상태로 설정
            // 예: 부모 컴포넌트에 아무것도 노출하지 않으려는 경우
            expose()

            const publicCount = ref(0)
            const privateCount = ref(0)
            // 선택적으로 로컬 상태를 노출
            expose({ count: publicCount })
          }
        }

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  렌더 함수와 함께 사용하기

    setup()은 범위 내 선언된 반응형 상태에 직접 접근할 수 있는 렌더 함수를 반환할 수도 있다.

      import { h, ref } from 'vue'

      export default {
        setup() {
          const count = ref(0)
          return () => h('div', count.value)
        }
      }

    렌더 함수를 반환하면, 다른 것을 반환할 수 없다. 내부적으로는 문제가 되지 않지만, 
    템플릿 참조를 통해 이 컴포넌트의 메서드를 부모 컴포넌트에 노출하려는 경우, 문제가 될 수 있다.

    이럴 때는 expose()를 호출하여 이 문제를 해결할 수 있다.

      import { h, ref } from 'vue'

      export default {
        setup(props, { expose }) {
          const count = ref(0)
          const increment = () => ++count.value

          expose({
            increment
          })

          return () => h('div', count.value)
        }
      }

    이제 increment 메서드는 템플릿 참조를 통해 부모 컴포넌트에서 사용할 수 있다.


-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
