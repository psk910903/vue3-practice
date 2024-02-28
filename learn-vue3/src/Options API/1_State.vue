<template>
	<div></div>
</template>
<!--  

  State

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  data

    컴포넌트 인스턴스의 초기 반응형 상태를 반환하는 함수이다.

    타입

      interface ComponentOptions {
        data?(
          this: ComponentPublicInstance,
          vm: ComponentPublicInstance
        ): object
      }

    세부 사항

      이 함수는 Vue에 의해 반응형을 만들어진 일반 JavaScript 객체를 반환한다. 인스턴스가 생성된 후, 반응형 데이터 객체는 this.$data로 접근할 수 있다.
      컴포넌트 인스턴스는 데이터 객체에서 찾은 모든 속성을 프록시하므로 this.$data.a는 this.a와 동일하다.

      모든 최상위 데이터 속성은 반환된 데이터 객체에 포함되어야 한다. this.$data에 새 속성을 추가하는 것이 가능하지만 권장되지는 않는다.
      원하는 속성 값을 아직 사용할 수 없는 경우, undefined 또는 null과 같은 빈 값을 사용하여 Vue가 속성이 존재함을 알 수 있도록 해야 한다.

      _또는 $로 시작하는 속성은 Vue의 내부 속성 및 API 메서드와 충돌할 수 있으므로, 컴포넌트 인스턴스에서 프록시되지 않는다.
      이런 경우에는 this.$data._property와 같은 방법으로 접근해야 한다.

      브라우저 API 객체나 프로토타입 속성과 같은 고유한 상태를 저장한 객체를 반환하는 것은 권장하지 않는다.
      이상적으로 반환된 객체는 컴포넌트의 상태만 나타내는 일반 객체야야 한다.

    예제

      export default {
        data() {
          return { a: 1 }
        },
        created() {
          console.log(this.a) // 1
          console.log(this.$data) // { a: 1 }
        }
      }

      data 속성에 화살표 함수를 사용하는 경우, this는 컴포넌트의 인스턴스가 아니지만 함수의 첫 번째 인자로 인스턴스에 접근할 수 있다.

        data: (vm) => ({ a: vm.myProp })

      
      data()는 Vue 컴포넌트의 초기 상태를 정의하는 함수이다. 이 함수를 통해 정의된 데이터는 해당 컴포넌트의 반응성을 갖게 된다.

      주의할 점은 data() 함수가 객체를 반환해야 한다는 것이다. 이 객체에는 컴포넌트의 데이터 속성과 초기 값이 포함되어야 한다.

      data() 함수를 사용하는 이유는 Vue 컴포넌트가 재사용될 때마다 새로운 데이터 객체를 생성하여 각 인스턴스가 독립적인 데이터를 가지게 하기 위함이다.

      예를 들어, 다음과 같이 data() 메서드를 사용하여 초기 데이터를 정의할 수 있다.
      
        Vue.component('my-component', {
          data() {
            return {
              message: 'Hello, Vue!'
            };
          }
        });

      위 예제에서 data() 메서드는 객체를 반환하고, 해당 객체에는 message라는 데이터 속성이 포함되어 있다.
      이 데이터 속성은 컴포넌트의 초기 상태를 나타내며, 화면에 렌더링될 때 사용된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  props

    컴포넌트의 props를 선언한다.

    타입

      interface ComponentOptions {
        props?: ArrayPropsOptions | ObjectPropsOptions
      }

      type ArrayPropsOptions = string[]

      type ObjectPropsOptions = { [key: string]: Prop }

      type Prop<T = any> = PropOptions<T> | PropType<T> | null

      interface PropOptions<T> {
        type?: PropType<T>
        required?: boolean
        default?: T | ((rawProps: object) => T)
        validator?: (value: unknown, rawProps: object) => boolean
      }

      type PropType<T> = { new (): T } | { new (): T }[]

    세부 사항

      Vue에서 모든 컴포넌트 props는 명시적으로 선언되어야 한다. 컴포넌트 props는 두 가지 방식으로 선언할 수 있다.

      1. 문자열 배열을 이용한 간단한 형태
      2. "키: prop의 이름", "값: 타입(생성자 함수) 또는 고급 옵션"을 속성으로 하는 객체를 이용한 완전한 형태

      객체 기반 문법을 사용하면, 각 prop은 다음 옵션을 추가로 정의할 수 있다.

      - type: 다음 네이티브 생성자 중 하나일 수 있따. String, Number, Boolean, Array, Object, Date, Funtion, Symbol, "모든 커스텀 생성사 함수" 또는 이것들로 이루어진 배열.
        개발 모드에서 Vue는 prop의 값이 선언된 타입과 일치하는지 확인하고, 일치하지 않으면 경고를 표시한다.

        Boolean 타입은 개발 및 프로덕션에서 값을 캐스팅 하는 동작에 영향을 준다.

      - default: 부모로부터 전달받지 않거나 undefined 값인 경우, prop에 지정할 기본값이다. 기본값이 객체 또는 배열일 경우, 팩토리 함수를 사용하여 반환돼야 한다.
        팩토리함수는 rawProps(상위 컴포넌트에게 받은 props 전체 객체)를 인자로 받는다.

      - required: prop이 필수인지 정의한다. 비프로덕션 환경에서 이 값이 true이고 prop이 전달되지 않으면 콘솔 경고가 발생한다.

      - validator: prop값을 유일한 인자로 사용하는 커스텀 유효성 검사 함수이다. 개발 모드에서 이 함수가 거짓 값을 반환하면 (유효성 검사가 실패하면) 콘솔 경고가 발생한다.

    예제

      간단하게 선언하기

        export default {
          props: ['size', 'myMessage']
        }
      
      유효성 검사가 포함된 객체로 선언

        export default {
          props: {
            // 타입 체크
            height: Number,
            // 타입 체크 및 유효성 검사
            age: {
              type: Number,
              default: 0,
              required: true,
              validator: (value) => {
                return value >= 0
              }
            }
          }
        }

    Props는 Vue 컴포넌트에서 데이터를 전달하는 메커니즘이다. 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달할 때 사용된다.
    Props는 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달하는 단방향 통신 방식을 따른다.

    Props는 자식 컴포넌트에서 변경할 수 없는 읽기 전용 속성으로 취급된다. 부모 컴포넌트에서 전달된 데이터를 자식 컴포넌트 내부에서 사용할 수 있지만, 해당 데이터를 변경할 수는 없다.

    Props를 사용하여 데이터를 전달할 때는 다음과 같은 과정을 거친다.

    1. 부모 컴포넌트에서 Props를 사용하여 데이터를 자식 컴포넌트로 전달한다.
    2. 자식 컴포넌트에서는 Props를 통해 전달받은 데이터를 사용한다.

    Porps를 사용하기 위해서는 다음과 같은 방법으로 자식 컴포넌트에 Props를 선언한다.

      <template>
        <div>
          <h1>{{ title }}</h1>
          <p>{{ content }}</p>
        </div>
      </template>

      <script>
      export default {
        props: {
          title: String,
          content: String
        }
      }
      </script>

    위 예제에서 props 객체 내에는 title과 content라는 두 개의 Props가 선언되어 있다. 각 Props는 해당 데이터 타입을 지정할 수 있으며, 이 예제에서는 문자열(String) 타입으로 지정되어 있다.

    부모 컴포넌트에서 다음과 같이 Props를 사용하여 데이터를 자식 컴포넌트로 전달할 수 있다.

      <template>
        <div>
          <child-component title="Hello Vue" content="Welcome to Vue.js"></child-component>
        </div>
      </template>

      <script>
      import ChildComponent from './ChildComponent.vue'

      export default {
        components: {
          ChildComponent
        }
      }
      </script>

    위 예제에서는 title과 content Props에 각각 Hello Vue와 Welcome to Vue.js라는 값을 전달하고 있다. 이러한 방식으로 Props를 사용하여 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  computed

    컴포넌트 인스턴스에 노출될 계산된 속성을 선언한다.

    타입

      interface ComponentOptions {
        computed?: {
          [key: string]: ComputedGetter<any> | WritableComputedOptions<any>
        }
      }

      type ComputedGetter<T> = (
        this: ComponentPublicInstance,
        vm: ComponentPublicInstance
      ) => T

      type ComputedSetter<T> = (
        this: ComponentPublicInstance,
        value: T
      ) => void

      type WritableComputedOptions<T> = {
        get: ComputedGetter<T>
        set: ComputedSetter<T>
      }

    세부 사항

      이 옵션은 "키: 계산된 속성의 이름", "값: 계산된 getter 또는 get과 set(쓰기 가능한 계산된 속성의 경우) 메서드가 있는 객체"로 이루어진 객체이다.
      모든 getter와 setter에는 컴포넌트 인스턴스에 자동으로 바인딩된 this 컨텍스트가 있다.

      화살표 함수를 사용한 계산된 속성의 경우, this는 컴포넌트의 인스턴스를 가리키지 않지만, 여전히 함수의 첫 번째 인자로 인스턴스에 접근할 수 있다.

        export default {
          computed: {
            aDouble: (vm) => vm.a * 2
          }
        }

    예제

      export default {
        data() {
          return { a: 1 }
        },
        computed: {
          // 읽기전용
          aDouble() {
            return this.a * 2
          },
          // 쓰기가능
          aPlus: {
            get() {
              return this.a + 1
            },
            set(v) {
              this.a = v - 1
            }
          }
        },
        created() {
          console.log(this.aDouble) // => 2
          console.log(this.aPlus) // => 2

          this.aPlus = 3
          console.log(this.a) // => 2
          console.log(this.aDouble) // => 4
        }
      }

    computed() 함수는 Vue.js에서 제공하는 계산된 속성(Computed Property)을 정의하는 방법 중 하나이다.
    계산된 속성은 다른 데이터를 기반으로 계산되는 속성으로, 해당 데이터가 변경될 때만 다시 계산된다.

    computed() 함수는 Vue 컴포넌트의 옵션 중 하나로 사용되며, 계산된 속성을 정의하는 데에 사용된다.
    이 함수는 함수 형태로 정의되며, 반환된 값은 계산된 속성의 값으로 사용된다.

    computed() 함수를 사용하여 계산된 속성을 정의할 때는 다음과 같은 방법을 사용한다.

      export default {
        computed: {
          // 계산된 속성의 이름: 함수
          // 함수는 해당 계산된 속성의 값을 반환
          fullName() {
            return this.firstName + ' ' + this.lastName;
          }
        }
      }

    위의 예제에서 fullName이라는 계산된 속성을 정의하고 있다.
    이 계산된 속성은 firstName과 lastName 데이터를 기반으로 계산되며, 해당 데이터가 변경될 때마다 자동으로 다시 계산된다.

    계산된 속성은 마치 일반 데이터 속성처럼 사용할 수 있지만, 실제로는 함수로 정의되어 있으므로 다른 데이터에 의존하는 값이기 때문에 변경되는 데이터가 있을 때만 다시 계산된다.
    이를 통해 효율적으로 데이터를 계산하고 화면에 반영할 수 있다.

    계산된 속성은 템플릿이나 컴포넌트의 다른 옵션에서도 마치 일반 데이터 속성처럼 사용할 수 있다.
    Vue는 내부적으로 해당 계산된 속성을 추적하고, 해당하는 데이터가 변경될 때마다 자동으로 다시 계산하여 화면에 반영한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  Options API vs Composition API

  Options API

    <template>
      <div>
        <p>Count: {{ count }}</p>
        <p>Doubled Count: {{ doubledCount }}</p>
        <button @click="increment">Increment</button>
      </div>
    </template>

    <script>
    export default {
      data() {
        return {
          count: 0
        };
      },
      computed: {
        doubledCount() {
          return this.count * 2;
        }
      },
      methods: {
        increment() {
          this.count++;
        }
      }
    };
    </script>

  Composition API

    <template>
      <div>
        <p>Count: {{ count }}</p>
        <p>Doubled Count: {{ doubledCount }}</p>
        <button @click="increment">Increment</button>
      </div>
    </template>

    <script>
    import { ref, computed } from 'vue';

    export default {
      setup() {
        // 반응형 상태 정의
        const count = ref(0);
        
        // 계산된 속성 정의
        const doubledCount = computed(() => {
          return count.value * 2;
        });

        // count 값을 증가시키는 메서드
        const increment = () => {
          count.value++;
        };

        return {
          count,
          doubledCount,
          increment
        };
      }
    };
    </script>

    Options API

    - 데이터와 계산된 속성이 각각 data()와 computed 옵션에 정의된다.
    - 계산된 속성으 this 키워드를 사용하여 접근한다.
    - methods 옵션에서 메서드를 정의하고, 해당 메서드 내에서 데이터를 변경한다.

    Composition API

    - setup() 함수 내부에서 데이터와 계산된 속성을 정의한다.
    - 계산된 속성은 바로 함수 내에서 정의되고, 반환된 값에 접근한다.
    - 메서드는 직접 반환 객체에 포함되어 있다.

    두 예시는 동일한 기능을 수행하지만, Composition API를 사용한 예시는 보다 직관적이고 유연하며, 코드를 구성하기 쉽고 재사용성을 높일 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  reactive()

    객체의 반응형 프록시(proxy)를 반환한다.

    타입

      function reactive<T extends object>(target: T): UnwrapNestedRefs<T>

    세부 사항

      반응형 변환은 "내부 깊숙이 있는(deep)" 모든 중첩 속성에 영향을 준다. 또한, 반응형 객체는 내부 깊숙이 잇는 모든 refs 속성의 반응형을 유지하면서 언래핑한다.

      그러나 Map 같은 네이티브 컬렉션 타입 또는 반응형 배열의 요소인 ref로 접근할 ㄸ내느 언래핑 되지 않음에 유의해야 한다.

      내부 깊은 곳까지의 변환은 피하고 루트 수준에서만 반응형을 유지하려면, shallowReactive()를 사용해야 한다.

      반환된 객체와 중첩된 객체는 프록시(Proxy)로 래핑되므로, 원본 객체와 동일하지 않다.
      그로므로 반응형 프록시로만 작업하고, 원본 객체에 의존하지 않는 것이 좋다.

    예제

      반응형 객체 생성

        const obj = reactive({ count: 0 })
        obj.count++

      Ref 언래핑

        const count = ref(1)
        const obj = reactive({ count })

        // obj.count인 ref는 언래핑 됨
        console.log(obj.count === count.value) // true

        // `obj.count`도 업데이트 됨
        count.value++
        console.log(count.value) // 2
        console.log(obj.count) // 2

        // `count` ref도 업데이트 됨
        obj.count++
        console.log(obj.count) // 3
        console.log(count.value) // 3

      배열 또는 컬렉션 엘리먼트인 ref에 접근시, 언래핑 되지 않는다.

        const books = reactive([ref('Vue 3 Guide')])
        // .value 필요
        console.log(books[0].value)

        const map = reactive(new Map([['count', ref(0)]]))
        // .value 필요
        console.log(map.get('count').value)

      ref를 reactive 속성에 할당하면, 해당 ref도 자동으로 언래핑된다.

        const count = ref(1)
        const obj = reactive({})

        obj.count = count

        console.log(obj.count) // 1
        console.log(obj.count === count.value) // true

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  methods

    컴포넌트 인스턴스에서 사용할메서드를 선언한다.

    타입

      interface ComponentOptions {
        methods?: {
          [key: string]: (this: ComponentPublicInstance, ...args: any[]) => any
        }
      }

    세부 사항

      선언된 메서드는 컴포넌트 인스턴스에서 직접 접근하거나, 템플릿 표현식에서 사용할 수 있다.
      모든 메서드에는 해당 컴포넌트 인스턴스에 자동으로 바인딩된 this 컨텍스트가 있으며, 주위 컴포넌트로 전달된 경우에도 유지된다.

      메서드를 선언할 때 화살표 함수를 사용하지 않도록 해야 하는데, this를 통해 컴포넌트 인스턴스에 접근할 수 없기 때문이다.

    예제

      export default {
        data() {
          return { a: 1 }
        },
        methods: {
          plus() {
            this.a++
          }
        },
        created() {
          this.plus()
          console.log(this.a) // => 2
        }
      }

    methods는 Vue 컴포넌트의 메서드를 정의하는 데 사용된다.
    이 속성은 컴포넌트의 인스턴스 메서드를 포함하는 객체이다. 메서드는 일반 JavaScript 함수로 정의되며, 컴포넌트 내에서 해당 메서드를 호출하여 특정 동작을 수행할 수 있다.

    예를 들어, 다음은 Options API에서 methods를 사용하여 Vue 컴포넌트에 메서드를 추가하는 예제이다.

      export default {
        data() {
          return {
            count: 0
          };
        },
        methods: {
          increment() {
            this.count++;
          },
          decrement() {
            this.count--;
          }
        }
      };

    위의 예제에서 methods 객체 안에 increment와 decrement 메서드가 정의되어 있다. 이들 메서드는 Vue 컴포넌트의 인스턴스 메서드로 취급되며, 컴포넌트 내에서 this를 사용하여 호출할 수 있다.

      <template>
        <div>
          <button @click="increment">Increment</button>
          <span>{{ count }}</span>
          <button @click="decrement">Decrement</button>
        </div>
      </template>

    위의 템플릿에서 @click 디렉티브를 사용하여 버튼 클릭 이벤트가 발생할 때 increment와 decrement 메서드를 호출하고 있다.

    Options API의 methods를 사용하면 컴포넌트의 로직을 단순화하고 코드를 재사용할 수 있다.
    여러 컴포넌트에서 동일한 메서드를 사용해야할 때 특히 유용하다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  watch

    데이터 변경 시 호출될 감시 콜백을 선언한다.

    타입

      interface ComponentOptions {
        watch?: {
          [key: string]: WatchOptionItem | WatchOptionItem[]
        }
      }

      type WatchOptionItem = string | WatchCallback | ObjectWatchOptionItem

      type WatchCallback<T> = (
        value: T,
        oldValue: T,
        onCleanup: (cleanupFn: () => void) => void
      ) => void

      type ObjectWatchOptionItem = {
        handler: WatchCallback | string
        immediate?: boolean // 기본 값: false
        deep?: boolean // 기본 값: false
        flush?: 'pre' | 'post' | 'sync' // 기본 값: 'pre'
        onTrack?: (event: DebuggerEvent) => void
        onTrigger?: (event: DebuggerEvent) => void
      }

    세부 사항

      watch 옵션은 키가 감시할 반응형 컴포넌트 인스턴스 속성(예: data 또는 computed를 통해 선언된 속성)이고, 값이 해당 콜백으로 이루어진 객체이다.
      콜백은 감시되는 소스의 새값과 이전 값을 인자로 수신한다.

      루트 수준 속성이 아닌 경우, 키는 a.b.c와 같이 점으로 구분된 경로가 될 수도 있다. 이 사용법은 복잡한 표현식을 지원하지 않으며, 점으로 구분된 경로만 지원된다.
      복잡한 데이터 소스를 감시해야 하는 경우, 명령형 $watch() API를 사용해야 한다.

      값은 메서드 이름에 해당하는 문자열(methods를 통해 선언됨) 또는 추가적인 옵션을 포함하는 객체일 수도 있다. 
      객체 문법을 사용할 경우, handler 필드에 콜백을 선언해야 한다. 추가 업션은 다음과 같다.

      - immediate: 감시자 생성 시 즉시 콜백 트리거. 첫 호출 시 이전 값은 undefined.
      - deep: 객체 또는 배열인 경우 내부 깊숙한 곳에서 변경 사항 발생 시에도 콜백이 실행되도록 한다.
      - flush: 콜백 실행 타이밍을 조정한다.(watchEffect())
      - onTrack/onTrigger: 감시자의 의존성을 디버그한다.

      감시 콜백을 선언할 때 화살표 함수를 사용해선 안 되는데, this로 컴포넌트 인스턴스에 접근할 수 없기 때문이다.

    예제

      export default {
        data() {
          return {
            a: 1,
            b: 2,
            c: {
              d: 4
            },
            e: 5,
            f: 6
          }
        },
        watch: {
          // 루트 레벨 속성 감시
          a(val, oldVal) {
            console.log(`new: ${val}, old: ${oldVal}`)
          },
          // 메서드 이름 문자열
          b: 'someMethod',
          // 중첩 깊이에 관계없이 감시하는 객체 속성이 변경될 때마다 콜백을 호출
          c: {
            handler(val, oldVal) {
              console.log('c 변경됨')
            },
            deep: true
          },
          // 중첩 속성 감시:
          'c.d': function (val, oldVal) {
            /* ... */
          },
          // 콜백은 감시 시작 직후에도 호출됨
          e: {
            handler(val, oldVal) {
              console.log('e 변경됨')
            },
            immediate: true
          },
          // 콜백 배열을 전달할 수 있으며 하나씩 호출됨
          f: [
            'handle1',
            function handle2(val, oldVal) {
              console.log('handle2 실행됨!')
            },
            {
              handler: function handle3(val, oldVal) {
                console.log('handle3 실행됨!')
              }
              /* ... */
            }
          ]
        },
        methods: {
          someMethod() {
            console.log('b 변경됨')
          },
          handle1() {
            console.log('handle1 실행됨!')
          }
        },
        created() {
          this.a = 3 // => 현재 값: 3, 이전 값: 1
        }
      }

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  emits

    컴포넌트에서 내보낼 커스텀 이벤트를 선언한다.

    타입

      interface ComponentOptions {
        emits?: ArrayEmitsOptions | ObjectEmitsOptions
      }

      type ArrayEmitsOptions = string[]

      type ObjectEmitsOptions = { [key: string]: EmitValidator | null }

      type EmitValidator = (...args: unknown[]) => boolean

    세부 사항

      내보낼 이벤트는 두 가지 타입으로 선언할 수 있다.

      1. 문자열 배열을 이용한 간단한 형태
      2. "키: prop의 이름", "값: null 또는 유효성 검사 함수"를 속성으로 하는 객체를 이용한 완전한 형태

      유효성 검사 함수는 컴포넌트의 $emit 호출에 전달된 인자를 수신한다.
      예를 들어, this.$emit('foo', 1)이 호출되면, foo에 해당하는 유효성 검사기는 인자 1을 받는다.
      유효성 검사 함수는 이벤트 인자의 유효성을 불리언으로 반환해야 한다.

      emits 옵션은 기본 DOM 이벤트 리스너가 아닌 컴포넌트 이벤트 리스너로 간주되는 이벤트 리스너에 영향을 미친다.
      emits에 선언된 이벤트의 리스너는 컴포넌트의 $attrs 객체에서 제거되므로, 컴포넌트의 루트 엘리먼트로 전달되지 않는다.
      자세한 내용은 폴스루 속성 참고

    예제

      배열 문법

        export default {
          emits: ['check'],
          created() {
            this.$emit('check')
          }
        }

      객체 문법

        export default {
          emits: {
            // 유효성 검사 없음
            click: null,

            // 유효성 검사 사용
            submit: (payload) => {
              if (payload.email && payload.password) {
                return true
              } else {
                console.warn(`잘못된 정보 제출 이벤트!`)
                return false
              }
            }
          }
        }

    emits는 컴포넌트가 발생시키는 이벤트를 정의하는 옵션이다.
    이를 통해 부모 컴포넌트에서 리스닝할 수 있는 이벤트 목록을 명시적으로 지정할 수 있다.

    emits 옵션을 사용하면 컴포넌트가 어떤 이벤트를 발생시키는지 명확하게 알 수 있으며, 타입 검사와 같은 도구들이 올바른 사용을 돕게 된다.
    또한 코드 읽기와 유지보수가 훨씬 쉬워진다.

    emits 옵션은 배열이나 객체 또는 함수의 형태로 제공될 수 있다.

    배열 형태: 각 요소는 해당 컴포넌트에서 발생시킬 수 있는 이벤트의 이름을 나타낸다.
    부모 컴포넌트에서 이러한 이벤트를 리스닝할 수 있다.

      const app = Vue.createApp({
        emits: ['update:modelValue', 'input'], // update:modelValue와 input 이벤트를 발생시킬 수 있음
        // ...
      });

    객체 형태: 키는 이벤트 이름이고 값은 해당 이벤트를 발생시키는 함수이다. 함수는 옵셔널이며, 사용하면 해당 이벤트의 처리를 커스터마이징 할 수 있다.

      const app = Vue.createApp({
        emits: {
          'update:modelValue': (newValue) => {
            // 이벤트 발생시 처리할 로직
            console.log('update:modelValue event emitted with value:', newValue);
            return true; // 이벤트를 발생시키기 위해 true를 반환해야 함
          },
          'customEvent': () => {
            // 이벤트 발생시 처리할 로직
            console.log('customEvent emitted');
            return false; // false를 반환하면 이벤트가 발생하지 않음
          }
        },
        // ...
      });

    emits 옵션을 사용하면 컴포넌트의 인터페이스를 명확히 정의할 수 있으며, 이는 컴포넌트를 사용하는 개발자가 컴포넌트와의 상호작용을 이해하고 쉽게 사용할 수 있도록 도와준다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  expose

    부모 컴포넌트 인스턴스에서 템플릿 참조로 접근할 때, 노출될 공용 속성을 선언한다.

    타입

      interface ComponentOptions {
        expose?: string[]
      }

    세부 정보

      기본적으로 컴포넌트 인스턴스는 $parent, $root 또는 템플릿 참조($refs)를 통해 접근할 때, 모든 인스턴스 속성을 부모에게 노출한다.
      자식 컴포넌트에는 긴밀한 결합을 피하기 위해 비공개로 유지되어야 하는 상태 또는 메서드가 있을 가능성이 높기 때문에, 이것은 바람직하지 않을 수 있다.

      expose 옵션은 속성의 이름 문자열로 이루어진 배열이다. expose를 사용하면 명시적으로 나열된 속성만 컴포넌트의 공개 인스턴스에 노출된다.

      expose는 커스텀 속성에만 영향을 미치며, 빌트인 컴포넌트 인스턴스 속성은 걸러내지 않는다.

    예제

      export default {
        // 공개 인스턴스에서는 `publicMethod`만 접근 가능
        expose: ['publicMethod'],
        methods: {
          publicMethod() {
            // ...
          },
          privateMethod() {
            // ...
          }
        }
      }

    expose는 컴포넌트에서 부모 컴포넌트로 함수나 데이터를 노출하는 기능이다. 이것은 Composition API expose와 약간 다르다.

    Options API에서 expose를 사용하면 컴포넌트의 일부를 부모 컴포넌트로 노출할 수 있다.
    이것은 주로 레거시 코드와의 호환성을 유지하기 위해 사용되며, Composition API보다는 덜 추전된다.

    expose를 사용하려면 컴포넌트의 setup 메서드 내부에서 expose 객체를 반환해야 한다.
    expose 객체에는 노출하련느 함수나 데이터가 포함된다. 이러한 노출된 함수나 데이터는 부모 컴포넌트에서 직접적으로 엑세스할 수 있다.

    예를 들어, 다음은 Options API에서 expose를 사용하여 컴포넌트에서 함수를 노출하는 방법을 보여준다.

      export default {
        data() {
          return {
            count: 0,
          };
        },
        methods: {
          increment() {
            this.count++;
          },
        },
        expose: ['increment'], // increment 함수를 노출합니다.
      };

    이제 이 컴포넌트를 사용하는 부모 컴포넌트에서 increment 함수에 직접 엑세스할 수 있다.

      <template>
        <child-component ref="child"></child-component>
        <button @click="handleIncrement">Increment</button>
      </template>

      <script>
      import ChildComponent from './ChildComponent.vue';

      export default {
        components: {
          ChildComponent,
        },
        methods: {
          handleIncrement() {
            this.$refs.child.increment(); // ChildComponent의 increment 함수 호출
          },
        },
      };
      </script>

    이와 같이 expose를 사용하면 Options API에서 컴포넌트의 일부를 외부에 노출시켜 부모 컴포넌트에서 엑세스할 수 있다.

    




-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
