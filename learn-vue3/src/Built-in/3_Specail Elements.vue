<template>
	<div></div>
</template>
<!--  

  빌트인 특수 엘리먼트

    컴포넌트가 아님
    <component> <slot>은 컴포넌트와 유사한 기능이며, 템플릿 문법의 일부이다.
    이것들은 진정한 컴포넌트가 아니며, 템플릿 컴파일 중에 편집된다. 따라서 일반적으로 소문자로 작성된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  <component>

    동적 컴포넌트 또는 엘리먼트를 렌더링하기 위한 "메타 컴포넌트"이다.

    Props

      interface DynamicComponentProps {
        is: string | Component
      }

    세부 사항
    
      is라는 prop의 값으로 렌더링할 실제 컴포넌트가 결정된다.

      문자열인 경우, HTML 태그 이름 또는 컴포넌트로 등록된 이름일 수 있음

      컴포넌트의 정의에 직접 바인딩될 수도 있음.

    예제

      등록된 이름으로 컴포넌트 렌더링(옵션 API)

        <script>
        import Foo from './Foo.vue'
        import Bar from './Bar.vue'

        export default {
          components: { Foo, Bar },
          data() {
            return {
              view: 'Foo'
            }
          }
        }
        </script>

        <template>
          <component :is="view" />
        </template>

      정의에 따른 컴포넌트 렌더링(<script setup>이 있는 컴포지션 API)

        <script setup>
        import Foo from './Foo.vue'
        import Bar from './Bar.vue'
        </script>

        <template>
          <component :is="Math.random() > 0.5 ? Foo : Bar" />
        </template>

      HTML 엘리먼트 렌더링

        <component :is="href ? 'a' : 'span'"></component>

      빌드인 컴포넌트는 모두 is에 전달할 수 있지만, 이름으로 전달하려면 등록해야 한다. 예를 들어

        <script>
        import { Transition, TransitionGroup } from 'vue'

        export default {
          components: {
            Transition,
            TransitionGroup
          }
        }
        </script>

        <template>
          <component :is="isGroup ? 'TransitionGroup' : 'Transition'">
            ...
          </component>
        </template>

      이름이 아닌 컴포넌트 자체를 is에 전달하는 경우, 등록이 필요하지 않다.(예를 들어 <script setup>에서).

      v-model이 <component> 태그에 사용되면, 템플릿 컴파일러는 다른 컴포넌트와 마찬가지로 이를 modelValue prop및 update:modeValue 이벤트 리스너로 확장한다.
      그러나 이것은 <input> 또는 <select>와 같은 기본 HTML 엘리먼트와 호환되지 않는다. 
      결과적으로 동적으로 생성된 기본 엘리먼트와 함께 v-model을 사용하면 작동하지 않는다.

        <script setup>
        import { ref } from 'vue'

        const tag = ref('input')
        const username = ref('')
        </script>

        <template>
          //'input'이 기본 HTML 엘리먼트이므로 작동하지 않음
          <component :is="tag" v-model="username" />
        </template>

      실제로 기본 양식(form) 필드가 일반적으로 실제 앱의 컴포넌트에 래핑되기 때문에 이러한 예외적인 경우는 일반적이지 않다.
      네이티브를 직접 사용해야 하는 경우, v-model을 속성과 이벤트로 수동으로 분할할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  <slot>

    템플릿의 슬롯 컨텐츠를 내보낼 지점을 나타낸다.

    Props

      interface SlotProps {

        범위가 지정된 슬롯의 인자로 전달하기 위해
        <slot>에 전달된 모든 props
        [key: string]: any

        슬롯 이름을 지정
        name?: string
      }

    세부 사항
    
      <slot>엘리먼트는 name 속성을 사용하여 슬롯 이름을 지정할 수 있다. name을 지정하지 않으면 기본 슬롯으로 렌더링된다.
      슬롯 엘리먼트에 전달된 추가 속성은 부모 내부에서 범위가 정의된 슬롯에 슬롯 props로 전달된다.

      엘리먼트는 일치하는 슬롯의 컨텐츠로 대체된다.

      Vue 템플릿의 <slot> 엘리먼트는 JavaScript로 컴파일되므로 네이티브 <slot> 엘리먼트와 혼동하면 안된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  <template>

    <template> 태그가 DOM에 렌덩링되지 않는 placeholder로 사용된다.
    즉, 이 태그 안에서 작성된 요소들은 DOM에 렌더링되지 않지만, Vue의 템플릿 디렉티브들과 함께 사용될 때 특별한 취급을 받는다.

    세부사항

      <template>의 이런 특수한 취급은 다음 디렉티브들과 함께 사용될때만 적용된다.

        - v-if, v-else-if, v-else
        - v-for
        - v-slot

      만약 이런 디렉티브가 없다면, 네이티브 <template> 엘리먼트로 렌더링된다.

      v-for가 있는 <template>는 key 속성도 가질 수 있다. 다른 모든 속성과 디렉티브는 해당 엘리먼트가 없으면 의미가 없으므로 버려진다.

      싱글 파일 컴포넌트에서 최상위 <template> 태그는 전체 템플릿을 래핑하는 역할을 한다.
      이 최상위 태그는 템플릿 자체의 일부가 아니며, 지시문과 같은 템플릿 문법을 지원하지 않는다.
      그저 템플릿을 구성하는 부분을 래핑하는 역할을 한다.

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
