<template>
	<div></div>
</template>
<!--  

  렌더 함수 APIs

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  h()

    가상 DOM 노드(vnode)를 생성한다.

    타입

      // 전체 시그니쳐
      function h(
        type: string | Component,
        props?: object | null,
        children?: Children | Slot | Slots
      ): VNode

      // omitting props
      function h(type: string | Component, children?: Children | Slot): VNode

      type Children = string | number | boolean | VNode | null | Children[]

      type Slot = () => Children

      type Slots = { [name: string]: Slot }

    세부 사항

      첫 번째 인자는 문자열(네이티브 엘리먼트의 경우) 또는 Vue 컴포넌트 정의일 수 있다.
      두 번째 인자는 전달할 prop이고, 세 번째 인자는 자식이다.

      컴포넌트 vnode를 생성할 때, 자식은 슬롯 함수로 전달되어야 한다. 컴포넌트가 기본 슬롯만 기대하는 경우 단일 슬롯 함수를 전달할 수 있다.
      그렇지 않으면 슬롯을 슬롯 함수의 객체로 전달해야 한다.

      편의상 자식이 슬롯 객체가 아닌 경우 props 인자를 생략할 수 있다.

    예제

      네이티브 엘리먼트 만들기

        import { h } from 'vue'

        // 유형을 제외한 모든 인자는 선택 사항이다.
        h('div')
        h('div', { id: 'foo' })

        //속성과 프로퍼티를 모두 prop에 사용할 수 있다. Vue가 자동으로 올바른 할당 방법을 선택한다.
        h('div', { class: 'bar', innerHTML: 'hello' })

        //클래스와 스타일은 템플릿에서와 같이 동일한 객체/배열 값을 지원한다.
        h('div', { class: [foo, { bar }], style: { color: 'ref' } })

        // 이벤트 리스너는 onXxx로 전달되어야 한다.
        h('div', { onClick: () => {} })

        // 자식은 무자열일 수 있다.
        h('div', { id: 'foo' }, 'hello')

        //prop이 없는 경우 prop 생략 가능
        h('div', 'hello')
        h('div', [h('span', 'hello')])

        // 자식 배열은 혼합 노드와 문자열을 포함할 수 있다.
        h('div', ['hello', h('span', 'hello')])

      컴포넌트 만들기

        import Foo from './Foo.vue'

        h(Foo, {
          someProp: 'hello',
          onUpdate: () => {}
        })

        h(Foo, () => 'default slot')

        h(MyComponent, null, {
          default: () => 'default slot',
          foo: () => h('div', 'foo'),
          bar: () => [h('span', 'one'), h('span', 'two')]
        })

    h() 함수는 Vue.js에서 사용되는 하이퍼스크립트(Hyperscript) 함수이다. 이 함수는 Virtual DOM(가상 DOM)을 생성하는 데 사용된다.
    Virtual DOM은 실제 DOM의 가벼운 복사본으로, 렌더링 성능을 향상시키고 애플리케이션의 상태 변화를 효율적으로 처리하는 데 도움이 된다.

    예기서 h() 함수의 각 매개변수는 다음과 같은 역할을 한다.

    type: 렌더링할 요소의 유형을 나타낸다. 이것은 문자열 또는 Vue 컴포넌트이다. 문자열인 경우 HTML 요소를 나타내며, Vue 컴포넌트인 경우 해당 컴포넌트를 렌더링한다.
    props: 렌더링된 요소에 적용될 속성을 포함하는 객체이다. 이 객체에는 요소의 속성 및 이벤트 핸들러가 포함될 수 있다.
    children: 렌더링된 요소의 자식 요소를 나타내는 매개변수이다. 이것은 문자열, 배열 또는 Vue 슬롯과 같은 요소가 될 수 있다. 이를 통해 요소 내부에 포함될 텍스트, 다른 요소 또는 컴포넌트를 지정할 수 있다.

    h() 함수는 이러한 매개변수를 사용하여 가상 DOM의 트리를 구성하고, 이를 통해 Vue 컴포넌트의 렌더링을 관리한다.
    예를 들어, 다음과 같은 코드는 <div>요소와 그 안에 있는 텍스트 노드를 생성한다.

      import { h } from 'vue';

      const vnode = h('div', null, 'Hello, Vue!');

    이렇게 생성된 vnode는 Vue의 가상 DOM에서 사용되며, Vue 컴포넌트의 렌더링 및 상태 변화를 관리하는 데 사용된다.

    h() 함수는 일반적으로 직접적으로 사용되는 경우보다는 Vue 컴파일러나 JSX와 같은 도구에 의해 내부적으로 사용된다.
    그러나 몇 가지 특별한 상황에서 개발자가 직접적으로 h() 함수를 사용할 수 있다.

    1. 동적 컴포넌트 생성
      런타임에 동적으로 컴포넌트를 생성해야 할 때, h()함수를 사용할 수 있다. 예를 들어, 조건에 따라 다른 컴포넌트를 렌더링해야 하는 경우나 API응답에 따라 동적으로 컴포넌트를 선택해야하는 경우에 유용하다.

    2. 렌더링 함수
      Vue.js에서 렌더링 함수를 사용하여 템플릿을 대체할 수 있다. h() 함수는 렌더링 함수의 핵심이다. 특히 함수형 컴포넌트를 작성하거나 렌더링 로직이 복잡한 경우에 사용된다.

    3. 커스텀 렌더링
      일부 경우에는 JSX나 템플릿을 작성하는 것보다 h() 함수를 사용하여 더 간단하고 직관적인 코드를 작성할 수 있다. 특히 간단한 UI 요소를 동적으로 생성할 때 유용하다.

    4. 인라인 템플릿
      일부 경우에는 JSX나 템플릿을 작성하는 것보다 h() 함수를 사용하여 더 간단하고 직관적인 코드를 작성할 수 있다. 특히 간단한 UI 요소를 동적으로 생성할 때 유용하다.

    일반적으로 대부분의 Vue 애플리케이션에서는 템플릿 또는 JSX를 사용하여 UI를 작성하고, 컴파일러나 뷰 라이브러리가 h() 함수를 내부적으로 처리하도록 하는 것이 좋다.
    그러나 위에서 언급한 상황과 같이 특별한 경우에 직접적으로 h() 함수를 사용할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  mergeProps()

    특정 porp에 대한 특수 처리를 사용하여 여러 prop 개체를 병합한다.

    타입

      function mergeProps(...args: object[]): object

    세부 사항

      mergeProps()는 다음 프로퍼티에 대한 특수 처리를 통해 여러 프로퍼티 객체를 병합하는 것을 지원한다.

      - class
      - style
      - onXxx 이벤트 리스너 - 같은 이름을 가진 여러 리스너가 배열로 병합된다.

      병합 동작이 필요하지 않고 간단한 덮어쓰기를 원하는 경우 네이티브 객체 스프레드를 대신 사용할 수 있다.

        import { mergeProps } from 'vue'

        const one = {
          class: 'foo',
          onClick: handlerA
        }

        const two = {
          class: { bar: true },
          onClick: handlerB
        }

        const merged = mergeProps(one, two)
        /**
        {
          class: 'foo bar',
          onClick: [handlerA, handlerB]
        }
        */

    mergeProps() 함수는 두 개의 프로퍼티 객체를 병합하여 하나의 프로퍼티 객체로 반환하는 Vue 3의 유틸리티 함수이다.
    이 함수는 주로 렌더링 함수나 커스텀 렌더링 로직에서 사용된다.

    일반적으로 Vue 컴포넌트에서는 부모 컴포넌트에서 전달된 프로퍼티와 컴포넌트 자체에서 정의한 기본값을 병합하여 최종적으로 사용할 프로퍼티 객체를 생성해야 한다.
    mergeProps() 함수는 이러한 프로퍼티 병합 작업을 수행하기 위해 사용된다.

    주요 특징

      1. 프로퍼티 병합
        mergeProps() 함수는 두 개의 프로퍼티 객체를 병합하여 하나의 프로퍼티 객체로 변환한다.
        첫 번째 프로퍼티 객체는 일반적으로 부모 컴포넌트에서 전달된 프로퍼티를 나타내며,
        두 번째 프로퍼티 객체는 컴포넌트 자체에서 정의한 기본값을 나타낸다.

      2. 우선순위
        첫 번째 프로퍼티 객체에 동일한 키가 있는 경우, 두번째 프로퍼티 객체의 값이 우선된다.
        즉, 부모 컴포넌트에서 전달된 값이 기본값을 덮어쓴다.

      3. 옵셔널 프로퍼티
        두 번째 프로퍼티 객체에만 존재하는 옵셔널한 프로퍼티는 병합 과정에서 무시된다.
        이는 컴포넌트 자체에서만 사용되는 기본값이며, 부모 컴포넌트에서 전달되지 않는 경우 무시된다.

        * 옵셔널 프로퍼티란
          컴포넌트나 함수 등에서 사용되는 매개변수 또는 속성 중 일부가 필수가 아니라 선택적인 경우를 말한다.
          다시 말해, 사용자가 해당 프로퍼티를 제공하지 않아도 되는 것이다.

    사용 예시

      import { mergeProps } from 'vue'

      export default {
        props: {
          // 부모 컴포넌트에서 전달된 프로퍼티
          msg: String
        },
        setup(props) {
          // 기본값 정의
          const defaultProps = {
            // 기본값
            count: 0
          }

          // 병합된 프로퍼티 객체 생성
          const mergedProps = mergeProps(props, defaultProps)

          return {
            mergedProps
          }
        }
      }

    위의 예시에서 mergeProps() 함수는 부모 컴포넌트에서 전달된 props와 컴포넌트 자체에서 정의한 defaultProps를 병합하여 mergedProps로 반환한다.
    최종적으로 mergedProps 객체에는 부모 컴포넌트에서 전달된 값이 우선되고, 컴포넌트 자체에서 정의한 기본값이 포함된다.

    mergeProps() 함수가 사용되는 상황

    1. 컴포넌트의 기본값 설정
      컴포넌트가 기본값을 가지고 있고, 부모 컴포넌트에서 전달된 값이 없는 경우 사용된다.
      이 경우 mergeProps() 함수를 사용하여 부모 컴포넌트에서 전달된 값과 기본값을 병합하여 최종적인 프로퍼티를 생성한다.

    2. 옵셔널한 프로퍼티 처리
      컴포넌트의 일부 프로퍼티가 옵셔널하고, 해당 프로퍼티가 부모 컴포넌트에서 전달되지 않은 경우 기본값을 사용해야 할 때에 사용된다.
      mergeProps() 함수를 사용하여 부모 컴포넌트에서 전달된 값과 기본값을 병합하여 최종적인 프로퍼티를 생성하면서, 옵셔널한 프로퍼티를 처리할 수 있다.

    3. 프로퍼티의 우선순위 정의
      경우에 따라 컴포넌트에서 정의한 기본값이 부모 컴포넌트에서 전달된 값보다 우선되어야 할 수 있다.
      이러한 경우 mergeProps() 함수를 사용하여 두 개의 프로퍼티 객체를 병합하여 최종적인 프로퍼티를 생성하면서, 기본값이 우선되도록 설정할 수 있다.

    4. 컴포넌트의 인터페이스 정의
      컴포넌트의 인터페이스를 명확하게 정의하고, 부모 컴포넌트와 컴포넌트 자체에서 사요되는 프로퍼티를 명시적으로 병합할 때 사용된다.
      이는 컴포넌트의 동작을 이해하고 유지보수하기 쉽도록 도와준다.

    따라서 mergeProps() 함수는 컴포넌트의 프로퍼티 관리와 유연성을 향상시키는 데 유용하며, 특히 기본값 처리 및 프로퍼티 우선순위 정의에 있어서 실무적으로 매우 도움이 된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  cloneVNode()

    vnode를 복제한다.

    타입

      function cloneVNode(vnode: VNode, extraProps?: object): VNode

    세부 사항

      원본가 병합할 추가 프로퍼티와 함께 복제된 vnode를 반환한다.

      vnode는 한 번 생성되면 변경할 수 없는 것으로 간주해야 하며, 기존 vnode의 프로퍼티를 변경해서는 안된다. 대신 다른/추가 프로퍼티로 복제해면 된다.

      vnode에는 특별한 내부 속성이 있으므로 복제하는 것은 객체 스프레드만큼 간단하지 않다.
      cloneVNode()는 대부분 내부 로직을 처리한다.

    예제

      import { h, cloneVNode } from 'vue'

      const original = h('div')
      const cloned = cloneVNode(original, { id: 'foo' })

    cloneVNode() 함수는 주어진 VNode를 복제하는 데 사용된다. 이 함수를 사용하면 원본 VNode를 변경하지 않고 동일한 구조와 속성을 가진 새로운 VNode를 생성할 수 있다.

    일반적으로 Vue.js에서 가상 DOM을 조작할 때 사용된다. 예를 들어, 동적으로 컴포넌트를 렌더링하거나 조건부로 특정 요소를 추가하거나 제거할 때 유용하다.
    cloneVNode() 함수를 사용하면 기존 VNode의 구조를 그대로 유지하면서 새로운 VNode를 생성할 수 있다.

    cloneVNode() 함수는 다음과 같은 구문을 가진다.

      cloneVNode(vnode, props, children)

    vnode: 복제할 대상이 되는 원본 VNode이다.
    props(선택적): 새 VNode에 적요할 속성이다. 이를 통해 기존 VNode의 속성을 덮어쓸 수 있다.
    children(선택적): 새 VNode에 추가할 자식 노드이다. 이를 통해 기존 VNode의 자식 노드를 변경하거나 새로운 자식 노드를 추가할 수 있다.

    예를 들어, 다음은 cloneVNode() 함수를 사용하여 기존 VNode를 복제하는 간단한 예제이다.

      import { h, cloneVNode } from 'vue'

      const originalVNode = h('div', { class: 'original' }, 'Original Content')

      const clonedVNode = cloneVNode(originalVNode, { class: 'cloned' })

      console.log(clonedVNode)

    이 코드는 원본 VNode에 새로운 클래스 속성을 추가하여 새로운 VNode를 생성한다.
    결과적으로 cloneVNode는 div 요소이며 클래스가 cloned로 지정되어 있다.

    cloneVNode() 함수가 사용되는 상황

    1. 동적으로 컴포넌트 생성 및 관리
      사용자 상호 작용 또는 조건에 따라 동적으로 컴포넌트를 생성하고 관리해야 하는 경우가 있다.
      이 때, cloneVNode() 함수를 사용하여 원본 VNode를 복제하여 새로운 컴포넌트 인스턴스를 만들 수 있다.

    2. 상태 변경에 따른 UI 업데이트
      상태 변경에 따라 UI를 업데이트해야 하는 경우가 있다.
      이 때 cloneVNode() 함수를 사용하여 이전 VNode를 복제하고 새로운 상태를 적용하여 UI를 업데이트할 수 있다.

    3. 조건부 렌더링
      조건부로 특정 요소를 렌더링하거나 숨기는 경우가 있다.
      이 때 cloneVNode() 함수를 사용하여 조건에 따른 VNode를 생성하여 조건부 렌더링을 구현할 수 있다.

    4. 동적으로 자식 요소 추가 또는 제거
      동적으로 자식 요소를 추가하거나 제거해야 하는 경우가 있다.
      이 때, cloneVNode() 함수를 사용하여 기존 VNode를 복제하고 자식 요소를 변경하여 동적으로 자식 요소를 관리할 수 있다.

    5. UI 상태 관리
      UI 상태를 관리하고 상태에 따라 다양한 UI를 렌더링해야 하는 경우가 있다.
      이때, cloneVNode() 함수를 사용하여 다양한 UI 상태를 처리하고 관리할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  isVNode()

    값이 v노드인지 확인한다.

    타입

      function isVNode(value: unknown): boolean

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  resolveComponent()

    등록된 컴포넌트를 이름으로 수동으로 확인한다.

    타입

      function resolveComponent(name: string): Component | string

    세부 사항

      참고: 컴포넌트를 직접 임포트할 수 있는 경우에는 이 작업이 필요없다.

      올바른 컴포넌트 컨텍스트에서 확인하려면 렌더 함수 내부에서 resolveComponent()를 호출해야 한다.

      컴포넌트를 찾을 수 없는 경우 런타임 경고가 발생하고 이름 문자열이 반환된다.

    예제

      import { h, resolveCompenont } from 'vue'

      export default {
        render() {
          const ButtonCounter = resolveComponent('ButtonCounter')
          return h(ButtonCounter)
        }
      }

    resolveComponent() 함수는 문자열이나 컴포넌트 옵션을 가져와서 해당하는 컴포넌트 인스턴스를 반환하는 Vue3의 유틸리티 함수이다.

    일반적으로 다음과 같은 상황에서 사용된다.

    1. 동적 컴포넌트 로딩
      컴포넌트를 동적으로 로딩할 때, resolveComponent() 함수를 사용하여 문자열 또는 컴포넌트 옵션을 컴포넌트 인스턴스로 변환한다.
      이를 통해 Vue 컴포넌트를 런타임에 로딩하고 사용할 수 있다.

    2. 템플릿 내부에서 컴포넌트 참조
      템플릿 내부에서 컴포넌트를 참조할 때, resolveComponent() 함수를 사용하여 문자열 또는 컴포넌트 옵션을 컴포넌트 인스턴스로 해결한다.

      예를 들어, 다음 resolveComponent() 함수를 사용하여 컴포넌트를 로딩하는 방법이다.

        import { resolveComponent } from 'vue'

        // 문자열을 사용하여 컴포넌트 로딩
        const MyComponent = resolveComponent('MyComponent')

        // MyComponent를 사용하여 컴포넌트 인스턴스 생성
        const componentInstance = h(MyComponent)

        // 또는 템플릿 내에서 사용할 수 있습니다.
        <template>
          <MyComponent />
        </template>

      또한, 컴포넌트 옵션을 직접 전달하여 컴포넌트 인스턴스를 해결할 수 있다.

        import { resolveComponent } from 'vue'

        const MyComponent = resolveComponent({
          template: '<div>My Component</div>'
        })

      이와 같이 resolveComponent() 함수를 사용하면 컴포넌트를 동적으로 로딩하고 템플릿 내에서 참조할 수 있다. 이는 특히 라우팅, 조건부 렌더링 및 동적 UI 생성과 같은 상황에서 유용하다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  resolveDirective()

    등록된 디렉티브를 이름으로 수동으로 확인한다.

    타입

      function resolveDirective(name: string): Directive | undefined

    세부 사항

      참고: 직접 디렉티브를 가져올 수 있는 경우 이 작업은 필요없다.

      올바른 컴포넌트 컨텍스트에서 해결하려면 resolveDirective()를 setup() 또는 렌더링 함수 내부에서 호출해야 한다.

      디렉티브를 찾을 수 없으면 런타임 경고가 발생하고 함수는 undefined를 반환한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  withDirectives()

    노드에 사용자 지정 지시문을 추가한다.

    타입

      function withDirectives(
        vnode: VNode,
        directives: DirectiveArguments
      ): VNode

      // [Directive, value, argument, modifiers]
      type DirectiveArguments = Array<
        | [Directive]
        | [Directive, any]
        | [Directive, any, string]
        | [Directive, any, string, DirectiveModifiers]
      >

    세부 사항

      기존 vnode를 사용자 정의 디렉티브로 래핑한다. 두 번째 인자는 사용자 정의 디렉티브의 배열이다.
      각 사용자 정의 지시어는 [directive, value, argument, modifiers] 형식의 배열로 표현된다.
      배열의 꼬리 엘리먼트는 필요하지 않은 경우 생략할 수 있다.

    예제

      import { h, withDirectives } from 'vue'

      // a custom directive
      const pin = {
        mounted() {
          /* ... */
        },
        updated() {
          /* ... */
        }
      }

      // <div v-pin:top.animate="200"></div>
      const vnode = withDirectives(h('div'), [
        [pin, 200, 'top', { animate: true }]
      ])

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  withModifiers()

    이벤트 핸들러 함수에 내장된 v-on 수정자를 추가하려면 다음과 같이 해야한다.
    Vue 3에서는 이벤트 핸들러 함수에 직접 수정자를 추가하는 것이 더 이상 지원되지 않기 때문에 withModifiers() 함수를 사용하여 이를 대처해야한다.

    타입

      function withModifiers(fn: Function, modifiers: string[]): Function

      fn: 수정자가 적용될 이벤트 핸들러 함수이다.
      modifiers: 추가할 v-on 수정자의 배열이다.

      반환값: 수정자가 추가된 이벤트 핸들러 함수를 반환한다.

    예제

      import { h, withModifiers } from 'vue'

      const vnode = h('button', {
        //v-on.stop.prevent에 해당하는 수정자가 추가된 onClick 이벤트 핸들러 함수
        onClick: withModifiers(() => {
          // 이벤트 핸들러 함수의 내용
        }, ['stop', 'prevent'])
      })

    위의 예제에서는 onClick 이벤트 핸들러 함수에 stop과 prevent 수정자가 추가되었다.
    이렇게 하면 해당 이벤트의 기본 동작이 중지되고 이벤트 버블링도 막힌다.
    withModifiers() 함수를 사용하면 이벤트 핸들러 함수에 다양한 수정자를 간편하게 추가할 수 있다.

    이와 같이 withModifiers() 함수는 특정 이벤트에 대한 핸들러 함수를 만들 때 편리하게 수정자를 추가할 수 있도록 도와준다.


-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
