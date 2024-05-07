<template>
	<div></div>
</template>
<!--  

  Symbols

    ECMAScript 2015부터, symbol은 number와 string과 같은 기본 데이터 타입이다.

    symbol값은 Symbol 생성자를 호출함으로써 생성된다.

      let sym1 = Symbol();

      let sym2 = Symbol("key"); // 선택적 문자열 키

    심볼은 불변하고 유일하다.

      let sym2 = Symbol("key");
      let sym3 = Symbol("key");

      sym2 === sym3; // false, 심볼은 유일하다.

    문자열처럼, 심볼은 객체 프로퍼티의 키로써 사용될 수 있다.

      const sym = Symbol();

      let obj = {
        [sym]: "value",
      };

      console.log(obj[sym]); // "valuse"

    또한 심볼은 초기화된 프로퍼티 선언과 결합되어 객체 프로퍼티와 클래스 멤버를 선언할 수 있다.

      const getClassNameSymbol = Symbol();

      class C {
        [getClassNameSymbol]() {
          return "C";
        }
      }

      let c = new C();
      let className = c[getClassNameSymbol](); // "C"


  잘 알려진 심볼 (Well-known Symbols)

    사용자 정의 심볼 외에, 잘 알려진 내장 심볼이 있다. 내장 심볼은 언어 내부의 동작을 나타내는데 사용된다.
    아래는 잘 알려진 심볼의 목록이다.

    Symbol.hasInstance - 생성자가 객체를 생성자의 인스턴스 중 하나로 인식하는지 확인하는 메서드, instanceof 연산자로 호출된다.
    Symbol.inConcatSpreadable - 객체가 자신의 배열 요소를 Array.prototype.concat을 사용하여 직렬화할 수 있는지 나타내는 Boolean 값.
    Symbol.iterator - 객체의 기본 반복자를 반환하는 메서드. for-of 문으로 호출된다.
    Symbol.match - 정규 표현식과 문자열을 비교하는 정규 표현식 메서드. String.prototype.match 메서드로 호출된다.
    Symbol.replace - 문자열에서 일치하는 부분 문자열을 치환하는 정규 표현식 메서드. String.prototype.replace 메서드로 호출된다.
    Symbol.search - 정규 표현식과 매치되는 문자열의 인덱스를 반환하는 정규 표현식 메서드. String.prototype.search 메서드로 호출된다.
    Symbol.species - 파생된 객체를 생성하는데 사용하는 생성자 함수의 중요한 속성
    Symbol.split - 정규 표현식과 매치되는 인덱스들에 위치한 문자열을 나누는 정규 표현식 메서드. String.prototype.split 메서드로 호출된다.
    Symbol.toPrimitive - 객체를 대응되는 기본 값으로 변환하는 메서드. ToPrimitive 추상 연산으로 호출된다.
    Symbol.toStringTag - 객체의 기본 문자열 형식을 만드는데 사용되는 문자열 값. 내장 메서드인 Object.prototype.toString로 호출된다.
    Symbol.unscopables - 자신의 프로퍼티 이름이 연결된 개체의 'with' 환경 바인딩에서 제외되는 프로퍼티 이름인 객체.


  타입스크립트에서 Symbol은 유니크하고 불변(immutable)한 데이터 이다. Symbol은 ES6에서 도입되었으며,
  주로 객체 속성의 키로 사용되어 객체 속성을 식별하는 데에 쓰인다.
  일반적으로 문자열을 키로 사용할 때 발생할 수 있는 충돌을 피할 수 있다.

  심볼은 옵셔널로 설명 문자열을 가질 수 있다. 이 설명은 디버깅에 도움이 되는 용도로 사용될 수 있다.
  Symbol은 주로 객체의 키로 사용되지만, 일반적인 반복문에서 무시되므로, 객체 속성을 숨기거나 의도적으로 노출하지 않고 안전하게 사용할 수 있다.

  타입스크립트에서 Symbol은 기본 데이터 타입 중 하나로 제공되므로, 선언할 때 명시적인 타입 어노테이션을 사용할 필요가 없다.


  Symbol이 사용되는 경우

  1. 객체 속성 실별자로 사용
    Symbol은 유일성을 보장하기 때문에, 객체의 키로 사용하면 충돌을 피할 수 있다. 예를 들어, 라이브러리나 프레임워크에서 내부적으로 사용되는 특별한 속성을 정의할 때 유용하다.

  2. Well-known symbols
    자바스크립트 내장 객체의 특별한 동작을 정의하기 위해 사용된다.
    예를 들어, Symbol.iterator는 객체가 이터러블(iterable)하다는 것을 나타내며 for...of 루프에서 사용될 수 있도록 한다.

  3. Private 속성 및 메서드
    Symbol을 사용하면 객체의 내부 상태를 외부로부터 은닉할 수 있다. 외부에서 접근할 수 없도록 보호되어야 하는 속성이나 메서드를 만들 때 유용하다.

  4. Enum-like values: Symbol을 사용하여 열거형(enum)과 유사한 패턴을 만들 수 있다. Enum과 달리 Symbol은 유일하며 열거형 값과 분리된다.
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
