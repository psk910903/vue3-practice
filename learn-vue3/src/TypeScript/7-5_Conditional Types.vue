<template>
	<div></div>
</template>
<!--  

  Conditional Types

    대부분 유용한 프로그램의 핵심은, 입력에 따라 출력이 결정되어야 한다는 것이다.
    JavaScript 프로그램도 크게 다르진 않지만, 값의 타입을 쉽게 검사할 수 있다는 사실을 고려할 때, 출력에 대한 결정은 또한 입력의 타입에도 기반한다.
    조건부 타입은 입력과 출력 타입간의 관계를 설명하는 데 도움을 줄 수 있다.

      interface Animal {
        live(): void;
      }
      interface Dog extends Animal {
        woof(): void;
      }

      type Example1 = Dog extends Animal ? number : string;

        type Example1 = number

      type Example2 = RegExp extends Animal ? number : string;

        type Example2 = string

    조건부 타입은 JavaScript에 있는 삼항 연산자 조건문 (codition ? treuExpression : falseExpression)같은 형태를 가진다.

      SomeType extends OtherType ? TrueType : FalseType;

    extends를 기준으로 왼쪽에 있는 타입이 오른쪽 타입에 할당할 수 있다면 첫 번째 분기("참"값 분기)를, 그렇지 않다면 뒤의 분기("거짓"값 분기)를 얻게 된다.

    Dog extends Animal에 따라 number나 string인지 알려주는 것 말곤, 위의 예제에서 조건부 타입은 그다지 유용해 보이지 않는다.
    하지만 제네릭과 함께 사용될 때 조건부 타입은 강력한 힘을 갖는다.

    예를 들어, 다음 createLabel 함수를 살펴보자

      interface IdLabel {
        id: number // some fields
      }

      interface NameLabel {
        name: string // other fields
      }

      function createLabel(id: number): IdLabel;
      function createLabel(name: string): NameLabel;
      function createLabel(nameOrId: string | number): IdLabel | NameLabel;
      function createLabel(nameOrId: string | number): IdLabel | NameLabel {
        throw "unimplemented";
      }

    createLabel의 오버로드들은 입력 타입에 따른 단일 JavaScript 함수를 나타낸다.

      1.만약 라이브러리가 매번 API 전체에서 비슷한 종류의 함수를 만들어야 한다면 번거로워진다.
      2.우린 3가지 오버로드 즉, 각 케이스별로 확실한 타입을 가지거나 (각각 number와 string) 그리고 일반적인 케이스(string | number)가져야 한다. 
        createLabel의 새로운 타입을 다루기 위해선 오버로드의 수는 기하급수적으로 증가한다.

    대신에 조건부 타입으로 로직을 인코딩할 수 있다.

      type NameOrId<T extends number | string> = T extends number ? IdLabel : NameLabel;

    조건부 타입을 사용하면 단일 함수까지 오버로드 없이 단순화 시킬 수 있다.

      function createLabel<T extends number | string>(idOrName: T): NameOrId<T> {
        throw "unimplemented";
      }

      let a = createLabel("typescript")

        let a: NameLabel

      let b = createLabel(2.8);

        let b: IdLabel

      let c = createLabel(Math.random() ? "hello" : 42);

        let c: NameLabel | IdLael


    조건부 타입으로 제한하기

      종종, 조건부 타입의 검사에서 새로운 정보를 얻을 수 있다.
      타입 가드가 더 구체적인 타입으로 좁혀주듯이, 조건부 타입의 "참"값 분기는 대조하는 타입에 따라서 제네릭을 더 제한할 수 있다.

      다음 예를 살펴보자.

        type Message<T> = T["message"];

        Type '"message"' cannot be used to index type 'T'.

      위 예제에서, T가 message 프로퍼티를 가지고 있는지 알 수 없기 때문에 TypeScript에서 오류가 발생한다. 
      T의 타입을 제한해서 TypeScript가 더 이상 오류를 내지 않도록 만들 수 있다.

        type MesageOf<T extends { message: unknown }> = T["message"];

        interface Email {
          message: string;
        }

        type EmailMessageContents = MessageOf<Email>;

          type EmailMessageContents = string

      하지만 MessageOf가 아무 타입이나 받을 수 있고, message 프로퍼티가 없으면 never 타입으로 결정하도록 만들 수 있을까?
      여기서 제약 조건을 외부로 옮기고, 조건부 타입을 적용하면 가능하다.

        type Message<T> = T extends { message: unknown } ? T["message"] : never;

        interface Email {
          message: string;
        }

        interface Dog {
          bark(): void;
        }

        type EmailMessageContents = MessageOf<Email>;

          type EmailMessageContents = string

        type DogMessageContents = MessageOf<Dog>

          type DogMessageContents = never

      "참"값 분기내에서는 TypeScript는 T가 message 프로퍼티를 가지고 있을 것을 알 수 있다.

      또 다른 예제에서 배열 타입이면 배열의 개별 요소 타입으로 평탄화 시키지만, 배열 타입이 아니면 그대로 유지하는 Flatten 타입을 만들 수 있다.

        type Faltten<T> = T extends any[] ? T[number] : T;

        // Extracts out the element type.
        type Str = Flatten<string[]>;

          type Str = string

        // Leaves the type alone.
        type Num = Flatten<number>;

          type Num = number

      Flatten에 배열 타입이 주어지면, number를 사용한 인덱스 접근을 통해 string[]의 요소 타입을 가져올 수 있다.
      그렇지 않으면, 주어진 타입을 반환한다.


    조건부 타입 내에서 추론하기

      위에서 제약 조건을 가진 조건부 타입을 이용해서 타입을 추출할 수 있다는 점을 살펴봤다.
      이 부분은 조건부 타입을 더 쉽게 만드는 평범한 작업이 된다.

      조건부 타입은 infer 키워드를 사용해서 "참"값 분기에서 비교하는 타입을 추론할 수 있다.
      예를 들어, Flatten에서 인덱싱된 접근 타입으로 "직접" 추출하지 않고 요소 타입을 추론할 수 있다.

        type Flatten<Type> = Type extends Array<infer item> ? Item : Type;

      여기 "참"값 분기에서 T의 요소 타입을 어떻게 제시할 필요 없이, infer 키워드를 새 제네릭 타입 변수 Item에 선언적으로 사용했다.
      이 방식은 관심 있는 타입의 구조를 깊게 분석하지 않아도 되도록 만들어준다.

      inter 키워드를 사용해서 유용한 헬퍼 타입 별칭을 사용할 수 있다.
      예를 들어 함수 타입에서 리턴 타입을 추출하는 간단한 케이스를 살펴보자.

        type GetRetrunType<Type> = Type extends (...args: never[]) => infer Retrun ? Retrun : never;

        type Num = GetRetrunType<() => number>;

          type Num = number

        type Str = GetRetrunType<(x: string) => string>;

          type Str = string

        type Bools = GetRetrunType<(a: boolean, b: boolean) => boolean[]>;

          type Bools = boolean[]

      여러 호출 시그니처 (오버로드 함수 타입 길이)를 가진 타입을 추론할 때, 마지막 시그니처(아마 모든 케이스에 허용되는)로 추론하게 된다.
      인자 타입의 목록에 기반해서 오버로드들을 처리할 수는 없다.

        declare function stringOrNum(x: stirng): number;
        declare function stringOrNum(x: number): string;
        declare function stringOrNum(x: string | number): string | number;

        type T1 = ReturnType<typeof stringOrNum;

          type T1 = string | number


  분산적인 조건부 타입

    제네릭 타입 위에서 조건부 타입은 유니언 타입을 만나면 분산적으로 동작한다. 예를 들어 다음을 보자.

      type ToArray<Type> = Type extends any ? Type[] : never;

    ToArray에 유니언 타입을 넘기면 조건부 타입은 유니언의 각 멤버에 적용된다.

      type ToArray<Type> = Type extends any ? Type[] : never;

      type StrArrOrNumArr = ToArray<string | number>;

        type StrArrOrNumArr = string[] | number[]

    StrArrOrNumArr이 동작하는 방식은 다음과 같다.

      string | number;

    유니언의 각 멤버 타입은 효율적으로 매핑된다.

      ToArray<string> | ToArray<number>;

    그리고 다음과 같이 결과가 나온다.

      string[] | number[];

    일반적으로 분산성이 원하는 동작이다. 이러한 동작을 방지하려면 extends 키워드의 양 옆을 대괄호로 감싸면 된다.

      type ToArrayNonDist<Type> = [Type] extends [any] ? type[] : never;

      // 'StrArrOrNumArr' is no longer a union.
      type StrArrOrNumArr = ToArrayNonDist<string | number>;

        type StrArrOrNumArr = (string | number)[]


    TypeScript에서 조건부 타입은 입력된 타입에 따라 타입을 결정할 수 있도록 해주는 고급 타입 시스템의 기능이다.
    조건부 타입을 사용하면 제네릭 타입과 같이 유연한 타입을 만들 수 있으며, 특정 조건에 따라 다른 타입을 반환하도록 설계할 수 있다.
    이는 프로그램의 타입 안전성을 높이고 복잡한 상황에서의 타입 관리를 용이하게 한다.


    조건부 타입의 기본 형태

      조건부 타입은 아래와 같은 형태를 갖는다.

        T extends U ? X : Y

      여기서,

      - T와 U는 타입니다.
      - T extends U는 타입의 확장을 의미하는데 T 타입이 U 타입에 할당 가능한지를 검사한다.
      - ? 는 조건 연산자로 T extends U의 결과가 참이면 X 타입을, 거짓이면 Y 타입을 결과로 반환한다.


    조건부 타입의 예시

      조건부 타입의 간단한 예시는 다음과 같다.

        type Check<T> = T extends string ? "Yes" : "No";

      위 조건부 타입 Check는 제네릭 타입 T가 string에 할당 가능한 경우 Yes를 반환하고, 그렇지 않은 경우 No를 반환한다.

        type Type1 = Check<string>; // Yes
        type Type2 = Check<number>; // No


    복잡한 조건부 타입

      조건부 타입은 더 복잡한 로직에도 사용될 수 있다. 예를 들어, 두 타입 중 하나를 선택하는 유틸리티 타입을 만들 수 있다.

        type Either<T, U> = T extends U ? T : U;

      또는 여러 조건을 체이닝해서 다양한 타입을 결합할 수 있다.

        type Message<T> = T extends { message: unknown } ? T["message"] : never;


    조건부 타입의 실제 활용

      조건부 타입은 라이브러리나 프레임워크 내에서 API의 타입을 다룰 때 매우 유용하다.
      예를 들어, Props 타입에 따라 다른 Props 타입을 반환하거나, 함수 오버로딩을 타입 레벨에서 처리할 때 조건부 타입이 사용된다.


    결론

      타입스크립트의 조건부 타입은 프로그램의 타입 정확성을 향상시키는 데 크게 기여한다.
      복잡한 조건을 타입 수준에서 처리할 수 있게 하며, 제네릭과 함께 강력한 타입 설계를 가능하게 한다.
      따라서 타입스크립트를 사용하는 개발자에게는 조건부 타입을 이해하고 활용하는 것이 중요하다.


    예제: Props에 따른 조건부 타입

      Vue 컴포넌트에서 props를 받을 때 props의 타입에 따라 컴포넌트의 동작이나 반환 타입이 달라질 수 있다.
      예를 들어, 특정 prop에 따라 다른 타입의 데이터를 처리하는 함수를 만들 수 있다.

        // Props 타입 정의
        interface Props {
          text: 'text' | 'number';
          value: string | number;
        }

        // 조건부 타입을 사용하여 Prop 'value'의 타입을 기반으로 반환 타입 결정
        function processValue(props: Props): string | number {
          return props.type === 'text' ? props.valeu.toSTring() : +props.value;
        }

        const propsText: Props = { type: 'text, value: 'hello' };
        const resultText = processValue(propsText); //string

        const propsNumber: Props = { type: 'number', value: 123 };
        const resultNumber = processValue(propsNumber); // number


      예제: 조건부 타입과 Vue Component API

        Vue 3의 Composition API와 조건부 타입을 결합하여, 입력 타입에 따라 다른 컴포지션 함수의 로직을 실행할 수 있다.

          import { ref, Ref, computed, ComputedRef } from 'vue'

          interface User {
            name: string;
            age: number;
          }

          // showFull이 ture면 전체 사용자 정보를, false면 이름만 반환하는 함수
          function useUserInfo(user: Ref<User>, showFull: boolean): ComputedRef<string> | ComputedRef<User> {
            return showFull ? computed(() => user.value) : computed(() => user.value.name)
          }

          const user = ref<User>({ name: 'Jane', age: 30 });
          const userInfo = useUserInfo(user, false); // ComputedRef<string>, 사용자의 이름만 반환

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
