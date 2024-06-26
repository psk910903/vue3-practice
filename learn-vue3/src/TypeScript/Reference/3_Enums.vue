<template>
	<div></div>
</template>
<!--  

  Enums

    열거형은 TypeSciprt가 제공하는 기능 중 하나이다.

    열거형으로 이름이 있는 상수들의 집합을 정의할 수 있다. 열거형을 사용하면 의도를 문서화 하거나 구분되는 사례 집합을 더 쉽게 만들 수 있다.
    TypeScirpt는 숫자와 문자열-기반 열거형을 제공한다.


    숫자 열거형(Numeric enums)

      열거형은 enum 키워드를 사용해 정의할 수 있다.

        enum Direction {
          Up = 1,
          Down,
          Left,
          Right,
        }

      위 코드에서 Up이 1로 초기화된 숫자 열거형을 선언했다.
      그 지점부터 뒤따르는 멤버들은 자동으로-증가된 값을 갖는다.
      즉, Direction.Up은 1, Down은 2, Left는 3, Right는 4를 값으로 가진다.

      원한다면, 전부 초기화 하지 않을 수도 있다.

        enum Direction {
          Up,
          Down,
          Left,
          Right,
        }

      위 경우 Up 값은 0, Down은 1이 된다. 자동 증가하는 기능은 멤버 값 자체에는 신경 쓰지 않지만, 각 값이 같은 열거형의 다른 값과 구별돼야 하는 경우에 유용하다.

      열거형을 사용하는 것은 간단하다. 그냥 열거형 자체에서 프로퍼티로 모든 멤버에 접근하며, 열거형의 이름을 사용해 타입을 선언한다.

        enum UserResponse {
          No = 0,
          Yes = 1,
        }

        function respond(recipient: string, message: UserResponse): void {
          // ...
        }

        respond("Princess Caroline", UserResponse.Yes);

      숫자 열거형은 계산된 멤버와 상수 멤버를 섞어서 사용할 수 있다.
      간단히 말해, 초기화되지 않은 열거형이 먼저 나오거나, 숫자 상수 혹은 다른 상수 열거형 멤버와 함께 초기화된 숫자 열거형 이후에 와야 한다.
      즉 아래 방식은 허용되지 않는다.

        enum E {
          A = getSomeValue(),
          B, // 오류! 앞에 나온 A가 계산된 멤버이므로 초기화가 필요하다.
        Enum member must have initailizer.
        }


    문자열 열거형(String enums)

      문자열 열거형은 유사항 개념이지만 아래 설명된 것과 같이 런타임에서 열거형의 동작이 약간 다르다.
      문자열 열거형에서 각 멤버들은 문자열 리터럴 또는 다른 문자열 열거형의 멤버로 상수 초기화 해야 한다.

        enum Direction {
          Up = "UD",
          Down = "DOWN",
          Left = "LEFT",
          Right = "RIGHT",
        }

      문자열 열거형은 숫자 열거형처럼 자동 증가하는 기능은 없지만 "직렬화"를 잘한다는 이점이 있다.
      다시 말해서 만약 숫자 열거형을 이용해서 디버깅하고 있고 그 값을 읽어야 한다면, 종종 그 값이 불확실한 경우가 있다.
      숫자만으로는 이것이 어떤 의미인지 유의미한 정보를 제공해주지 않기 때문이다.
      반면 문자열 열거형을 이용하면 코드를 실행할 때, 열거형 멤버에 지정된 이름과는 무관하게 유의미하고 읽기 좋은 값을 이용하여 실행할 수 있다.


    이종 열거형(Heterogeneous enums)

      기술적으로 열거형은 숫자와 문자를 섞어서 사용할 수 있지만 굳이 그렇게 할 이유는 없다.

        enum BooleanLikeHeterogeneousEnum {
          No = 0,
          Yes = "YES",
        }

      확실하게 JavaScript 런타임에서 장점을 취하려는 것이 아니라면, 이렇게 사용하지 않는 것을 권장한다.


    계간된 멤버와 상수 멤버(Computed and constant members)

      각 열거형의 멤버는 상수이거나 계산된 값일 수 있다. 열거형의 멤버는 아래의 경우 상수로 간주한다.

      - 열거형의 첫 번째 데이터이며 초기화 값이 없는 경우, 0으로 값이 할당된다.

        // E.X는 상수이다.
        enum E {
          X,
        }

      - 초기화 값이 없으며 숫자 상수로 초기화된 열거형 멤버 뒤에 따라 나오는 경우. 앞에 나온 상수 값에 1씩 증가한 값을 상수로 갖는다.

        // E1과 E2의 모든 열거형 멤버는 상수이다.
        enum E1 {
          X,
          Y,
          Z,
        }

        enum E2 {
          A,
          B,
          C,
        }

      - 열거형 멤버는 상수 열거형 표현식으로 초기화된다. 상수 열거형 표현식은 컴파일 시 알아낼 수 있는 TypeScript 표현식의 일부이다.
        아래의 경우 상수 열거형 표현식이라고 한다.

        1. 리터럴 열거형 표현식(기본적으로 문자 리터럴 또는 숫자 리터럴)
        2. 이전에 정의된 다른 상수 열거형 멤버에 대한 참조(다른 열거형에서 시작될 수 있음)
        3. 괄호로 묶인 상수 열거형 표현식
        4. 상수 열거형 표현식에 단항 연산자 +, -, ~를 사용한 경우
        5. 상수 열거형 표현식을 이중 연산자 +, -, *, /, %, <<, >>, >>>, &, |, ^ 의 피연산자로 사용할 경우

      상수 열거형 표현식 값이 NaN이거나 Infinity 이면 컴파일 시점에 오류가 발생한다.

      이외 모든 경우 열거형 멤버는 계산된 것으로 간주한다.

        enum FileAccess {
          // 상수 멤버
          None,
          Read = 1 << 1,
          Write = 1 << 2,
          ReadWrite = Read | Write,
          // 계산된 멤버
          G = "123".length,
        }


    유니언 열거형과 열거형 멤버 타입(Union enums and enum member types)

      계산되지 않는 상수 열거 멤버의 특수한 부분 집합이 있다: 리터럴 열거형 멤버는 초기화 값이 존재하지 않거나, 아래 값들로 초기화되는 멤버이다.

      - 문자 리터럴 (예시. "foo", "bar", "baz")
      - 숫자 리터럴 (에시. 1, 100)
      - 숫지 리터럴에 단항 연산자 - 가 적용된 경우 (e.g. -1, -100)

      열거형의 모든 멤버가 리터럴 열거형 값을 가지면 특별한 의미로 쓰이게 된다.

      첫째로 열거형 멤버를 타입처럼 사용할 수 이 있다. 예를 들어 멤버는 오직 열거형 멤버의 값만 가지게 할 수 있다.

        enum ShapeKind {
          Circle,
          Square,
        }

        interfce Circle {
          kind: ShapeKind.Circle;
          radius: number;
        }

        interface Square {
          kind: ShapeKind.Square;
          sideLength: number;
        }

        let c: Circle = {
          kind.ShapeKind.Square, // 오류! 'ShapeKind.Circle' 타입에 'ShapeKind.Square' 타입을 할당할 수 없다.
        Type 'ShapeKind.Square' is not assignable to type 'ShapeKind.Circle'.
          radius: 100,
        };

      또 다른 점은 열거형 타입 자체가 효율적으로 각각의 열거형 멤버의 유니언이 된다는 점이다. 
      유니언 타입 열거형을 사용하면 타입 시스템이 열거형 자체에 존재하는 정확한 값의 집합을 알고 있다는 사실을 활용할 수 있다는 점만 알아두면 된다.
      이 때문에 TypeScript는 값을 잘못 비교하는 어리석은 버그를 잡을 수 있다. 예를 들어,

        Enum E {
          Foo,
          Bar,
        }

        function f(x: E) {
          if (x !== E.Foo) || x !== E.bar {
        This comparison appears to be unintentional because the types 'E.Foo' and 'E.Bar' have no overlap.
            // 에러! E 타입은 Foo, Bar 둘 중 하나이기 때문에 이 조건은 항상 true를 반환한다.
          }
        }

      이 예제에서 x가 E.Foo가 아닌지 확인한다. 만약 이 조건이 true 라면, || 조건은 더는 체크할 필요가 없으므로 if 아래의 body가 실행될 것이다.
      그러나 만약 이조건이 통과되지 않는다면, x는 반드시 E.Foo이기 때문에, x가 E.Bar가 아닌지 묻는 조건과 비교하는 것은 적절하지 않다.


    런타임에서의 열거형(Enums at runtime)

      열거형은 런타임에 존재하는 실제 객체이다. 예를 들어 아래와 같은 열거형은
      
        enum E {
          X,
          Y,
          Z,
        }

      실제로 아래와 같이 함수로 전달될 수 있다.

        enum E {
          X,
          Y,
          Z,
        }

        function f (obj: {X: number}) {
          return obj.X;
        }

        // E가 X라는 숫자 프로퍼티를 가지고 있기 때문에 동작하는 코드이다.
        f(E);


    컴파일 시점에서 열거형(Enums at compile time)

      열거형이 런타임에 실제 객체라고 할지라도, keyof 키워드는 일반적인 객체에서 기대하는 동작과 다르게 동작한다.
      대신, keyof typeof를 사용하면 모든 열거형의 키를 문자열로 나타내는 타입을 가져온다.

        enum LogLevel {
          ERROR,
          WARN,
          INFO,
          DEBUG,
        }

        // 이것은 아래와 동일하다.
        // type LogLevelStrings = 'ERROR' | 'WRAN' | 'INFO' | 'DEBUG';
        type LogLevelStrings = keyof typeof LogLevel;

        function printImportant(key: LogLevelStrings, message: string) {
          const num = LogLevel[key];
          if (num <= LogLevel.WARN) {
            console.log("Log level key is", key)
            console.log("Log level value is", num)
            console.log("Log level message is", message)
          }
        }
        printImportant("ERROR", "This is a message");


      역 매핑(Reverse mapplin)

        숫자 열거형 멤버는 멤버의 프로퍼티 이름을 가진 객체를 생성하는 것 외에도 열거형 값에서 열거형 이름으로 역 매핑을 받는다.

          enum Enum {
            A,
          }

          let a = Enum.A;
          let nameOfA = Enum[a]; // "A"

        TypeScript는 아래와 같은 JavaScript 코드로 컴파일할 것이다.

          "use strict";
          var Enum;
          (function (Enum) {
            Enum[Enum["A"] = 0] = "A";
          })(Enum | Enum {}));
          let a = Enum.A;
          let nameOfA = Enum[a]; // "A"

        이렇게 생성된 코드에서, 열거형은 정방향 (name -> value) 매핑과 역방향 (value -> name) 매핑 두 정보를 모두 저장하는 객체로 컴파일된다.
        다른 열거형 멤버 참조는 항상 프로퍼티 접근으로 노출되며 인라인되지 않는다.

        문자열 열거형은 역 매핑을 생성하지 않는다는 것을 명심해야 한다.


      const 열거형 (const enums)

        대부분의 경우, 열거형은 완벽하게 유효한 해결책이다. 하지만 종종 열거형의 요구사항이 좀 더 엄격해 진다.
        열거형 값에 접근할 때, 추가로 생성된 코드 및 추가적인 간접 참조에 대한 비용을 피하기 위해 const 열거형을 사용할 수 있다.
        const 열거형은 const 지정자를 열거형에 붙여 정의한다.

          const enum Enum {
            A = 1,
            B = A * 2,
          }

        const 열거형은 상수 열거형 표현식만 사용될 수 있으며 일반적인 열거형과 달리 컴파일 과정에서 완전히 제거된다.
        const 열거형은 사용하는 공간에 인라인된다.
        이러한 동작은 const 열거형이 계산된 멤버를 가지고 있지 않기 때문에 가능하다.

          const enum Direction {
            Up,
            Down,
            Left,
            Right,
          }

          let directions = {
            Direction.Up,
            Direction.Down,
            Direction.Left,
            Direction.Right,
          };

        위 코드는 아래와 같이 컴파일된다.

          "use strict";
          let directions = [
            0, // Direction.Up,
            1, // Direction.Down,
            2, // Direction.Left,
            3, // Direction.Right,
          ]


    Ambient 열거형 (Ambient enums)

      Ambient 열거형은 이미 존재하는 열거형 타입의 모습을 묘사하기 위해 사용된다.

        declare enum Enum {
          A = 1,
          B,
          C = 2,
        }

      ambient 열거형과 비-ambient 열거형의 가장 큰 차이점은, 일반적인 열거형에서 초기화 되지 않은 멤버가 상수로 간주하는 멤버 뒤에 있다면, 
      이 멤버도 상수로 간주할 것이다. 반면 (const가 아닌) ambient 열거형에서 초기화되지 않은 멤버는 항상 계산된 멤버로 간주한다.


    
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
