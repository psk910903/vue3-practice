<template>
	<div></div>
</template>
<!--  

  name

    컴포넌트에 대한 표시 이름을 명시적으로 선언한다.

    타입

      interface ComponentOptions {
        name?: string
      }

    상세 정보

      컴포넌트의 이름은 다음과 같은 용도로 사용된다.

      - 컴포넌트 자체 템플릿에서 재귀적인 자기 참조
      - Vue DevTools의 컴포넌트 검사 트리에서 표시
      - 경고 컴포넌트 추적에서 표시

      단일 파일 컴포넌트(Single-File Components)를 사용하는 경우, 컴포넌트는 이미 파일 이름에서 추론된 이름을 가지게 된다.
      예를 들어, MyComponent.vue라는 파일은 추론된 표시 이름이 "MyComponent"가 된다.

      또 다른 경우는 app.component를 사용하여 전역으로 컴포넌트를 등록하는 경우, 전역 ID가 자동으로 이름으로 설정된다.

      name 옵션을 사용하면 추론된 이름을 재정의하거나 이름을 명시적으로 제공할 수 있다.
      (빌드 도구를 사용하지 않거나 인라인된 비-SFC 컴포넌트를 사용하는 경우와 같이 이름을 추론할 수 없는 경우.)

      name이 명시적으로 필요한 경우가 하나 있다.
      <KeepAlive>의 include/exclude props에서 캐시 가능한 컴포넌트와 일치시킬 때이다.
      
      TIP
        3.2.34 버전부턴 <script setup> 을 사용하는 단일 파일 컴포넌트는 <KeepAlive>와 함께 사용될 때도 이름을 수동으로 선언할 필요 없이 파일 이름을 기반으로 자동으로 name 옵션을 추론한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    inheritAttrs

      기본 컴포넌트 속성 전달 동작을 활성화할지 여부를 제어한다.

      타입

        interface ComponentOptions {
          inheritAttrs?: boolean // 기본값: true
        }

      상세 정보

        기본적으로, 부모 범위에서 인식되지 않는 속성 바인딩은 "전달된다(fallthrough)".
        즉, 단일 루트 컴포넌트인 경우, 이러한 바인딩은 하위 컴포넌트의 루트 요소에 일반 HTML 속성으로 적용된다.
        대상 요소나 다른 컴포넌트를 감싸는 컴포넌트를 작성할 때 항상 원하는 동작은 아닐 수 있다.
        inheritAttrs를 false로 설정하여 이 기본 동작을 비활성화할 수 있다.
        속성은 $attrs 인스턴스 속성을 통해 사용할 수 있으며, v-bind를 사용해 비루트 요소에 명시적으로 바인딩할 수 있다.

      예시

        <script setup>을 사용하는 컴포넌트에서 이 옵션을 선언할 때는 defineOptions 매크로를 사용할 수 있다.

        <script setup>
        defineProps(['label', 'value'])
        defineEmits(['input'])
        defineOptions({
          inheritAttrs: false
        })
        </script>

        <template>
          <label>
            {{ label }}
            <input
              v-bind="$attrs"
              v-bind:value="value"
              v-on:input="$emit('input', $event.target.value)"
            />
          </label>
        </template>

    inheriAttrs는 부모 컴포넌트로부터 상속된 속성을 자식 컴포넌트에 적용할지 여부를 결정하는 옵션이다.

    기본적으로 Vue 컴포넌트는 부모로부터 상속된 모든 속성을 자식 컴포넌트의 루트 요소에 적용한다.
    하지만 때로는 이러한 동작이 원하지 않는 경우도 있다. inheriAttrs 옵션을 사용하면 이러한 동작을 제어할 수 있다.

    inheriAttrs값으로는 불리언(Boolean)또는 함수(Function)가 올 수 있다.

    - 불리언(Boolean)값으로 설정할 경우
      - true : 부모 컴포넌트로부터 상속된 속성을 자식 컴포넌트의 루트 요소에 자동으로 적용한다. 이것이 기본값이다.
      - false : 부모 컴포넌트로부터 상속된 속성을 자식 컴포넌트의 루트 요소에 적용하지 않는다. 이 옵션을 사용하면 자식 컴포넌트에서 직접 필요한 속성만 선택적으로 받을 수 있다.
    - 함수(Function)로 설정할 경우
      - 부모 컴포넌트로부터 상속된 속성을 동적으로 제어하기 위한 함수를 제공할 수 있다. 이 함수는 부모 컴포넌트로부터 상속된 속성을 나타내는 객체를 받아야 한다.
        이 함수는 다음과 같은 형식을 가진다.

          inheritAttrs: function(attrs) {
            // 필요한 로직을 수행한 후에 객체를 반환한다.
            return attrs;
          }

        예를 들어, 다음은 inheritAttrs를 사용하여 부모 컴포넌트로부터 상속된 class 속성을 자식 컴포넌트의 루트 요소에만 적용하는 방법을 보여준다.

          export default {
            inheritAttrs: false, // 상속된 속성을 자식 컴포넌트에 적용하지 않음
          };

        또한, 다음은 inheritAttrs를 사용하여 동적으로 부모 컴포넌트로부터 상속된 속성을 제어하는 방법을 보여준다.

          export default {
          inheritAttrs: function(attrs) {
            // 필요한 로직을 수행한 후에 객체를 반환
            // 여기서는 모든 상속된 속성을 그대로 반환하는 예시.
            return attrs;
          },
        };

      inheritAttrs를 사용하면 컴포넌트가 부모로부터 상속된 속성을 어떻게 다룰지를 조절할 수 있어서 유연한 컴포넌트 설계를 할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  components

    컴포넌트 인스턴스에서 사용 가능하도록 컴포넌트 등록하는 객체이다.

    타입

      interface ComponentOptions {
        components?: { [key: string]: Component }
      }

    예시

      import Foo from './Foo.vue'
      import Bar from './Bar.vue'

      export default {
        components: {
          // 간략한 표현
          Foo,
          // 다른 이름으로 등록
          RenamedBar: Bar
        }
      }

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  directives

    컴포넌트 인스턴스에서 사용 가능하도록 지시자를 등록하는 객체이다.

    타입

      interface ComponentOptions {
        directives?: { [key: string]: Directive }
      }

    예시

      export default {
        directives: {
          // 템플릿에서 v-focus 활성화
          focus: {
            mounted(el) {
              el.focus()
            }
          }
        }
      }

      <input v-focus>

      

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
