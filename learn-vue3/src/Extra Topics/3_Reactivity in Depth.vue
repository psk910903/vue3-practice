<template>
	<div></div>
</template>
<!--  

    반응형 심화

        Vue의 가장 독특한 기능 중 하나는 눈에 거슬리지 않는 반응성 시스템이다.
        컴포넌트 상태는 반응형 자바스크립트 객체로 구성된다. 이를 수정하면 뷰가 업데이트 된다.
        상태 관리를 간단하고 직관적으로 만들지만 몇 가지 일반적인 문제를 피하려면 작동 방식을 이해하는 것도 중요하다.
        이 섹션에서는 Vue의 반응성 시스템에 대한 몇 가지 하위 수준의 세부 사항을 살펴볼 것이다.

    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

        반응형이란?

            요즘 프로그램에서 이 용어가 자주 등장하는데, 사람들이 이 말을 할 때 무슨 뜻일까?
            반응성은 선언적인 방식으로 변화에 적응할 수 있는 프로그래밍 패러다임이다.
            반응형 프로그래밍이 훌륭하기 때문에 사람들이 일반적으로 보여주는 대표적인 예는 Excel 스프레드시트이다.

            스프레드시트의 셀은 수식을 통해 정의되므로 스프레드시트는 우리에게 계산된 값을 제공한다.
            그리고 수식에 사용된 값을 업데이트하면, 결과값도 자동으로 업데이트됨을 알 수 있다.

            JavaScript는 일반적으로 이렇게 작동하지 않는다. JavaScript에서 비슷한 것을 작성한다면

                let A0 = 1
                let A1 = 2
                let A2 = A0 + A1

                console.log(A2) // 3

                A0 = 2
                console.log(A2) // 여전히 3

            A0를 변경한다고 A2가 자동으로 변경되지 않는다.

            그러면 JavaScript에서 이것을 어떻게 해야 할까? 먼저 A2를 업데이트하는 코드를 다시 실행하기 위해 다음과 같이 함수로 래핑해야 한다.

                let A2

                function update() {
                    A2 = A0 + A1
                }

            그런 다음 몇 가지 용어를 정의해야 한다.

                - update() 함수는 프로그램의 상태를 수정하기 때문에 "사이드 이팩트" 줄여서 "이팩트"를 생성한다.

                - A0 및 A1은 해당 값이 이팩트를 수행하는 데 사용되므로 이팩트의 "의존성"(dependency)으로 간주된다. 그 이팩트는 의존성에 대한 "구독자"(subscriber)라고 한다.

            우리게에 필요한 것은 A0 또는 A1(의존성)이 변경될 때마다 update() (이팩트)를 호출할 수 있는 함수이다.

                whenDepsChange(update)

            이 whenDepsChange(update) 함수는 다음과 같은 작업이 있다.

                1. 변수가 읽힐 때 추적한다. 예를 들어 A0 + A1 표현식을 평가할 때 A0와 A1이 모두 읽힌다.

                2. 현재 실행 중인 이팩트가 있을 때 변수를 읽으면, 해당 이팩트를 해당 변수의 구독자로 만든다. 
                    예를 들어 A0dhk A1은 update()가 실행될 때 읽히기 때문에 update()는 첫 번째 호출 이후에 A0과 A1의 구독자가 된다.

                3. 변수가 언제 변이되는지 감지한다. 예를 들어 A0에 새 값이 할당되면, 모든 구독자 이팩트에 다시 실행하도록 알린다.

    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

        Vue에서 반응형 작동 방식

            예제에서처럼 로컬 변수의 읽기 및 쓰기를 추적할 수는 없다. 바닐라 자바스크립트에는 이를 수행할 수 있는 메커니즘이 없기 때문이다.
            하지만 우리가 할 수 있는 것은 객체 속성의 읽기 및 쓰기를 가로채는 것이다.

            JavaScript에서 속성 접근을 가로채는 방법에는 getter/setter 및 프락시의 두 가지가 있다. Vue 2는 브라우저 지원 제한으로 인해 단독으로
            getter/setter를 사용했다. Vue 3에서 프락시는 반응형 객체에 사용되면 getter/setter는 참조에 사용된다. 다음은 작동 방식을 보여주는 유사 코드이다.

                function reactive(obj) {
                    return new Proxy(obj, {
                        get(target, key) {
                            track(target, key)
                            return target[key]
                        },
                        set(target, key, value) {
                            target[key] = value
                            trigger(target, key)
                        }
                    })
                }

                function ref(value) {
                    const refObject = {
                        get value() {
                            track(refObject, 'value')
                            return value
                        },
                        set value(newValue) {
                            value = newValue
                            trigger(refObject, 'value')
                        }
                    }
                    return refObject
                }

            TIP
                여기와 아래의 코드 스니펫은 가능한 한 간단한 형태로 핵심 개념을 설명하기 위한 것이기 때문에 많은 세부 사항은 생략되고 예외적인 케이스는 무시된다.

            아래는 우리가 기초 섹션에서 논의한 몇 가지 반응형의 제한 사항이다.

                1. 반응형 객체의 속성을 로컬 변수에 할당하거나 해체할 때, 해당 변수에 접근하거나 할당하는 것은 비반응형(non-reactive)이다.
                    이는 소스 객체의 get/set 프록시 트랩을 더 이상 트리거하지 않기 때문이다. 이러한 "끊김(disconnect)"은 변수 바인딩에만 영향을 미친다.
                    변수가 객체와 같은 원시 타입이 아닌 값을 가리키는 경우, 해당 객체를 수정하는 것은 여전히 반응형으로 동작한다.

                2. reactive()에서 반환된 프락시는 원본가 동일하게 동작하지만 === 연산자를 사용하여 원본과 비교하면 다른 ID를 갖는다.

            track() 내부에서 현재 실행 중인 이팩트가 있는지 확인한다. 존재하는 경우, 추적 중인 속성에 대한 구독자 이팩트(Set에 저장됨)를 조회하고 이팩트를 Set에 추가한다.

                // 이팩트가 실행되기 직전에 설정된다.
                // 나중에 배울 예정.
                let activeEffect

                function track(target, key) {
                    if (activeEffect) {
                        const effects = getSubscribersForProperty(target, key)
                        effects.add(activeEffect)
                    }
                }

            이팩트 구독은 전혀 WeakMap<target, Map<key, set<effect>>> 데이터 구조에 저장된다. 속성에 대한 구독 이팩트 Set이 발견되지 않은 경우 (처음 추적 시) 생성된다.
            이것이 setSubscribersForProperty() 함수가 하는 일이다. 간단하게 하기 위해 자세한 내용은 건너뛰겠다.

            trigger() 내부에서 속성에 대한 구독자 이팩트를 다시 조회한다. 그러나 이벤에는 우리가 그것들을 대신 호출한다.

                function trigger(target, key) {
                    const effects = getSubscribersForProperty(target, key)
                    effects.forEach((effect) => effect())
                }

            이제 다시 whenEdpsChange() 함수로 돌아가 보면

                function whenDepsChange(update) {
                    const effect = () => {
                        activeEffect = effect
                        update()
                        activeEffect = null
                    }
                    effect()
                }

            실제 업데이트를 실행하기 전에 자신을 현재 활성 이팩트로 설정하는 이팩트에 로우(raw) 업데이트 함수를 래핑한다.
            이것은 현재 활성 이팩트를 찾기 위해 업데이트 동안 track() 호출을 활성화한다.

            이 시점에서 의존성을 자동으로 추적하고 의존성이 변경될 때마다 다시 실행하는 이팩트를 만들었다.
            이것을 반응 반응 이팩트(Reactive Effect)라고 부른다.

            Vue는 반응 이팩트를 생성할 수 있는 watchEffect() API를 제공한다. 사실 예제의 whenDepsChange()와 매우 유사하게 작동한다는 것을 눈치챘을 것이다.
            이제 실제 Vue API를 사용하여 원래 예제를 다시 작업할 수 있다.

                import { ref, watchEffect } from 'vue'

                const A0 = ref(0)
                const A1 = ref(1)
                const A2 = ref()

                watchEffect(() => {
                    // A0과 A1을 추적함
                    A2.value = A0.value + A1.value
                })

                // 이팩트가 트리거됨
                A0.value = 2

            반응 이팩트를 사용하여 ref를 변경하는 것이 가장 흥미로운 사례는 아니다. 
            계산된 속성을 사용하면 더 선언적이다.

                import { ref, computed } from 'vue'

                const A0 = ref(0)
                const A1 = ref(1)
                const A2 = computed(() => A0.value + A1.value)

                A0.value = 2

            내부적으로 computed는 무효화 및 재계산을 반응 이팩트를 사용하여 관리한다.

            그렇다면 일반적이고 유용한 반응 이팩트의 예는 무엇일까?
            DOM을 업데이트 하는 것이다. 다음과 같이 간단한 "반응형 렌더링"을 구현할 수 있다.

                import { ref, watchEffect } from 'vue'

                const count = ref(0)

                watchEffect(() => {
                    document.body.innerHTML = `숫자 세기: ${count.value}`
                })

                // DOM 업데이트
                count.value++

            사실 이것은 Vue 컴포넌트가 상태와 DOM을 동기화 상태로 유지하는 방법과 매우 유사하다. 각 컴포넌트 인스턴스는 DOM을 렌더링하고 업데이트하는 반응 이팩트를 생성한다.
            물론 Vue 컴포넌트는 innerHTML 보다 훨씬 더 효율적인 방법으로 DOM을 업데이트한다.

    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

        런타임 (실행 시) vs 컴파일 타임 (컴파일 시) 반응형

            Vue의 반응성 시스템은 주로 런타임 기반이다. 추적 및 트리거는 코드가 브라우저에서 직접 실행되는 동안 수행된다.
            런타임 반응성의 장점은 빌드 단계 없이 작동하며 예외 상황이 더 적다. 
            반면에 JavaScript의 구문 제한 때문에 구문 제한이 있는 JavaScript 값 컨테이너인 Vue refs와 같은 값 컨테이너의 필요성이 생긴다.

            Sevlte와 같은 몇몇 프레임워크는 컴파일 중에 반응성을 구현하여 이러한 제한을 극복하기로 선택했다. 이 프레임워크는 코드를 분석하고 변환하여 반응성을 시뮬레이션한다.
            컴파일 단계를 통해 프레임워크는 JavaScript 자체의 의미를 변경할 수 있다. 예를 들어 로컬로 정의된 변수에 대한 엑세스 주변의 종속성 분석 및 효과 트리거를 수행하는 코드를 암시적으로 삽입할 수 있다.
            단점은 이러한 변환이 빌드 단계를 필요로 하며, JavaScript 의미를 변경하는 것은 본질적으로 JavaScript처럼 보이지만 다른 것으로 컴파일되는 언어를 생성하는 것이다.

            Vue 팀은 이 방향을 탐색하기 위해 실험적 기능을 통해 이를 탐색했지만, 결국 프로젝트에 적합하지 않다고 결정했다.

    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

        반응형 디버깅

            Vue의 반응성 시스템이 종속성을 자동으로 추적하는 것은 훌륭하지만, 어떤 경우에는 추적 대상 도는 컴포넌트가 렌더링되는 원인을 정확히 파악하고 싶을 수 있다.

            컴포넌트 디버깅 훅

                컴포넌트의 렌더링 중에 사용되는 의존성과 onRenderTracked 및 onRenderTriggered 생명 주기 훅을 사용하여 업데이트를 트리거하는 의존성을 디버그할 수 있다.
                두 훅 모두 해당 의존성에 대한 정보가 포함된 디버거 이벤트를 수신한다. 의존성을 대화식으로 검사하기 위해 콜백에 debugger 문을 배치하는 것이 좋다.

                    <script setup>
                    import { onRenderTracked, onRenderTriggered } from 'vue'

                    onRenderTracked((event) => {
                        debugger
                    })

                    onRenderTriggered((event) => {
                        debugger
                    })
                    </script>

                TIP
                    컴포넌트 디버그 훅은 개발 모드에서만 작동한다.

                디버그 이벤트 객체의 타입은 다음과 같다.

                    type DebuggerEvent = {
                        effect: ReactiveEffect
                        target: object
                        type:
                            | TrackOpTypes /* 'get' | 'has' | 'iterate' */
                            | TriggerOpTypes /* 'set' | 'add' | 'delete' | 'clear' */
                        key: any
                        newValue?: any
                        oldValue?: any
                        oldTarget?: Map<any, any> | Set<any>
                    }

            계산된 속성 디버깅

                onTrack 및 onTrigger 콜백이 있는 두 번째 옵션 객체를 computed()에 전달하여 계산된 속성을 디버그할 수 있다.

                    1. onTrack은 반응 속성 또는 ref가 의존성으로 추적될 때 호출된다.
                    2. onTrigger는 의존성의 변경에 의해 감시자 콜백이 트리거될 때 호출된다.

                두 콜백 모두 컴포넌트 디버그 훅과 동일한 형식의 디버거 이벤트를 수신한다.

                    const plusOne = computed(() => count.value + 1, {
                        onTrack(e) {
                            // count.value가 의존성으로 추적될 때 트리거된다.
                            debugger
                        },
                        onTrigger(e) {
                            // count.value가 변경되면 트리거된다.
                            debugger
                        }
                    })

                    // plusOne에 접근, onTrack을 트리거해야 한다.
                    console.log(plusOne.value)

                    // count.value를 변경, onTrigger를 트리거해야 한다.
                    count.value++

                TIP
                    계산된 속성의 onTrack 및 onTrigger 옵션은 개발 모드에서만 작동한다.

            감시자 디버깅

                computed()와 유사하게 감시자는 onTrack 및 onTrigger 옵션을 지원한다.

                    watch(source, callback, {
                        onTrack(e) {
                            debugger
                        },
                        onTrigger(e) {
                            debugger
                        }
                    })

                    watchEffect(callback, {
                        onTrack(e) {
                            debugger
                        },
                        onTrigger(e) {
                            debugger
                        }
                    })

                TIP
                    감시자의 onTrack 및 onTrigger 옵션은 개발 모드에서만 작동한다.

    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

        외부 상태 시스템의 통합

            Vue의 반응형 시스템은 일반 JavaScript 객체를 반응형 프락시로 깊이 변환하여 작동한다. 깊은 변환은 외부 상태 관리 시스템과 통합할 때 필요하지 않거나 때때로 원하지 않을 수 있다.
            (예: 외부 솔루션도 프락시를 사용하는 경우)

            Vue의 반응형 시스템을 외부 상태 관리 솔루션과 통합하는 일반적인 아이디어는 외부 상태를 shallowRef에 유지하는 것이다. 얕은 참조는 .value 속성에 접근할 때만 반응한다.
            내부 값은 그대로 유지된다. 외부 상태가 변경되면 ref 값을 교체하여 업데이트를 트리거한다.

            불변 데이터

                실행 취소/다시 실행 기능을 구현하는 경우, 사용자가 편집할 때마다 앱 상태의 스냅샵을 찍고 싶을 것이다.
                그러나 Vue의 변경 가능한 반응형 시스템은 상태 트리가 큰 경우 적합하지 않다.
                모든 업데이트에서 전체 상태 객체를 직렬화하는 것은 CPU 및 메모리 비용 측면에서 비용이 많이 들 수 있기 때문이다.

                불변 데이터 구조는 상태 객체를 변경하지 않음으로써 이 문제를 해결한다. 대신 이전 객체와 동일하고 변경되지 않은 부분을 공유하는 새 객체를 생성한다.
                JavaScript에서 변경할 수 없는 데이터를 사용하는 방법에는 여러 가지가 있지만, Vue와 함께 Immer를 사용하는 것이 좋다.
                왜냐하면 보다 인체 공학적이고 변경 가능한 문법을 유지하면서 변경할 수 없는 데이터를 사용할 수 있기 때문이다.

                간단히 컴포넌트를 통해 Immer를 Vue와 통합할 수 있다.

                    import produce from 'immer'
                    import { shallowRef } from 'vue'

                    export function useImmer(baseState) {
                        const state = shallowRef(baseState)
                        const update = (updater) => {
                            state.value = produce(state.value, updater)
                        }

                        return [state, update]
                    }

            상태 머신 (State Machine)

                상태 머신은 앱이 어떤 상태에 있을 수 있는 모든 가능한 상태와 한 상태에서 다른 상태로 전환할 수 있는 모든 가능한 방법을 설명하기 위한 모델이다.
                단순한 컴포넌트에는 과도할 수 있지만, 복잡한 상태 흐름을 보다 강력하고 관리하기 쉽게 만드는 데 도움이 될 수 있다.

                JavaScript에서 가장 널리 사용되는 상태 머신 구현 중 하나는 XState이다.
                다음은 이를 통합하는 컴포저블이다.

                    import { createMachine, interpret } from 'xstate'
                    import { shallowRef } from 'vue'

                    export function useMachine(options) {
                        const machine = createMachine(options)
                        const state = shallowRef(machine.initialState)
                        const service = interpret(machine)
                            .onTransition((newState) => (state.value = newState))
                            .start()
                        const send = (event) => service.send(event)

                        return [state, send]
                    }

            RxJS

                RxJS는 비동기 이벤트 스트림 작업을 위한 라이브러리이다. VueUse 라이브러리는 RxJS 스트림을 Vue의 반응형 시스템과 연결하기 위한 @vueuse/rxjs 추가 기능을 제공한다.

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
