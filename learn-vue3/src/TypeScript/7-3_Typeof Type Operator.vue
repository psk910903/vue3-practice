<template>
	<div></div>
</template>
<!--  

  Typeof Type Operator

    타입스크립트에서 typeof 연산자는 자바스크립트의 typeof 연산자와 유사하게 변수 또는 객체의 타입을 추출할 때 사용된다.
    하지만 타입스크립트에서 typeof는 주로 타입 컨텍스트에서 사용되며, 런타임이 아닌 컴파일 시간에 타입 정로르 얻는 데 사용된다.
    이를 통해 동적으로 객체의 타입을 기반으로 새로운 타입을 정의할 수 있다.


    typeof의 기본 사용법

        let x = "Hello world";
        type T typeof x; // T는 string 타입이 된다.

      여기서 변수 x는 문자열 값을 가지므로 typeof x는 "string" 타입을 반환한다. T 타입은 이제 "string"과 동일하다.


    typeof와 객체

      typeof를 객체에 사용하면, 해당 객체의 구조를 기반으로 한 타입을 추출할 수 있다.

        const person = {
          name: "John",
          age: 30
        };

        type Person = typeof person;
        // Person 타입은 { name: string, age: number }와 같다.

      이 예시에서 Person 타입은 person 객체의 구조를 반영하여 { name: string; age: number; }로 정의된다.
      이는 객체 리터럴 타입을 기반으로 새로운 타입을 생성할 때 매우 유용하다.


    함수의 반환 타입 추출

      typeof를 함수와 함께 사용하여 함수의 반환 타입을 추출할 수도 있다.

        function sum(a: number, b: number) {
          return a + b;
        }

        type ReturnTypeOfSum = typeof sum; // (a: number, a: number) => number

      여기서 RetrunTypeOfSum은 sum함수의 전체 시그니처를 타입으로 가진다. 만약 단지 반환 타입만을 얻고 싶다면 타입스크립트의 유틸리티 타입 ReturnType<T>을 사용할 수 있다.

        type OutputOfSum = ReturnType<typeof sum>; // number


    실제 활용 예

      typeof는 실제 코드에서 다양한 형태로 활용될 수 있다. 
      예를 들어, 함수의 매개변수 타입을 동적으로 설정하거나, 객체 리터럴을 이용한 설정 등에서 유용하게 사용된다.

        function setSettings(settings: typeof defultSettings) {
          // 설정 적용 로직
        }

        const defultSettings = {
          darkMode: true,
          version: 1
        };

        setSettings({ darkMode: false, version: 2 }); // 정상 작동
        setSettings({ darkMode: false }); // 에러: version 속성이 누락됨.

      이렇게 typeof를 사용하면 기존의 변수나 상수의 타입을 재사용하여 타입 일관성을 유지하고, 코드의 중복을 줄일 수 있다.
      타입스크립트에서 typeof는 코드의 유연성과 안정성을 동시에 높여주는 강력한 도구이다.


    typeof 타입 연산자

      JavaScript에서는 이미 표현식 컨텍스트에서 사용할 수 있는 typeof 연산자가 있다.

        // "string"을 출력한다.
        console.log(typeof "Hello world");

      TypeScript는 타입 컨텍스트에서 변수나 프로퍼티의 타입을 추론할 수 있는 typeof 연산자를 추가한다.

        let s = "hello";
        let n: typeof s;

          let n: string;

      기본 타입에 대해선 별로 유용하진 않지만, 다른 타입 연산자와 함께 typeof를 사용하여 많은 패턴을 편리하게 표현할 수 있다.
      예를 들어, 미리 정의된 타입인 ReturnType<T> 부터 살펴보자. 위 타입은 함수 타입을 받으면서 반환되는 타입을 제공한다.

        type Predicate = (x: unknown) => boolean;
        type K = ReturnType<Predicate>;

          type K = boolean;

      함수 이름에 ReturnType을 사용하면, 안내 오류를 확인할 수 있다.

        function f() {
          return { x: 10, y: 3 };
        }

        type P = ReturnType<f>;

        'f' refers to a value, but is being used as a type here. Did you mean 'typeof f'?

      값과 타입은 같지 않다는 것을 명심하자. 값 f의 타입을 추론하기 위해 typeof를 사용한다.

        function f() {
          return { x: 10, y: 3};
        }
        type P = ReturnType<typeof f>;

          type P = {
            x: number;
            y: number;
          }


      제한

        TypeScript는 typeof를 사용할 수 있는 표현식의 종류를 의도적으로 제한한다.
        특히, 식별자(예: 변수이름) 혹은 프로퍼티에서만 typeof를 사용할 수 있다.
        실행 중인 것으로 생각되는 코드 작성의 실수를 피하는데 도움을 줄 수 있지만, 그렇지 않다.

          // Meant to use = ReturnType<typeof msgbox>
          let shouldContinue = typeof msgbox("Are you sure you want to continue?");

          ',' expected.
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
