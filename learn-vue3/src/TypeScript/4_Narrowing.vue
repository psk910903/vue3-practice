<template>
	<div></div>
</template>
<!--  

  Narrowing (좁히다)

    padLeft라는 함수가 있다고 가정해보자.

      function padLeft(padding: number | string, input: string): string {
        throw new Error("Not implemented yet!");
      }

    만약 padding이 숫자인 경우, 그것을 input의 앞에 추가하고자 하는 공백의 수로 처리한다. 만약 padding이 문자열인 경우, 그냥 padding을 input 앞에 추가해야 한다.
    padLeft가 padding으로 number를 전달받을 때의 로직을 구현해보자.

      function padLeft(padding: number | string, input: string): string {
        return " ".repeat(padding) + input;

        Argument of type 'string | number' is not assignable to parameter of type 'number'.
          Type 'string' is not assignable to type 'number'
      }

    패딩에 대한 오류가 발생하고 있다. TypeScript가 repeat 함수에 number만을 인자로 받는데 number또는 string 타입의 값을 전달하고 있다고 경고하고 있다.
    다시 말해서, 우리는 먼저 padding이 number인지 여부를 명시적으로 확인하거나, 그것이 string인 경우를 처리하고 있지 않는다.

      function pedLeft(padding: number | string, input: string): string {
        if(typeof padding === 'number') {
          return " ".repeat(padding) + input;
        }
        return padding + input;
      }

    만약 이 코드가 대부분의 경우 재미 없는 JavaScript 코드처럼 보인다면, 그게 바로 그 의도이다. 우리가 추가한 주석을 제외하고 이 TypeScript 코드는 JavaScript와 비슷하다.
    TypeScript의 타입 시스템은 타입 안전성을 얻기 위해 과도하게 고생하지 않고도 전형적인 JavaScript 코드를 가능한 쉽게 작성할 수 있도록 설계되었다.

    이 코드가 그다지 복잡해 보이지 않을지 모르지만, 실제로는 많은 일이 발생한다. 
    TypeScript가 정적 타입을 사용하여 런타임 값을 분석하는 방식과 유사하게 if/else, 조건부 삼항 연산자, 반복문, true/false 검사 등과 같은 자바스크립트의 런타임 제어 흐름 구조 위에 타입 분석을 오버레이한다.
    이러한 구조들은 모두 그 타입에 영향을 미칠 수 있다.

    우리는 if문 안에서, TypeScript는 typeof padding === "number"를 보고 이를 타입 가드(type guard)라고 알아낸다.
    TypeScript는 프로그램이 취할 수 있는 가능한 실행 경로를 따라가서 주어진 위치에서 값의 가장 구체적인 타입을 분석한다.
    이러한 특별한 체크(타입 가드라고 불리는)와 할당을 살펴보고, 선언된 타입보다 더 구체적인 타입으로 타입을 좁히는 과정을 좁히기(narrowing)라고 한다.
    많은 편집기에서 이러한 타입이 변화하는 것을 관찰할 수 있으며, 우리의 예시에서도 이를 확인할 수 있다.

      function padLeft(padding: number | string, input: string): string {
        if(typeof padding === "number") {
          return " ".repeat(padding) + input;

          (parameter) padding: number
        }
        return padding + input;
        
          (parameter) apdding: string
      }

    TypeScript 이해하는 타입을 좁히는 데에는 몇 가지 다른 구조가 있다.


  typeof 타입 가드들

    우리가 보았던 것처럼, JavaScript는 런타임 시점에서 값의 유형에 대한 매우 기본적인 정보를 제공하는 typeof 연산자를 지원한다. 
    TypeScript는 이 연산자가 특정한 문자열 집합을 반환할 것으로 예상한다.

    - string
    - number
    - bigint
    - boolean
    - symbol
    - undefined
    - object
    - function

    우리가 padLeft에서 보았던 것처럼, 이 연산자는 여러 JavaScript 라이브러리에서 자주 사용되며, TypeScript는 이를 이해하여 다양한 분기에서 타입을 좁힐 수 있다.

    TypeScript에서는 typeof 연산자로 반환된 값에 대한 확인이 타입 가드로 작용한다. 
    TypeScript는 typeof가 JavaScript에서 다양한 값에 어떻게 작용하는지를 알고 있기 때문에, 예를 들어 위에서 본 것처럼 typeof는 "null"이라는 문자열을 반환하지 않는다는 것을 알고 있다.
    아래 예제를 통해 이를 확인해보자.

      function printAll(strs: string | string[] | null) {
        if(typeof strs === "object") {
          for( const s of strs) {
            'strs' is possibly 'null'.
            console.log(s);
          }
        } else if (typeof strs === "string") {
          console.log(strs);
        } else {
          // do nothing
        }
      }

    printAll 함수에서는 strs가 객체인지 확인하여 배열 유형인지 확인하려고 한다. JavaScript에서 배열은 기술적으로 객체 유형이다. 그러나 여기서 특이한 점이 있다.
    typeof null은 실제로 "object"를 반환한다. 이러한 이상한 점은 JavaScript의 역사에서 비롯된 것 중 하나이다.

    경험이 충분한 사용자들은 놀라지 않을 수 있지만, 모든 사람이 JavaScript에서 이것을 경험한 것은 아닐 것이다.
    다행히도 TypeScript를 사용하면 strs가 stringp[] | null로 좁혔다는 것을 알려주므로 단순히 string[]이 아닌 것을 알 수 있다.

    여기서 "진실성(truthiness)"확인으로 넘어가는 것이 좋은 전환점일 것이다.


  Truthiness narrowing (진실성 좁힘)

    "진실성(truthiness)"이라는 단어는 사전에서 찾을 수 없을지 모르지만, JavaScript에서 자주 들을 수 있는 개념이다.

    JavaScript에서는 조건문, 논리연산자(&&, ||), if문, 불리언 부전(!) 등에서 어떤 표현식이든 사용할 수 있다.
    예를 들어, if문은 조건이 항상 boolean 타입을 가질 것으로 예상하지 않는다.

      function getUserOnlineMessage(numUsersOnline: number) {
        if (numUsersOnline) {
          return `There are ${numUsersOnline} onlin now!`;
        }
        return "Nobody's here"
      }

    JavaScript에서는 if와 같은 구조가 먼저 조건을 boolean 값으로 "강제 전환(coerce)"하여 의미를 파악한 다음, 결과가 true 또는 false인지에 따라 분기를 선택한다.
    같은 예제에서

    - 0
    - NaN
    - "" (the empty string)
    - 0n (the bigint version of zero)
    - null
    - undefined

    JavaScript에서는 모든 값이 false로 강제 변환되며, 다른 값은 true로 강제 변환된다. 값을 항상 boolean으로 강제 변환하려면 Boolean 함수를 사용하거나 짧은 이중 boolean 부정을 사용할 수 있다.
    (후자는 TypeScript가 좁은 리터럴 boolean 타입 true을 유추하는 반면, 전자는 타입을 boolean으로 유추한다.)

      // both of these result in 'true'
      Boolean("hello"); // type: boolean, value: true
      !!"world"; // type: true, value: true

    이러한 동작을 활용하는 것이 상당히 인기가 있으며, 특히 null 또는 undefined와 같은 값에 대한 보호를 위해 사용된다.
    예를 들어, printAll 함수에 대해 이를 사용해 보자.

      function printAll(strs: string | string[] | null) {
        if (strs && typeof strs === "object") {
          for (const s of strs) {
            console.log(s)
          }
        } else if (typeof strs === "string") {
          console.log(strs);
        }
      }

    위의 예에서는 strs가 truthy한지 확인함으로써 오류를 제거했다. 이렇게 하면 적어도 코드를 실행할 때 아래와 같은 골치 아픈 오류를 방지할 수 있다.

      TypeError: null is not iterable

    그러나 기본(primitives)에 대한 진실성 확인은 종종 오류를 일으킬 수 있음을 염두에 두어야 한다.
    예를 들어, printAll을 다시 작성하는 또 다른 시도를 고려해보자.

      function printAll(strs: string | string[] | null) {
        // !!!!!!!!!!!!!!!
        // DON'T DO THIS!
        //  KEEP READING
        // !!!!!!!!!!!!!!!
        if (strs) {
          if (typeof strs === "object") {
            for (const s of strs) {
              console.log(s);
            }
          } else if (typeof strs === "string") {
            console.log(strs);
          }
        }
      }

    우리는 함수의 전체 몸통을 진실성 확인으로 감쌌지만, 이것에는 미묘한 단점이 있다. 우리는 더 이상 빈 문자열 케이스를 올바르게 처리하지 못할 수 있다.

    TypeScript는 여기서 전혀 문제가 되지 않지만, JavaScript에 대해 덜 익숙한 경우 이 동작은 유의해야 한다. 
    TypeScript는 종종 초기에 버그를 잡아주지만, 값에 아무것도 하지 않기로 선택한 경우 너무 많은 규칙을 적용하지 않고는 할 수 있는 것이 그렇게 많지 않다.
    원한다면 린터(linter)를 사용하여 이러한 상황을 처리하는지 확인할 수 있다.

    진실성에 의한 좁힘에 대한 마지막은 boolean 부정(!)이 부정된 분기에서 제외되는 것이다.

      function multiplyAll(
        values: number[] | undefined,
        factor: number
      ) : number[] | undefined {
        if (!values) {
          return values;
        } else {
          return values.map((x) => x * factor);
        }
      }


  Equality narrowing(동등성 좁힙)

    TypeSCript는 switch문과 ===, !==, ==, !=와 같은 동등성 검사를 사용하여 유형을 좁힌다.
    예를 들어,

      function example(x: string | number, y: string | boolean) {
        if (x === y) {
          // 이제 x 또는 y에서 모든 string 메서드를 호출할 수 있다.

          x.toUpperCase();
          y.toUpperCase();
            (method) String.toUpperCase(): string
        } else {
          consoel.log(x);
            (parameter) x: string | number
          consoel.log(y);
            (parameter) y: string | boolean
        }
      } 

    위의 예에서 x와 y가 모두 같음을 확인했을 때, TypeScript는 그들의 유형도 같아야 한다는 것을 알 수 있다.
    x와 y가 취할 수 있는 유일한 공통 유형이 문자열이기 때문에, TypeScript는 첫 번째 분기에서 x와 y가 반드시 문자열이어야 한다는 것을 알고 있다.

    특정 리터럴 값에 대한 확인(변수가 아닌 경우)도 작동한다. 진실성 좁힘에 관한 섹션에서 우리는 빈 문자열을 제대로 처리하지 않아서 오류가 발생할 수 있는 printAll 함수를 작성했다.
    대신에 특정 체크를 통해 null을 차단할 수 있었으며, TypeScript는 여전히 strs의 타입에서 null을 제거했다.

      function printAll(strs: string | string[] | null) {
        if (strs !== null) {
          if (typeof strs === "object") {
            for (const s of strs) {
                (parameter) strs: string[]
              console.log(s);
            }
          } else if (typeof strs === "string") {
            console.log(strs);
              (parameter) strs: string
          }
        }
      }

    JavaScript의 느슨한 동등성 검사인 ==와 !=도 올바르게 좁혀진다. 익숙하지 않다면, 어떤 것이 null 인지를 확인하는 == null은 실제로 해당 값이 명시적으로 null인지를 확인하는 것뿐만 아니라, 잠재적으로 undefined인지도 확인한다.
    == undefined도 마찬가지이다. 이는 값이 null 이거나 undefined인지를 확인한다.

      interface Container {
        value: number | null | undefined;
      }

      function multiplyValue(container: Container, factor: number) {
        // 유형에서 null과 undefined를 모두 제거한다.
        if (container.value != null) {
          console.log(container.value);

            (property) Container.value: number

          // 이제 'container.value'를 안전하게 곱할 수 있다.
          container.value *= factor;
        }
      }

  in 연산자 좁히기

    JavaScript에는 객체나 그 프로토 타입 체인에 특정한 이름의 속성이 있는지 여부를 결정하는 연산자가 있다.
    in 연산자이다. TypeScript는 이를 유형을 좁히는 방법으로 고려한다.

    예를 들어, 코드 "value" in x가 있을 때, 여기서 "value"는 문자열 리터럴이고 x는 유니언 타입이다.
    "true" 분기는 x의 유형을 선택적이거나 필수 속성 값이 있는 유형으로 좁히며, "false"분기는 선택적 이거나 누락된 속성 값이 있는 유형으로 좁힌다.

      type Fish = { swim: () => void };
      type Bird = { fly: () => void };

      function move(animal: Fish | Bird) {
        if ("swim" in animal) {
          return animal.swim();
        }
        return animal.fly();
      }

    다시 강조하면, 선택적 속성은 좁히는 데 양쪽에 모두 존재할 것이다. 예를 들어, 사람은 수영할 수도 있고(적절한 장비를 갖추면) 날아다닐 수도 있으므로 in 확인에서 두 가지 경우에 모두 나타나야 한다.

      type Fish = { swim: () => void };
      type Bird = { fly: () => void };
      type Human = { swim?: () => void; fly?: () => void };

      function move(animal: Fish | Bird | Human) {
        if ("swim" in animal) {
          return animal.swim();

            (parameter) animal: Fish | Human
          }
          return animal.fly();

            (parameter) animal: Bird | Human
        }


  instanceof 좁히기

    JavaScript에는 값이 다른 값의 "instance"인지 여부를 확인하는 연산자가 있다.
    더 구체적으로 말하면, JavaScript에서 x instanceof Foo는 x의 프로토타입 체인에 Foo.prototype이 있는지 확인한다.
    우리는 여기서 깊게 파고들지는 않겠지만, 클래스에 대해 더 알아보면 이에 대해 더 많이 볼 것이다. 이 연산자는 여전히 new로 생성할 수 있는 대부분의 값에 유용할 수 있다.
    아마도 짐작했겠지만, instanceof도 타입 가드이며, TypeScript는 instanceof로 보호된 분기에서 타입을 좁힌다.

      function logValue(x: Date | string) {
        if (x instanceof Date) {
          console.log(x.toUTCString());

            (parameter) x: Date
        } else {
          console.log(x.toUpperCase());

            (parameter) x: string
        }
      }


  과제들

    우리가 이전에 언급한 대로, 변수에 할당할 때 TypeScript는 할당 구문의 오른쪽을 살펴보고 왼쪽을 적절하게 좁힌다.

      let x = Math.random() < 0.5 ? 10 : "hello world";

        let x: string | number

      x = 1;

      console.log(x);

        let x: number

      x = "goodbye";

      console.log(x);

        let x: string


    각각의 할당문이 유효함을 주목하자. 첫 번째 할당 후에 x의 관찰된 타입이 숫자로 변경되었음에도 불구하고, 우리는 여전히 x에 문자열을 할당할 수 있었다.
    이는 x의 선언된 타입 -x가 시작했을 때의 타입 -이 string | number이기 때문에 할당 가능성은 항상 선언된 타입과 비교된다.

    만약 우리가 x에 불리언을 할당했다면, 선언된 타입에 포함되지 않았기 때문에 오류가 발생했을 것이다.

      let x = Math.random() < 0.5 ? 10 : "hello world";

        let x: string | number

      x = 1;

      console.log(x);

        let x: number

      x = true;

      Type 'boolean' is not assignable to type 'string | number'

      console.log(x);

        let x: string | number


  제어 흐름 분석

    지금까지 TypeScript가 특정 분기 내에서 좁혀지는 방식에 대한 몇 가지 기본적인 예제를 살펴보았다. 그러나 모든 변수를 검사하고 if문, while문, 조건문 등에서 타입 가드를 찾는 것 이상의 작업이 진행되고 있다.
    예를 들어,

      function padLeft(padding: number | string, input: string) {
        if (typeof padding === "number") {
          return " ".repeat(padding) + input;
        }
        return psdding + input;
      }

    padLeft 함수는 첫 번째 if 블록 내에서 반환한다. TypeScript는 이 코드를 분석하여 padding이 숫자인 경우에는 남은 부분( return padding + input;)이 도달할 수 없다는 것을 알아냈다.
    결과적으로 TypeScript는 padding의 유형에서 number를 제거할 수 있었고 (string | number에서 string으로 좁혀짐) 함수의 나머지 부분에서는 해당 타입을 사용할 수 있었다.

    이러한 도달성에 기반한 코드 분석을 제어 흐름 분석(controll flow analysis)이라고 하며, TypeScript는 이러한 흐름 분석을 만나면서 타입 가드와 할당을 통해 타입을 좁힌다.
    변수가 분석될 때, 제어 흐름은 여러 번 분할되고 다시 병합될 수 있으며, 해당 변수는 각 지점에서 다른 유형을 가질 수 있다.

      function example() {
        let x: string | number | boolean;

        x = Math.random() < 0.5;

        console.log(x);

          let x: boolean

        if (Math.random() < 0.5) {
          x = "hello";
          console.log(x);

            let x: string
        } else {
          x = 100;
          console.log(x);

            let x: number
        }

        return x;

          let x: string | number
      }


  유형을 사용하여 예측 (Using type predicates)

    지금까지 우리는 코드 내에서 유형을 좁히는 데 기존의 JavaScript 구조를 사용해 왔지만, 때로는 코드 전반에 걸쳐 유형이 어떻게 변경되는지에 대한 직접적인 제어가 필요할 수 있다.

    사용자 정의 타입 가드에 정의하려면 반환 유형이 유형 예측자(type predicate)인 함수를 정의하면 된다.

      function isFish(pet: Fish | Bird): pet is Fish {
        return (pet as Fish).swim !== undefined;
      }

    이 예제에서는 pet is Fish가 우리의 타입 예측자이다. 예측자는 parameterName이 현재 함수 시그니처에서 매개변수의 이름이여야 하는 parameterName is Type 형식을 취한다.

    어떤 변수가 isFish와 함께 호출될 때마다, TypeScript는 원래의 유형이 호환 가능한 경우 해당 변수를 해당 특정 유형으로 좁힌다.

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
