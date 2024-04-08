<template>
	<div></div>
</template>
<!--  

  More on Function

    지역 함수이건, 다른 모듈에서 불러온 함수이건, 어떤 클래스의 메서드이건, 함수는 어느 어플리케이션에서도 기초적인 구성 요소의 역할을 한다.
    함수는 값이다. 그리고 다른 값처럼, TypeScript에서는 함수들이 호출될 수 있는 방법을 서술하는 방법이 많이 있다. 함수를 설명하는 타입들을 작성하는 방법들을 알아보자.


    함수 타입 표현식

      함수를 설명하는 가장 간단한 방법은 함수 타입 표현식이다. 이 타입은 화살표 함수와 문법적으로 유사하다.

        function greeter(fn: (a: string) => void) {
          fn("Hello, World");
        }

        function printToConsole(s: string) {
          console.log(s);
        }

        greeter(printToConsole);

      (a: string) => void라는 문법은 "문자열 타입 a를 하나의 매개변수로 가지고 반환값이 없는 함수"를 의미한다. 함수 선언처럼, 매개변수의 타입이 지정되지 않으면, 암묵적으로 any가 된다.

        매개변수 이름이 필수 라는것을 명심하자. 함수 타입 (string) => void는 any 타입을 가진 string이라는 이름의 매개변수를 가진 함수를 뜻한다

      물론 타입 별칭을 사용해서 함수의 타입에 이름을 붙일 수 있다.

        type GreetFunction = (a: string) => void;
        function greeter(fn: GreetFunction) {
          // ...
        }


    호출 시그니처

      JavaScript에서, 함수들은 호출이 가능할 뿐만 아니라, 프로퍼티도 가질 수 있다. 하지만, 함수 타입 표현식 문법은 프로퍼티를 정의하는 것을 허락하지 않는다.
      만약 우리가 호출 가능하면서 프로퍼티를 가진 무언가를 설명하려고 하면, 객체 타입에 호출 시그니처를 사용하여 표현할 수 있다.

        type DescribableFunction = {
          description: string;
          (someArg: number): boolean;
        };
        function doSomething(fn: DescribableFunction) {
          console.log(fn.discription + " retruned " + fn(6));
        }

      이 문법은 함수 타입 표현식과 다르다. 매개변수 타입과 반환값의 타입 사이에 =>가 아닌 구 를 사용해야 한다.


    구성 시그니처

      JavaScript 함수는 new 연산자를 통해서도 호출 될 수 있다. TypeScript는 이런 것들이 주로 새로운 객체를 생성하는데 사용되기 때문에 생성자로 간주한다.
      여러분은 호출 시그니처 앞에 new 키워드를 붙임으로서, 구성 시그니처를 작성할 수 있다.

        type SomeConstructor = {
          new (s: string): SomeObject;
        };
        function fn(ctor: SomeConstructor) {
          return new ctor("hello");
        }

      JavaScript의 Date 객체와 같은 몇몇 객체는 new가 있든 없든 호출될 수 있다. 여러분은 호출 시그니처와 구성 시그니처를 임의로 같은 타입에서 결합시킬 수 있다.

        interface CallOrConstruct {
          new (s: string): Date;
          (n?: number): number;
        }


    제네릭 함수

      입력 값이 출력 값의 타입과 관련이 있거나, 두 타입값의 타입이 서로 관련이 있는 형태의 함수를 작성하는 것은 흔히 일어나는 일이다. 잠시 배열의 첫 번째 원소를 반환하는 함수를 생각해 보자.

        function firstElement(arr: any[]) {
          return arr[0];
        }

      함수는 제 역할을 하지만, 아쉽게도 반환 타입이 any이다. 함수가 배열의 원소의 타입을 반환한다면 더 나을 것 같다.
      TypeScript에서, 제네릭 문법이 두 값 사이의 상관관계를 표현하기 위해서 사용된다. 우리는 함수 시그니처에서 타입 매개변수를 선언함으로서 그런 표현을 할 수 있다.

        function firstElement<Type>(arr: Type[];): Type | undefined {
          return arr[0]
        }

      타입 매개변수 Type을 이 함수에 선언하고, 필요한 두 곳에 사용함으로써 우리는 함수의 입력 값(배열)과 출력(반환 값) 사이에 연결고리를 만들었다. 이제 우리가 이 함수를 호출할 때, 더 명확한 타입을 얻을 수 있다.

        // s는 "string" 타입
        const s = firstElement(["a", "b", "c"]);
        // n은 "number" 타입
        const n = firstElement([1, 2, 3]);
        // u는 "undefined" 타입
        const u = firstElement([]);


      추론(Inference)

        이 예제에서 우리는 Type을 특정하지 않았음에 주목하자. 여기서 타입은 추론되었다. 즉 TypeScript에 의해서 자동적으로 선택된 것이다.
        우리는 여러 개의 타입 매개변수도 사용할 수 있다. 예를 들어서, map의 스탠드얼론 버전은 아래와 같을 것이다.

          function map<Input, Output>(arr: Input[], func: (arg: Input) => Output): Output[] {
            return arr.map(func);
          }

          // 매개변수는 'n'의 타입은 'string'이다.
          // 'parsed'는 number[] 타입을 하고 있다.
          const parsed = map(["1", "2", "3"], (n) => parseInt(n));

        이 예제에서 TypeScript는 Input 타입과(입력으로 주어진 string 배열로부터) Output 타입을 함수 표현식의 반환 값(number)를 통해서 추론할 수 있는 점을 눈여겨보자.


      타입 제한 조건

        우리는 모든 타입에 대해서 동작하는 제네릭 함수들을 작성하였다. 가끔, 우리는 두 값을 연관시키기 원하지만 특정한 값들의 부분집합에 한해서만 동작하기를 원할 때가 있다. 
        이러한 경우에 우리는 타입제한 조건을 사용하여, 타입 매개변수가 받아들일 수 있는 타입들을 제한할 수 있다.

        두 값 중에 더 긴 것을 반환하는 함수를 작성해 보자. 이 작업을 위해, number인 length 프로퍼티가 필요하다. extends절을 이용하여 타입 매개변수를 그 타입으로 제한할 수 있다.

          function longest<Type extends { length: number } > (a: Type, b: Type) {
            if (a.length >= b.length) {
              return a;
            } else {
              return b;
            }
          }

          // longerArray 의 타입은 'number[]' 이다.
          const longerArray = longest([1, 2], [1, 2, 3]);
          // longerString 의 타입은 'alice' | 'bob' 이다.
          const longerString = longest("alice", "bob");
          // 에러 ! Number에는 'length' 프로퍼티가 없다.
          const notOK = longest(10, 100);

          Argument of type 'number' is not assinable to parameter of type '{ length: number; }'.

        이 예시에서는 몇 가지 흥미로운 점에 주목해야 한다. 우리는 TypeScript가 longest의 반환 타입을 추론 하도록 허용했다. 반환 타입 추론은 제네릭 함수에서도 작동한다.

        우리가 Type을 { length: number }로 제한했기에, 우리는 a와 b 매개변수에 대해서 .length 프로퍼티에 접근할 수 있었다. 타입 제한이 없다면, 이러한 값들이 length 프로퍼티를 가지지 않는 다른 타입일 수 있기 때문에, 그 프로퍼티에 접근할 수 없었을 것이다.

        longerArray와 longerString의 타입은 인수를 기반으로 추론되었다. 제네릭은 두 개 이상의 값을 같은 타입으로 연관짓는 것이라는 사실을 기억해야 한다.

        결욱 우리가 원하는 대로 longest(10,100)은 number 타입이 .length 프로퍼티를 가지고 있지 않았기 때문에 호출이 거부된 것을 볼 수 있다.


      제한된 값으로 작업하기

        다음은 제네릭 타입 제약 조건을 사용할 때, 흔히 범할 수 있는 실수이다.

          function minimumLength<Type extends { length: number }> (
            obj: Type,
            minimum: number
          ): Type {
            if (obj.length > minimum) {
              return obj;
            } else {
              return { length: minimum };

              Type '{ length: number; }' is not assignable to type 'Type'.
                '{ length: number; }' is assignable to the constraint of type 'Type', but 'Type' could be instantiated with a different subtype of constraint '{ length: number }'.

            }
          }

        이 함수는 문제가 없는 것처럼 보인다. Type은 { length: number }로 제한되어 있고, 함수는 Type이나 저 제약조건을 만족하는 값을 반환한다. 
        문제는 이함수가 제약사항을 만족하는 어떤 객체가 아닌 입력된 어떤 객체를 반환한다는 점이다.
        만약 이 코드가 유효하다면, 여러분은 확실히 동작하지 않을 아래의 코드를 작성할 수 있을 것이다.

          // 'arr' gets value { length : 6 }
          const arr = minimumLength([1, 2, 3], 6);
          // 여기서 배열은 'slice' 메서드를 가지고 있지만 반환된 객체는 그렇지 않기에 에러가 발생한다.
          console.log(arr.slice(0));


      타입 인수를 명시하기

        TypeScript는 제네릭 호출에서 의도된 타입을 대체로 추론해 내지만, 항상 그렇지는 않는다. 예를 들어서, 여러분들이 두 배열을 결합하는 함수를 하나 작성했다고 해보자.

          function combine<Type>(arr: Type[], arr2: Type[]): Type[] {
            return arr1.concat(arr2);
          }

        일반적으로 짝이 맞지 않는 배열과 함께 해당 함수를 부르는 것은 잘못된 것일 것이다.

          const arr = combine([1, 2, 3], ["hello"]);
          Type 'string' is not assignable to type 'number'.

        만약 여러분이 이런 것을 의도했다면, 여러분은 수동으로 Type을 명시해야 한다.
        
          const arr = combine<string | number>([1, 2, 3], ["hello"]);


      좋은 제네릭 함수를 작성하기 위한 가이드라인

        제네릭 함수를 작성하는 것은 재미있고, 타입 매개 변수를 사용하는 것이 쉬울 수 있다. 
        너무 많은 타입 매개변수나 제한조건을 꼭 필요하지 않은 곳에 사용하는 것은 추론을 잘하지 못하게 해서 여러분의 함수 호출자를 불만스럽게 만들 수 있다.


        타입 매개변수를 누르기

          여기 비슷해 보이는 두 함수를 쓰는 방법이 있다.

            function firstElement<Type>(arr: Type[]) {
              return arr[0];
            }

            function firstElement2<Type extends any[]>(arr: Type) {
              return arr[0];
            }

            // a: number (good)
            const a = firstElement1([1, 2, 3]);
            // b: any (bad)
            const b = firstElement2([1, 2, 3]);

          처음 보기에는 동일하게 보일 수 있지만, firstElement가 이 함수를 작성하는데 더 좋은 방법이다. 이 함수의 추론된 반환 타입은 Type 이지만,
          firstElement2의 추론된 반환 타입은 TypeScript가 호출 중에 타입을 해성하기 위해서 "기다리기"보다 호출 시점에 arr[0] 표현식을 타입 제한 조건을 이용해서 해석하기 때문에 any가 된다.

            규칙: 가능하다면, 타입 매개변수를 제약하기보다는 타입 매개변수 그 자체를 사용하면 안된다.


-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
