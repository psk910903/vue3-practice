<template>
	<div></div>
</template>
<!--  

  반응형 고급

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  shallowRef()

    ref()의 얕은 버전이다.

    타입

      function shallowRef<T>(value: T): ShallowRef<T>

      interface ShallowRef<T> {
        value: T
      }

    세부 사항 

      ref()와 달리 shallowRef()의 내부 값은 있는 그대로 저장되고 노출되며 내부 깊숙이까지 반응형으로 동작하지는 않는다.
      .value 접근만 반응형이다.

      shallowRef는 일반적으로 대규모 데이터 구조의 성능 최적화 또는 외부 상태 관리 시스템과의 통합에 사용된다.

    예제

      const state = shallowRef({ count: 1 })

      // change(변경)을 트리거하지 않음
      state.value.count = 2

      // change를 트리거 함
      state.value = { count: 2 }

    
    shallowRef() 함수는 ref() 함수의 얕은 버전이다.
    이 함수는 주어진 값(value)을 반응형으로 만들어 반환한다. 하지만 ref()와 달리 shallowRef()는 객체의 속성까지는 반응형으로 만들지 않고,
    단순히 주어진 객체를 감싸서 반환한다. 따라서 shallowRef()로 생성된 반응형 데이터는 .value 접근만 반응형으로 동작한다.

    예를 들어, 다음과 같이 shallowRef() 함수를 사용하여 객체를 만들 수 있다.

      import { shallowRef } from 'vue';

      const state = shallowRef({ count: 1 });

      // 객체의 속성을 변경하더라도 변경 사항은 반영되지 않음
      state.value.count = 2;

      // 객체를 새로운 값으로 변경하면 변경 사항이 반영됨
      state.value = { count: 2 };

    위의 예제에서 state는 shallowRef() 함수를 사용하여 반응형으로 만들어진 객체다.
    객체의 속성을 변경하면 해당 변경사항은 반영되지 않지만, 객체 자체를 새로운 값으로 변경하면 변경 사항이 반영된다.
    이는 shallowRef() 함수가 객체의 속성을 반응형으로 만들지 않고, 단순히 주어진 객체를 반응형으로 감싸기 때문이다.

    shallowRef()는 객체의 내부 속성까지는 감시하지 않는다. 따라서 객체의 내부 속성이 변경되어도 Vue가 이를 감시하지 않는다.
    그러나 객체 자체가 변경되는 경우(객체의 주소값이 다른 값으로 바뀌는 경우)에는 변경사항을 감지하여 반응한다.

      import { shallowRef } from 'vue';

      const obj = { name: 'John' };
      const shallowObjRef = shallowRef(obj);

      console.log(shallowObjRef.value); // { name: 'John' }

      // 객체의 속성을 변경해도 반영되지 않음
      obj.name = 'Jane';

      console.log(shallowObjRef.value); // { name: 'John' }

      // 객체 자체를 변경하면 변경사항이 반영됨
      const newObj = { name: 'Jane' };
      shallowObjRef.value = newObj;

      console.log(shallowObjRef.value); // { name: 'Jane' }

    위의 예제에서 obj 객체의 name 속성을 변경하더라도 shallowObjRef 의 값은 변하지 않는다.
    그러나 obj 객체를 새로운 newObj로 변경하면 shallowObjRef의 값이 반영되어 name 속성이 변경된다.
    이는 shallowRef() 함수가 객체의 내부 속성은 감시하지 않고, 객체 자체의 변경사항만을 반영하기 때문이다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  triggerRef()

    shallowRef()의 강제 트리거 이펙트, 이것은 일반적으로 shallowRef 내부 깊숙한 곳의 값을 변경 후, 관련 이펙트를 강제로 트리거 하기위해 사용한다.

    타입

      function triggerRef(ref: ShallowRef): void

    예제

      const shallow = shallowRef({
        greet: '안녕, Vue!'
      })

      // 첫 실행 시 로그: "안녕, Vue!"
      watchEffect(() => {
        console.log(shallow.value.greet)
      })

      // ref가 얕기 때문에 이펙트가 트리거되지 않음
      shallow.value.greet = '멋진 Vue!'

      // 강제로 shallow와 관련된 이펙트 트리거
      triggerRef(shallow) // 로그: "멋진 Vue!"

    shallow를 watchEffect에 등록하였지만 shallow.value.greet = '멋진 Vue!'로 shallow 객체의 값을 변경할 수 있지만 반응형으로 동작하지 않음
    triggerRef()를 사용하여 강제로 이펙트 트리거를 할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  customRef()

    의존성 추적 및 업데이트 트리거를 명시적으로 제어하기 위한 커스텀 ref를 만든다.

    타입

      function customRef<T>(factory: CustomRefFactory<T>): Ref<T>

      type CustomRefFactory<T> = (
        track: () => void,
        trigger: () => void
      ) => {
        get: () => T
        set: (value: T) => void
      }

    세부 사항

      customRef()는 인자로 펙토리 함수를 받는다. 이 함수는 track과 trigger를 인자로 수신하며,
      get과 set 메서드가 포함된 객체를 반환해야 한다.

      일반적으로 track()은 get() 내부에서 호출되어야 하고, trigger()는 set() 내부에서 호출되어야 하지만, 호출 조건 및 타이밍은 완전히 커스텀 제어 가능하다.

    예제

      최신 set 호출 후, 특정 시간 초과 후에만 값을 업데이트하는 디바운스된 ref 생성

        import { customRef } from 'vue'

        export function useDebouncedRef(value, delay = 200) {
          let timeout
          return customRef((track, trigger) => {
            return {
              get() {
                track()
                return value
              },
              set(newValue) {
                clearTimeout(timeout)
                timeout = setTimeout(() => {
                  value = newValue
                  trigger()
                }, delay)
              }
            }
          })
        }

    컴포넌트에서 사용법

      <script setup>
      import { useDebouncedRef } from './debouncedRef'
      const text = useDebouncedRef('hello')
      </script>

      <template>
        <input v-model="text" />
      </template>

      * track() 함수란
        의존성 추적(dependency tracking)을 수행하기 위한 콜백 함수이다. Vue의 반응성 시스템은 데이터와 그 데이트를 사용하는 각각의 컴포넌트 사이의 의존성을 추적하여,
        데이터가 변경될 때마다 영향을 받는 컴포넌트를 다시 렌더링한다.

        track() 함수는 customRef() 팩토리 함수에서 사용되며, 주로 get() 메서드 내부에서 호출된다.
        이 함수를 호출함으로써 현재 ref에 대한 의존성을 추적할 수 있다. 
        이를 통해 Vue는 해당 ref가 변경될 때 영향을 받는 컴포넌트를 식별하고, 변경된 데이터에 맞게 해당 컴포넌트를 다시 레더링할 수 있다.

        track() 함수의 호출은 Vue의 내부 동작이며, 주로 컴포넌트의 상태를 추적하고 변경사항을 감지하기 위해 사용된다.
        사용자가 직접 track() 함수를 호출할 필요는 없다.
        대신에 customRef()를 사용하여 커스텀 ref를 만들 때 이 함수가 내부적으로 호출되어야 한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  shallowReactive()

    reactive()의 얕은 버전이다.

    타입

      function shallowReactive<T extends object>(target: T): T

    세부 사항

      reactive()와 달리 내부 깊숙한 곳의 변경이 반응형으로 작동하지 않고, 얕게 루트 수준의 속성 변경에 대해서만 반응형인 객체이다.
      속성 값은 있는 그대로 저장되고 노출되므로, 속성이 ref 값인 경우, 자동으로 언래핑되지 않는다.
      
      *주의
        얕은 데이터 구조는 텀포넌트에서 루트 수준 상태로만 사용해야 한다.
        내부 깊숙이까지 반응형으로 동작하는 객체 내부에 중첩하는 경우, 
        반응형 동작에 일관성이 없는 트리가 생성되어 이해와 디버그가 어려울 수 있으니, 중첩하여 사용하면 안된다.

    예제

      const state = shallowReactive({
        foo: 1,
        nested: {
          bar: 2
        }
      })

      // 상태의 자체 속성 변경은 반응형으로 동작함
      state.foo++

      // ...하지만 중첩된 객체는 그렇지 않음
      isReactive(state.nested) // false

      // 반응형이 아님
      state.nested.bar++

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  shallowReadonly()

    readonly()의 얕은 버전이다.

    타입

      function shallowReadonly<T extends object>(target: T): Readonly<T>

    세부 사항

      readonly()와 달리 내부 깊숙이까지 변환하지 않고, 루트 수준 속성만 읽기 전용으로 만들어진다.
      속성 값은 있는 그대로 저장되고 노출되므로, 속성이 ref 값인 경우, 자동으로 언래핑되지 않는다.

      *주의
        얕은 데이터 구조는 컴포넌트에서 루트 수준 상태로만 사용해야 한다. 내부 깊숙이까지 반응형으로 동작하는 객체 내부에 중첩하는 경우,
        반응형 동작에 일관성이 없는 트리가 생성되어 이해와 디버그가 어려울 수 있으니, 중첩하여 사용하면 안된다.

    예제

      const state = shallowReadonly({
        foo: 1,
        nested: {
          bar: 2
        }
      })

      // 상태의 자체 속성을 변경하는 것은 실패 됨
      state.foo++

      // ...하지만 중첩된 객체는 그렇지 않음
      isReadonly(state.nested) // false

      // 변경 작업이 됨
      state.nested.bar++

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  toRaw()

    Vue에서 만든 프록시의 원시 원본 객체를 반환한다.

    타입

      function toRaw<T>(proxy: T): T

    세부 사항

      toRaw()는 reactive(), readonly(), shallowReactive(), shallowReadonly()로 생성된 프록시에서 원본 객체를 반환한다.

      이것은 일시적으로 프록시의 접근/추적 오버헤드를 발생시키는 읽기/쓰기와 관련된 트리거 없이 사용하기 위한 용도이다.
      원본 객체의 영구 참조를 유지하는 것은 권장되지 않는다.
      주의해서 사용해야 한다.

    예제

      const foo = {}
      const reactiveFoo = reactive(foo)

      console.log(toRaw(reactiveFoo) === foo) // true

    toRaw() 함수는 반응성을 가진 객체를 원시 JavaScript 객체로 변환하는 데 사용된다.
    Vue 3에서는 반응성을 가진 객체를 내부적으로 프록시(proxy) 객체로 감싸서 관리한다.
    이렇게 감싸진 객체는 반응성을 제공하면서도 내부적으로 원본 객체를 가리키고 있다.

    toRaw() 함수를 사용하면 이러한 프록시 객체를 제거하고, 객체의 실제 내부를 그대로 반환한다.
    이는 일반적으로 디버깅이나 로깅 시에 유용하며, 객체의 내부 상태를 확인하거나 객체를 직렬화할 때 사용될 수 있다.
    
    예를 들어, 다음은 toRaw() 함수를 사용하여 반응성을 가진 객체를 원시 JavaScript 객체로 변환하는 예제이다.

      import { reactive, toRaw } from 'vue'

      const originalObject = { count: 0 }
      const reactiveObject = reactive(originalObject)

      // reactiveObject를 변경합니다.
      reactiveObject.count++

      // reactiveObject를 toRaw()를 사용하여 원시 객체로 변환합니다.
      const rawObject = toRaw(reactiveObject)

      console.log(rawObject) // { count: 1 }

    이 코드에서 toRaw() 함수를 사용하여 reactiveObject를 rawObject로 변환했다.
    변환된 rawObject는 프록시가 제거된 원시 JavaScript 객체이며, 내부적으로는 동일한 데이터를 가지고 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  markRaw()

    객체가 프록시로 변환되지 않도록 마크(mark)한다. 객체 자체를 반환한다.

    타입

      function markRaw<T extends object>(value: T): T

    예제

      const foo = markRaw({})
      console.log(isReactive(reactive(foo))) // false

      // 다른 반응형 객체 내부에 중첩될 때도 작동함
      const bar = reactive({ foo })
      console.log(isReactive(bar.foo)) // false

    *주의
      markRaw()나 shallowReactive() 같이 얕은 API를 사용하면, 선택적으로 기본적인 내부 깊숙이까지의 반응형/읽기전용 변환을 옵트아웃(opt-out)하여,
      상태 그래프(선언/정의/사용하는 곳)에 프록시 되지 않은 원시 객체를 포함할 수 있다. 이것들은 다양한 이유로 사용될 수 있다.

      - 일부 값들은 함부로 반응형으로 만들면 안되는데, 그 예로 복잡한 타사의 클래스 인스턴스나 Vue 컴포넌트 객체가 있다.

      - 프록시 변경을 건너뛰면, 변경해서는 안되는 데이터 소스를 포함하고 있는 커다란 리스트를 렌더링할 때, 성능이 향상될 수 있다.

      원시(raw) 옵트아웃은 루트 레벨에만 있기 때문에 고급으로 간주되므로, 마크되지 않은 중첩된 원시 객체를 반응형 객체로 설정한 다음 다시 접근을 시도하면,
      프록시 버전의 객체로 접근하게 된다. 이것은 동일한 객체에 참조했다고 예상했으나, 원시 버전과 프록시 버전을 혼용하여 사용하는 잘못된 ID 참조(identity hazards)로 이어질 수 있다.

        const foo = markRaw({
          nested: {}
        })

        const bar = reactive({
          //foo는 원시 마크가 있지만, foo.nested는 그렇지 않음
          nested: foo.nested
        })

        console.log(foo.nested == bar.nested) // false

      잘못된 ID 참조 문제는 일반적으로 드물다. 그러나 이 문제로부터 안전하게 이러한 API를 활용하려면, 반응형 시스템이 어떻게 동작하는지에 확실한 이해가 필요하다.

      * 옵트 아웃(opt-out)이란
        Vue의 컴파일 옵션을 설정하여 특정 기능을 사용하거나 비활성화하는 것을 의미한다.
        Vue 3는 소규모 런타임 및 컴파일러의 구성 요소로 분할되어 있으며, 이러한 모듈을 선택적으로 사용하거나 제외할 수 있도록 옵트 아웃 기능이 제공된다.

        옵트 아웃은 주로 번들 크기를 최소화하고 성능을 향상시키기 위해 사용된다. 
        특히 더 큰 규모의 프로젝트에서는 필요하지 않은 기능을 제외함으로써 번들의 크기를 줄이고 초기 로딩 시간을 단축할 수 있다.

        Vue 3에서 사용되는 주요 옵트 아웃 옵션은 다음과 같다.

          1. Prod Mode
            개발 모드와 프로덕션 모드를 구분하여 개발 중에만 사용되는 경고 메시지를 제외한다.
            이를 통해 프로덕션 빌드에서는 더 작은 번들 크기를 얻을 수 있다.

          2. Custom Blocks
            컴포넌트의 <template>외에도 다른 유형의 블록을 처리하는 컴파일러 옵션을 비활성화한다.
            예를 들어 <style> 또는 <script setup> 블록을 사용하지 않을 때 해당 옵션을 비활성화하여 번들 크기를 줄일 수 있다.

          3. Fragment Mode
            Vue의 Fragment 모드를 사용할지 여부를 결정한다. Fragment 모드는 템플릿에서 여러 루트 요소를 허용하며
            이를 위해 추가적인 래퍼 요소를 생성한다. 이 옵션을 비활성화하여 추가적인 래퍼 요소를 제거할 수 있다.

          이러한 옵트 아웃 옵션들은 Vue CLI나 다른 빌드 도구를 사용하여 프로젝트를 설정할 수 있다.
          필요한 기능만 포함하여 번들 크기를 최적화하고 효율적인 프로덕션 빌드를 생성하는 데 도움이 된다.


  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  effectScope()

    범위 내 반응형 이펙트(계산된 속성, 감시자)들을 캡처하고, 일괄적으로 처리하는 effectScope 객체를 반환한다.

    타입

      function effectScope(detached?: boolean): EffectScope

      interface EffectScope {
        run<T>(fn: () => T): T | undefined // undefined if scope is inactive
        stop(): void
      }

    예제

      const scope = effectScope()

      scope.run(() => {
        const doubled = computed(() => counter.value * 2)

        watch(doubled, () => console.log(doubled.value))

        watchEffect(() => console.log('Count: ', doubled.value))
      })

      // 범위 내 여러 이펙트 중지(폐기 됨)
      scope.stop()

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
  getCurrentScope()
  
    현재 활성 effectScope가 있는 경우, 이를 반환한다.
    
    타입
      
      function getCurrentScope(): EffectScope | undefined
  
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  onScopeDispose()

    현재 활성 effectScope에 중지(폐기) 콜백을 등록한다. 연결된 effectScope이 중지되면 콜백이 호출된다.

    각 Vue 컴포넌트의 setup() 함수도 effectScope에서 호출되기 때문에, 비-컴포넌트-결합(non-component-coupled)에서 재사용 가능한 컴포지션 함수인 onUnmounted의 대체로 사용할 수 있다.

    타입

      function onScopeDispose(fn: () => void): void

    예제

      const scope = effectScope()

      scope.run(() => {
        onScopeDispose(() => {
          console.log('effectScope가 중지됨!')
        })
      })

      scope.stop() // 로그: 'effectScope가 중지됨!'

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
