<template>
	<div></div>
</template>
<!--  

  keyof 타입 연산자

    타입스크립트에서 keyof 타입 연산자는 객체 타입의 모든 퍼블릭 키를 문자열 또는 숫자 리터럴 형태로 유니언 타입으로 추출하는 데 사용된다.
    이는 특히 타입의 속성 이름만을 참조해야 할 때 유용하다.

    
    기본 사용법

      interface Person {
        name: string;
        age: number;
      }

      type Personkeys = keyof Person;
      // Personkey는 'name' | 'age' 유니언 타입과 동일하다.

    여기서 PersonKeys 타입은 'name' | 'age'와 같은 유니언 타입이 되며, Person 인터페이스의 모든 키들을 나타낸다.
    keyof의 결과로 반환된 유니언 타입은 해당 키에 접근하거나 할당할 때 사용할 수 있다.


    keyof와 제네릭

      keyof는 제네릭과 함께 사용될 때 매우 유용하다.
      객체의 타입이 정확히 알려지지 않았거나 유동적인 상황에서 각 객체의 키에 안전하게 접근할 수 있도록 한다.

        function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
          return obj[key];
        }

        const person = {
          name: 'John',
          age: 30
        };

        const name = getProperty(person, 'nane'); // 'John'
        const age = getProperty(person, 'age'); // 30

      이 예시에서 getProperty 함수는 제네릭을 이용해 유연하면서도 타입-안전한 방법으로 객체의 속성에 접근할 수 있도록 한다.
      K는 T의 키 중 하나에 해당하는 타입을 가지므로, 잘못된 키 이름의 함수를 호출하면 컴파일 시점에서 오류를 발생시킨다.


    keyof와 매핑된 타입

      keyof는 매핑된 타입과 함께 사용될 때 객체의 각 키에 대한 타입을 변형시키는데 사용할 수 있다.

        type ReadonlyPerson = {
          readonly [K in keyof Person]: Person[K];
        }

        // ReadonlyPerson 타입은 이제 모든 속성 읽기 전용이다.
        // {
        //   readonly name: string;
        //   readonly age: number;
        // }

      위 코드에서 ReadonlyPerson은 Person의 각 속성 읽기 전용으로 만든다.
      이러한 패턴은 객체의 불변성을 보장하는 데 유용하다.

      keyof 연산자를 통해 타입스크립트에서 객체의 키를 보다 강력하고 유연하게 달루 수 있게 되며, 코드의 안정성과 유지보수성을 높이는 데 기여한다.


    keyof 연산자는 객체 타입에서 객체의 키 값들을 숫자나 문자열 리터럴 유니언을 생성한다. 아래 타입 P는 "x" | "y"와 동일한 타입이다.

      type Point = { x: number; y: number };
      type P = keyof Point

        type P = keyof Point

    만약 타입이 string이나 number 인덱스 시그니처를 가지고 있다면, keyof는 해당 타입을 리턴한다.

      type Arrayish = { [n: number]: unknown };
      type A = keyof Arrayish;

        type A = number

      type Mapish = { [k: string]: boolean };
      type M =keyof Mapish;

        type M = string | number

    위 예제에서 주목할 점은 M은 string | number라는 점이다.
    JavaScript 객체 키는 항상 문자열을 강제하기 때문에, obj[0]은 obj["0"]과 동일하다.



-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
