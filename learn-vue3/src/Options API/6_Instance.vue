<template>
	<div></div>
</template>
<!--  

  컴포넌트 인스턴스

    info 
      이 페이지는 컴포넌트의 공개적인(public) 인스턴스인 this에 노출되는 빌트인 속성 및 메서드에 관한 문서이다.
      이 페이지에 나열된 모든 속성은 읽기 전용이다.($data의 중첩 속성 제외)

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $data

    data 옵션에서 반환된 객체로 컴포넌트에 의해 반응형이 된다. 컴포넌트 인스턴스는 데이터 객체의 속성 접근을 프록시한다.
    예를 들면, 컴포넌트 인스턴스는 데이터 객체의 속성 this.$data.a를 this.a와 같이 바로 접근할 수 있도록 한다.

    타입

      interface ComponentPublicInstance {
        $data: object
      }

    $data는 Vue 컴포넌트의 데이터를 담고 있는 객체이다. 이 객체는 컴포넌트의 data옵션에 정의된 모든 데이터 속성을 포함하고 있다.
    일반적으로 $data 객체는 컴포넌트 내부에서 데이터를 접근하거나 조작해야 할 때 사용된다. 
    예를 들어, 특정 데이터 속성을 콘솔에 출력하려면 $data 를 사용하여 해당 데이터에 엑세스할 수 있다.

    예를 들어, 다음과 같이 컴포넌트 내부에서 $data를 사용하여 데이터를 출력할 수 있다.

      export default {
        data() {
          return {
            message: 'Hello, Vue!'
          };
        },
        mounted() {
          console.log(this.$data.message); // 'Hello, Vue!'를 콘솔에 출력
        }
      };

    $data 객체는 Vue 컴포넌트의 반응형 데이터를 직접적으로 조작하는 것을 피하는 것이 좋다.
    Vue는 반응성 시스템을 효율적으로 관리하고 있는데, $data를 사용하여 직접 데이터를 조작하면 Vue의 반응성 시스템을 우회할 수 있어서 예기치 않은 동작을 유발할 수 있다.
    대신에 데이터를 업데이트하는 메소드나 computed 속성을 사용하여 데이터를 변경하는 것이 좋다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $props

    현재 컴포넌트의 처리(선언, 할당 및 초기화)된 props를 나타내는 객체이다.

    타입

      interface ComponentPublicInstance {
        $props: object
      }

    세부 사항

      props 옵션을 통해 선언된 props만 포함된다. 컴포넌트 인스턴스는 props 객체의 속성 접근을 프록시한다.
      
    $props는 부모 컴포넌트로부터 전달된 속성(prop)을 담고 있는 객체이다.
    이 객체는 컴포넌트 인스턴스의 속성으로 제공되며, 해당 컴포넌트가 부모로부터 전달받은 모든 속성을 포함한다.

    부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달할 때, 속성(prop)을 사용한다. 자식 컴포넌트에서는 이러한 속성을 $props를 통해 접근하여 사용할 수 있다. 
    $props를 사용하여 컴포넌트에 전달된 속성을 읽을 수 있다.

    예를 들어, 다음과 같이 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달하고, 자식 컴포넌트에서 $props를 사용하여 해당 페이지를 읽을 수 있다.

      // ParentComponent.vue
      <template>
        <ChildComponent message="Hello, Vue!" />
      </template>

      <script>
      import ChildComponent from './ChildComponent.vue';

      export default {
        components: {
          ChildComponent
        }
      };
      </script>

      // ChildComponent.vue
      <script>
      export default {
        props: {
          message: String
        },
        mounted() {
          console.log(this.$props.message); // 'Hello, Vue!'를 콘솔에 출력
        }
      };
      </script>


    $props 객체는 Vue 컴포넌트의 속성(prop)을 변경하지 않으며 읽기 전용이다. 이 객체를 통해 부모 컴포넌트로부터 전달된 데이터를 안전하게 읽을 수 있지만, 변경할 수는 없다. 
    만약 자식 컴포넌트에서 속성을 변경하고 싶은 경우, 해당 속성을 데이터로 다루고 부모 컴포넌트에게 이를 전달하기 위해 이벤트를 발생시켜야 한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $el

    컴포넌트 인스턴스가 관리하는 루트 DOM 노드이다.

    타입

      interface ComponentPublicInstance {
        $el: Node | undefined
      }

    세부 사항

      $el은 mounted될 때까지 undefined이다.

      - 싱글 루트 엘리먼트가 있는 컴포넌트의 경우, $el은 해당 엘리먼트를 가리킨다.
      - 텍스트 루트가 있는 컴포넌트의 경우, $el은 텍스트 노드를 가리킨다.
      - 여러 루트 노드가 있는 컴포넌트의 경우, $el은 Vue가 DOM(텍스트 노드 또는 SSR 하이드레이션 모드의 주석 노드)에서 컴포넌트의 위치를 추적하는 데 사용하는 플레이스홀더 (placeholder)DOM 노드이다.

      TIP
        엘리먼트에 일관적으로 접근해야 할 때, $el보다 템플릿 참조를 사용하는 것이 권장된다.

    $el은 컴포넌트의 루트 DOM 요소를 가리키는 속성이다. 이 속성은 Vue 인스턴스가 마운트된 후에 사용할 수 있다.

    일반적으로 Vue 컴포넌트의 템플릿은 가상 DOM(Virtual DOM)으로 변환되어 실제 DOM에 적용된다.
    이 과정에서 Vue는 컴포넌트의 템플릿을 실제 DOM 요소로 렌더링하고, 해당 요소는 Vue 인스턴스의 $el 속성에 연결된다.

    $el 속성을 사용하면 컴포넌트가 실제로 렌더링된 DOM요소에 직접 엑세스할 수 있다.
    이것은 주로 외부 라이브러리와의 통합이나 특정 DOM 조작 작업이 필요한 경우에 유용하다.

    예를 들어, 특정 이벤트가 발생했을 때 컴포넌트의 DOM 요소에 직접 접근하여 스타일을 변경하거나 클래스를 추가하는 등의 작업을 수행할 수 있다.
    하지만 주의할 점은 Vue의 반응성 시스템을 우회하고 직접 DOM을 조작하는 것은 Vue의 철학과 맞지 않을 수 있으므로 신중하게 사용해야 한다.

    아래는 $el을 사용하여 컴포넌트의 DOM 요소에 접근하는 간단한 예시이다.

      <template>
        <div>{{ message }}</div>
      </template>

      <script>
      export default {
        data() {
          return {
            message: 'Hello, Vue!'
          };
        },
        mounted() {
          // 컴포넌트가 마운트된 후에 $el을 사용하여 DOM 요소에 접근
          this.$el.style.color = 'red'; // 텍스트 색상을 빨간색으로 변경
        }
      };
      </script>

    위 코드에서 mounted 라이프사이클 훅에서 $el 속성을 사용하여 컴포넌트의 루트 DOM 요소에 직접 접근하여 텍스트 색상을 변경하고 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $options

    현재 컴포넌트 인스턴스를 인스턴스화하는 데 사용된, 처리된 컴포넌트 옵션이다.

    타입

      interface ComponentPublicInstance {
        $options: ComponentOptions
      }

    세부 사항

      $options 객체는 컴포넌트의 처리된 옵션을 노출하며, 이것은 아래와 같은 소스의 병합 결과이다.

      - 전역 믹스인
      - extends 기반의 컴포넌트
      - 컴포넌트 믹스인

      일반적으로 커스텀 컴포넌트 옵션을 지원하는 데 사용된다.

        const app = createApp({
          customOption: 'foo',
          created() {
            console.log(this.$options.customOption) // => 'foo'
          }
        })

    $options 속성은 컴포넌트의 옵션 객체를 포함하는 속성이다. 
    이 속성은 Vue 인스턴스가 생성될 때 컴파일된 옵션 객체를 포함하며, 컴포넌트가 생성될 때 동적으로 변경되지 않는다.

    $option 객체에는 컴포넌트의 옵션 및 데이터가 포함되어 있다.
    주요 속성으로는 data, computed, methods, watch, props 등이 있다.
    또한 컴포넌트의 라이프사이클 훅과 사용자 정의 디렉티브도 포함될 수 있다.

    아래는 간단한 컴포넌트에서 $options를 사용하는 예시이다.

      <template>
        <div>{{ message }}</div>
      </template>

      <script>
      export default {
        data() {
          return {
            message: 'Hello, Vue!'
          };
        },
        created() {
          // $options를 사용하여 컴포넌트의 옵션 객체에 접근
          console.log(this.$options.data()); // { message: 'Hello, Vue!' }
          console.log(this.$options.methods); // { }
          console.log(this.$options.created); // [Function: created]
        }
      };
      </script>

    위 예시에서 created 라이프사이클 훅에서 $options 를 사용하여 컴포넌트의 데이터 및 메서드를 확인할 수 있다.
    이를 통해 Vue 인스턴스의 옵션을 동적으로 확인하거나 접근할 수 있다.

    하지만 주의할 점은 $options를 사용하여 컴포넌트의 옵션을 동적으로 변경하는 것은 권장되지 않는다.
    이 속성은 컴포넌트가 생성될 때 한 번만 설정되며, 이후 변경되지 않는다.
    컴포넌트 옵션을 변경해야 하는 경우에는 다른 방법을 고려해야 한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $parent

    현재 인스턴스에 부모 인스턴스가 있는 경우, 부모 인스턴스를 나타낸다.
    현재 인스턴스가 루트 인스터일 경우에는 null 이 된다.

    타입

      interface ComponentPublicInstance {
        $parent: ComponentPublicInstance | null
      }

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $root

    현재 컴포넌트 트리의 루트 컴포넌트 인스턴스이다.
    현재 인스턴스에 부모가 없으면, 값은 현재 인스턴스 자체가 된다.

    타입

      interface ComponentPublicInstance {
        $root: ComponentPublicInstance
      }

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $slots

    부모 컴포넌트로부터 전달된 slots을 나타내는 객체이다.

    타입

      interface ComponentPublicInstance {
        $slots: { [name: string]: Slot }
      }

      type Slot = (...args: any[]) => VNode[]

    세부 사항

      일반적으로 렌더 함수를 수동으로 작성할 때 사용되지만, 슬롯 유무 확인에도 사용할 수 있다.

      this.$slots은 각 슬롯의 이름에 해당하는 함수가 노출되며, vnode(가상노드)로 구성된 배열을 반환한다.
      기본 슬롯은 this.$slots.default로 표시된다.

      슬롯이 범위가 지정된 슬롯이면, 슬롯 함수에 전달된 인자를 슬롯 prop으로 사용할 수 있다.

    $slots 속성은 현재 컴포넌트에 전달된 슬롯의 컨텐츠를 포함하는 속성이다.
    슬롯은 컴포넌트의 템플릿에서 사용자가 지정한 특정 부분에 다른 컴포넌트나 요소를 삽입할 수 있는 기능을 제공한다.

    일시적으로 부모 컴포넌트에서 자식 컴포넌트에 컨텐츠를 전달할 때 사용된다. 자식 컴포넌트에서는 $slots 속성을 사용하여 전달된 슬롯의 내용에 접근할 수 있다.

    아래는 간단한 예시이다.

      //ParentComponent.vue
      <template>
        <child-component>
          //'default' 슬롯에 컨텐츠를 전달
          <p>Hello from parent component</p>
        </child-component>
      </template>

      //ChildComponent.vue
      <template>
        <div>
          //'default' 슬롯의 컨텐츠를 출력
          <slot></slot>
        </div>
      </template>

      <script>
      export default {
        mounted() {
          // $slots를 사용하여 'default' 슬롯의 컨텐츠에 접근
          console.log(this.$slots.default); // [VNode]
        }
      };
      </script>

    위 예시에서 ParentComponent에서 child-component 를 사용할 때 <p>Hello from parent component</p> 이라는 컨텐츠를 전달했다.
    이 컨텐츠는 ChildComponent에서 <slot/>을 통해 default 슬롯에 삽입되며, ChildComponent의 mounted 훅에서 $slots.default를 통해 해당 슬롯의 컨텐츠에 접근할 수 있다.

    $slots 속성을 통해 다양한 슬롯에 전달된 컨텐츠에 접근할 수 있다. 또한 이름이 지정된 슬롯을 사용하는 경우 $slots 객체의 다른 속성을 통해 해당 슬롯의 컨텐츠에도 접근할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $refs

    템플릿 참조를 통해 등록된, DOM 엘리먼트 및 컴포넌트 인스턴스 객체이다.

    타입

      interface ComponentPublicInstance {
        $refs: { [name: string]: Element | ComponentPublicInstance | null }
      }

    $refs 속성은 컴포넌트 내의 자식 요소에 대한 참조를 제공한다.
    이 속성을 사용하면 Vue 인스턴스에서 자식 컴포넌트나 DOM 요소에 직접적으로 접근할 수 있다.
    $refs 속성은 컴포넌트가 렌더링된 후에 설정되며, 자식 요소의 접근이 필요한 경우에 유용하게 사용된다.

    아래는 $refs를 사용하여 자식 요소에 접근하는 간단한 예시이다.

      <template>
        <div>
          <child-component ref="childRef"></child-component>
          <button @click="focusChild">Focus Child</button>
        </div>
      </template>

      <script>
      import ChildComponent from './ChildComponent.vue';

      export default {
        components: {
          ChildComponent
        },
        methods: {
          focusChild() {
            // $refs를 사용하여 자식 컴포넌트의 메서드나 속성에 접근
            this.$refs.childRef.focus();
          }
        }
      };
      </script>

    위의 예시에서 child-component 컴포넌트에 ref="childRef"를 설정하여 $refs 속성을 토해 이 컴포넌트에 접근할 수 있다.
    그리고 focusChild 메서드에서 $refs.childRef를 사용하여 자식 컴포넌트의 focus() 메서드를 호출하여 자식 요소에 포커스를 설정하고 있다.

    주의할 점은 $refs 속성은 Vue 인스턴스의 렌더링 후에만 설정되므로, 렌더링 전에 접근하려고 하면 undefined가 반환될 것이다.
    또한 $refs를 사용하여 자식 요소에 직접적으로 접근하는 것은 일반적으로 자식 컴포넌트와의 강한 경합을 초래할 수 있으므로, 상황에 따라 신중하게 사용해야 한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $attrs

    컴포넌트의 폴스로 속성이 포함된 객체이다.

    타입

      interface ComponentPublicInstance {
        $attrs: object
      }

    세부 사항

      폴스루 속성은 부모 컴포넌트에서 전달한 속성 및 이벤트 핸들러이지만, 자식의 prop 또는 내보낼(emit) 이벤트로 선언하지 않는다.

      기본적으로 $attrs의 모든 항목은 싱글 루트 엘리먼트만 있는 경우, 컴포넌트의 루트 엘리먼트로 자동 상속된다.
      inheritAttrs 옵션을 사용하여 명시적으로 비활성화할 수 있으며, 컴포넌트에 여러 루트 노드가 있는 경우에도 비활성화 된다.

    $attrs 속성은 부모 컴포넌트에서 자식 컴포넌트로 전달된 모든 HTML 속성(attribute)을 포함하는 객체이다.
    이 속성은 부모 컴포넌트에서 자식 컴포넌트로 HTML 속성을 전달할 때 유용하게 사용된다.

    부모 컴포넌트에서 자식 컴포넌트로 전달된 HTML 속성은 자식 컴포넌트 템플릿 내에서 직접적으로 사용되지 않는 경우가 많다.
    이런 경우에 $attrs를 사용하여 자식 컴포넌트의 루트 요소에 전달된 HTML 속성들을 동적으로 바인딩하거나 다른 속성에 전달할 수 있다.

    아래는 $attrs를 사용하여 자식 컴포넌트의 루트 요소에 동적으로 HTML 속성을 바인딩하는 간단한 예시이다.

      <template>
        <div v-bind="$attrs">
          //자식 컴포넌트의 나머지 내용
          <slot></slot>
        </div>
      </template>

      <script>
      export default {
        inheritAttrs: false, // $attrs 사용을 활성화
      };
      </script>

    위의 예시에서 inheritAttrs: false를 사용하여 자식 컴포넌트에서 직접적으로 사용되지 않는 HTML 속성을 루트 요소에 동적으로 바인딩하고 있다.
    이렇게 함으로써 부모 컴포넌트에서 자식 컴포넌트로 전달된 HTML 속성들을 효율적으로 관리하고 활용할 수 있다.

    $attrs 속성은 Vue.js에서 제공하는 특별한 속성 중 하나로, 컴포넌트 간의 데이터 전달과 상호작용을 보다 편리하게 해주는 기능이다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $watch()

    감시자를 생성하기 위한 명령형 API

    타입

      interface ComponentPublicInstance {
        $watch(
          source: string | (() => any),
          callback: WatchCallback,
          options?: WatchOptions
        ): StopHandle
      }

      type WatchCallback<T> = (
        value: T,
        oldValue: T,
        onCleanup: (cleanupFn: () => void) => void
      ) => void

      interface WatchOptions {
        immediate?: boolean // 기본 값: false
        deep?: boolean // 기본 값: false
        flush?: 'pre' | 'post' | 'sync' // 기본 값: 'pre'
        onTrack?: (event: DebuggerEvent) => void
        onTrigger?: (event: DebuggerEvent) => void
      }

      type StopHandle = () => void

    세부 사항

      첫 번째 인자는 감시할 소스이다. 컴포넌트 속성명 문자열, 점으로 구분된 간단한 경로 문자열 또는 getter 함수일 수 있다.
      두 번째 인자는 콜백 함수이다. 콜백은 감시된 소스의 새 값과 이전 값을 인자로 받는다.

      - immediate: 감시자가 생성되는 즉시 콜백이 호출된다. 최초 호출 시, 이전 값은 undefined이다.
      - deep: 소스가 객체인 경우, 깊은 변경사항에서도 콜백이 실행되도록 한다.
      - flush: 콜백의 발생(flush) 타이밍을 조정한다.
      - onTrack/onTrigger: 감시자의 의존성을 디버그한다.

    예제

      속성명으로 감시

        this.$watch('a', (newVal, oldVal) => {})

      점 구분자로 경로를 감시

        this.$watch('a.b', (newVal, oldVal) => {})

      복합적인 추적이 필요할 경우 getter 사용

        this.$watch(
          // `this.a + this.b` 표현식이 다른 결과를
          // 생성할 때마다 핸들러가 호출됩니다.
          // 계산된 속성을 정의하지 않고
          // 계산된 속성을 감시하고 있는 것과 같습니다.
          () => this.a + this.b,
          (newVal, oldVal) => {}
        )

      감시자 중지(해제)

        const unwatch = this.$watch('a', cb)

        // 더 이상 'a'를 감시하지 않음
        unwatch()

    $watch() 메서드는 데이터나 프로퍼티의 변경을 감시할 때 사용된다. Vue 컴포넌트 인스턴스에 대한 데이터 변화를 감지하고 특정 동작을 수행하거나 필요한 로직을 실행할 수 있다.

    이 메서드는 두가지 형태로 사용될 수 있다.

    1. 단일 데이터나 프로퍼티 감시

      watch: {
        // 감시할 데이터나 프로퍼티명: 변화가 감지될 때 실행할 콜백 함수
        myData: function(newValue, oldValue) {
          // 변화가 감지될 때 실행할 로직
        }
      }

    2. 여러 데이터나 프로퍼티를 감시

      watch: {
        // 여러 데이터나 프로퍼티를 감시하는 객체
        // 각 키는 감시할 데이터나 프로퍼티명이 되고 값은 변화가 감지될 때 실행할 콜백 함수
        myData1: function(newValue, oldValue) {
          // 변화가 감지될 때 실행할 로직
        },
        myData2: function(newValue, oldValue) {
          // 변화가 감지될 때 실행할 로직
        },
        // 추가적인 감시 대상들...
      }

    $watch() 메서드는 주로 컴포넌트 인스턴스의 데이터나 계산된 속성의 변경을 감지하고 이에 따른 동작을 수행하기 위해 사용된다.
    이를 통해 데이터나 상태의 변화에 따라 화면을 업데이트하거나 필요한 작업을 수행할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $emit()

    현재 인스턴스에서 커스텀 이벤트를 트리거한다. 추가적인 인자는 리스너의 콜백 함수로 전달된다.

    타입

      interface ComponentPublicInstance {
        $emit(event: string, ...args: any[]): void
      }

    예제

      export default {
        created() {
          // 커스텀 이벤트 'foo'만 트리거
          this.$emit('foo')
          // 추가 인자와 함께 트리거
          this.$emit('bar', 1, 2, 3)
        }
      }

    $emit() 메서드는 부모 컴포넌트로 이벤트를 전달하기 위해 사용된다. 이를 통해 자식 컴포넌트에서 발생한 이벤트를 부모 컴포넌트에 알리고,
    부모 컴포넌트에서 해당 이벤트를 수신하여 필요한 동작을 수행할 수 있다.

    주로 자식 컴포넌트에서 사용자의 상호작용에 응답하여 이벤트를 발생시키고, 부모 컴포넌트에서 해당 이벤트를 수신하여 상태를 업데이트하거나 다른 동작을 수행하는 데에 활용된다.

    예를 들어, 자식 컴포넌트에서 버튼 클릭 이벤트가 발생했을 때 해당 이벤트를 부모 컴포넌트에 알리고 싶을 때 $emit() 을 사용할 수 있다.
    자식 컴포넌트에서는 다음과 같이 $emit()을 호출하여 이벤트를 발생시킨다.

      this.$emit('eventName', eventData);

    여기서 evevtName은 발생시킬 이벤트의 이름을 나타내며 eventData는 이벤트와 함께 전달할 데이터를 나타낸다.

    부모 컴포넌트에서는 자식 컴포넌트를 사용할 때 이벤트를 수신하고, 해당 이벤트가 발생했을 대 실행할 메서드를 지정할 수 있다.

      <ChildComponent @eventName="handleEvent"></ChildComponent>

    이렇게 하면 자식 컴포넌트에서 eventName이라는 이벤트가 발생할 때 handleEvent 메서드가 호출된다.
    이 때, 필요에 따라 eventData를 매개변수로 받아와서 처리할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $forceUpdate()

    컴포넌트 인스턴스를 강제로 다시 렌더링한다.

    타입

      interface ComponentPublicInstance {
        $forceUpdate(): void
      }

    세부 사항

      Vue의 전반적인 자동 반응형 시스템을 고려할 때, 이것은 거의 필요하지 않다.
      필요할 수 있는 유일한 경우는 고급 반응형 API를 사용하여, 비반응형 상태를 명시적으로 생성하는 경우이다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  $nextTick()

    전역 nextTick()이 인스턴스에 반인딩된 버전

    타입

      interface ComponentPublicInstance {
        $nextTick(callback?: (this: ComponentPublicInstance) => void): Promise<void>
      }

    세부 사항

      전역 nextTick()과의 유일한 차이점은 this.$nextTick()에 전달된 콜백이 현재 컴포넌트 인스턴스에 바인딩된 this 컨텍스트를 갖는다는 것이다.

    $nextTick() 메서드는 Vue 인스턴스에서 다음 DOM 업데이트 사이클 이후에 코드를 실행하기 위해 사용된다.
    이 메서드는 비동기적으로 작동하여 현재의 DOM 업데이트 사이클이 완료된 후에 지정된 콜백 함수를 실행한다.

    주로 DOM을 업데이트한 후에 발생하는 비동기 작업이나 다음 DOM 렌더링 사이클에 필요한 작업을 처리할 때 유용하다.

    예를 들어, Vue 컴포넌트의 데이터를 업데이트하고 해당 변경 사항이 DOM에 반영되었는지 확인하고 싶을 때 $nextTick()을 사용할 수 있다.

      this.message = 'Updated' // 데이터 변경

      this.$nextTick(() => {
        // DOM 업데이트 후에 실행되는 작업
        console.log('DOM 업데이트 후에 실행됩니다.')
      })

    위의 예제에서 this.message의 값을 업데이트 한 후에 $nextTick()을 사용하여 DOM 업데이트가 완료된 후에 콜백 함수를 실행한다.
    이를 통해 Vue는 데이터 변경 사항을 처리하고 DOM 업데이트를 수행한 다음에 콜백 함수를 호출한다.
    이겋게 함으로써 최신의 DOM 상태를 보장하고 콜백함수가 올바르게 실행될 수 있다.
    







-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
