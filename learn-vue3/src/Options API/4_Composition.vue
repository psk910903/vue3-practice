<template>
	<div></div>
</template>
<!--  

  provide

    하위 컴포넌트에 주입(Inject)할 수 있도록 값을 제공(Provide)한다.

    타입

      interface ComponentOptions {
        provide?: object | ((this: ComponentPublicInstance) => object)
      }

    세부 사항

      부모 컴포넌트가 값을 제공하면, 컴포넌트 계층 구조의 깊이와 관계없이 모든 자식 컴포넌트에서 선택적으로 의존성을 주입받을 수 있도록, provide와 inject는 함께 사용된다.

      provide 옵션은 객체이거나 객체를 반환하는 함수여야 한다. 이 객체는 자식 컴포넌트에 주입할 수 있는 속성으로 구성되며, 심볼(Symbol)을 키로 사용할 수 있다.

    예제

      기본 사용법

        const s = Symbol()

        export default {
          provide: {
            foo: 'foo',
            [s]: 'bar'
          }
        }

      함수 사용법

        export default {
          data() {
            return {
              msg: 'foo'
            }
          },
          provide() {
            return {
              msg: this.msg
            }
          }
        }

      위 예제에서 제공된 msg는 반응형이 아니다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  inject

    상위 컴포넌트 또는 app.provide()를 통해 앱에서 제공(Provide)된 값 중, 주입(Inject)할 속성을 선언한다.

    타입

      interface ComponentOptions {
        inject?: ArrayInjectOptions | ObjectInjectOptions
      }

      type ArrayInjectOptions = string[]

      type ObjectInjectOptions = {
        [key: string | symbol]:
          | string
          | symbol
          | { from?: string | symbol; default?: any }
      }

    세부 사항

      inject 옵션은 다음 중 하나이다.

      - 문자열로 이루어진 배열
      - 객체의 키는 인스턴스에 바인딩 될 이름이고, 값은 다음 중 하나이다.
        - 문자열 또는 심볼로 되어있는 주입 가능한 속성의 키.
        - 다음 속성 중 하나 이상으로 이루어진 객체
          - from: 문자열 또는 심볼로 되어있는 주입 가능한 속성의 키.
          - default: 대체 값으로 사용되며, props의 default와 유사, 대체될 값이 객체 타입일 경우, 컴포넌트의 여러 인스턴스 간 값의 참조가 공유되는 것을 피하기 위해 펙터리 함수를 사용해야 함

      일치하는 속성이나 기본값이 제공되지 않는 경우, 주입된 속성의 값은 undefined이다.

      주입되어 바인딩된 값은 반응형이 아니며, 이것은 의도적이다. 그러나 주입된 값이 반응형 객체인 경우, 해당 객체의 속성은 반응형으로 유지된다.

    예제

      기본 사용법

        export default {
          inject: ['foo'],
          created() {
            console.log(this.foo)
          }
        }

      주입된 값을 prop의 기본값으로 사용

        export default {
          inject: ['foo'],
          props: {
            bar: {
              default() {
                return this.foo
              }
            }
          }
        }

      주입된 값을 데이터 입력값으로 사용

        export default {
          inject: ['foo'],
          data() {
            return {
              bar: this.foo
            }
          }
        }

      주입될 값에 기본값을 설정한 옵션을 제공

        export default {
          inject: {
            foo: { default: 'foo' }
          }
        }

      주입할 속성의 이름을 다르게 해야 할 경우, from을 사용하여 주입할 속성 표기

        export default {
          inject: {
            foo: {
              from: 'bar',
              default: 'foo'
            }
          }
        }

      펙토리 함수를 사용한 객체 타입의 기본값 지정

        export default {
          inject: {
            foo: {
              from: 'bar',
              default: () => [1, 2, 3]
            }
          }
        }

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  mixins

    현재 컴포넌트에 혼합할 옵션 객체의 배열이다.

    타입

      interface ComponentOptions {
        mixins?: ComponentOptions[]
      }

    세부 사항

      mixins 옵션은 믹스인 객체로 이루어진 배열이다. 믹스인 객체는 일반 인스턴스 객체와 같은 옵션을 포함할 수 있으며, 특정 옵션 병합 로직을 사용하여 병합된 최종 옵션이 인스턴스에서 사용된다.
      예를 들어 믹스안에 created 훅이 있고, 컴포넌트에도 훅이 있으면, 두 함수가 모두 호출된다.

      믹스인 훅은 제공된 순서대로 호출되며, 컴포넌트 자체 훅보다 먼저 호출된다.

      * 더 이상 권장되지 않음

      - Vue2 에서는 mixins이 컴포넌트 로직의 재사용 가능한 청크를 생성하는 주요 매커니즘이었다. Vue 3에서도 mixins은 여전히 지원되지만,
        Composition API를 사용한 컴포저블 함수는 이제 컴포넌트 간에 코드를 재사용하는 선호되는 방법이다.

    예제

      const mixin = {
        created() {
          console.log(1)
        }
      }

      export default {
        createApp({
          created() {
            console.log(2)
          },
          mixins: [mixin]
        })
      }

      // => 1
      // => 2

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  extends

    현재 컴포넌트를 확장(extend)할 "클래스 기반" 컴포넌트이다.

    타입

      interface ComponentOptions {
        extends?: ComponentOptions
      }

    세부 사항

      B 컴포넌트 확장에 A 컴포넌트를 사용하면, A 컴포넌트의 옵션을 상속받을 수 있다.

      구현의 관점에서는 extens는 mixins와 거의 동일하다. extens로 지정된 컴포넌트는 첫번째 믹스인인 것처럼 처리된다.

      그러나 extens와 mixins는 구현된 목적이 다르다. 주로 mixins 옵션은 기능 묶음을 구성하는데 사용되지만, extens는 상속과 관련된다.

      mixins와 마찬가지로, setup()을 제외한 다른 옵션들은 해당하는 병합 전략을 사용하여 병합된다.

    예제

      // CompB.vue 파일에서

      import CompA from './CompA.vue'

      export default {
        // CompB 컴포넌트는 CompA에 정의된,
        // 옵션 속성들을 상속받음
        extends: CompA,
        ...
      }

      * Composition API에 권장되지 않음

        extends는 Options API를 위해 설계된 것으로, setup() 훅의 병합을 처리하지 않는다.
        Composition API에서는 로직 재사용에 대해 "상속(inheritance)"보다는 "조합(compose)"이 선호되는 개념 모델이다.
        다른 컴포넌트에서 재사용해야 할 로직이 있다면, 해당 로직을 컴포저블(Composable)로 추출하는 것을 고려해봐야 한다.

        Composition API를 사용하여 여전히 컴포넌트를 "확장(extend)"하려면, 확장하는 컴포넌트의 setup()에서 기본 컴포넌트의 setup()을 호출할 수 있다.

          import Base from './Base.js'
          export default {
            extends: Base,
            setup(props, ctx) {
              return {
                ...Base.setup(props, ctx),
                // 로컬 바인딩
              }
            }
          }
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
