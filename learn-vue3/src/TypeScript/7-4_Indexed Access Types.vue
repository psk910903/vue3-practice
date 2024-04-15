<template>
	<div></div>
</template>
<!--  

  Indexed Access Types

    타입스크립트에서 Indexed Access Types는 특정 객체 타입에서 특정 키에 해당하는 값의 타입을 참조할 수 있게 해주는 기능이다.
    이를 사용하여 다른 타입의 프로퍼티에 접근하거나, 배열 내 요소의 타입에 접근하는 등 다양한 방식으로 유용하게 사용할 수 있다.


    기본 사용법

      Indexed Access Type은 Type[Key] 형태로 사용되며 Key는 주로 문자열 리터럴 타입이나 숫자 리터럴 타입을 사용한다.
      객체 타입의 특정 키에 대한 값을 참조할 때, 그 키의 타입을 통해 접근할 수 있다.

        type Person = {
          name: string;
          age: number;
        };

        // name 키에 해당하는 타입을 가져온다.
        type NameType = Person['name']; // string

        // age 키에 해당하는 타입을 가져온다.
        type AgeType = Person['age']; // number

      이처럼 'Person['name']'은 string 타입을, 'Person['age']'는 number타입을 반환한다.


    배열의 요소 타입 참조

      Indexed Access Type은 배열 또는 튜플의 특정 인텍스에 위치한 요소의 타입을 참조할 때도 사용될 수 있다.

        type StringArray = string[];
        type StringType = StringArray[0]; // string

        type TupleType = [string, number];
        type FirstType = TupleType[0]; // stirng
        type secondType = TupleType[1]; // number

      위 예제에서 StringArray[0]은 배열의 요소 타입은 string을 반환한다. TupleType[0]과 TupleType[1]은 각각 튜플의 첫 번째 요소인 string과 두 번째 요소인 number 타입을 반환한다.


    객체 타입과 함께 유니언 타입 사용

      Indexed Access Type은 유니언 타입과 결합하여 여러 키의 타입을 합 번에 추출할 수도 있다.

        type Person = {
          name: string;
          age: number;
          email: string;
        };

        type KeyType = 'name' | 'email';
        type PersonInfo = Person[KeyType]; // string

      이 예제에서 KeyType은 name과 email의 유니언 타입이다. Person[KeyType]은 name과 email키의 값 타입인 string을 반환한다.


      실제 활용 예제

        Indexed Access Types는 객체의 구조를 동적으로 조작하거나, 다른 타입의 정보를 기반으로 새로운 타입을 생성할 때 매우 유용하게 활용할 수 있다.
        예를 들어, 객체의 특정 키만을 선택하여 새로운 타입을 만드는 경우 등에서 사용할 수 있다.

          type Person = {
            name: string;
            age: number;
            email: string;
          }

          type Pick<T, K extend keyof T> = {
            [P in K]: T[P];
          }

          type PersonNameEmail = Pick<Person, 'name' | 'email'>;
          // { name: string; email: string }

        여기서 'Pick' 타입은 제네릭과 매핑된 타입을 이용해 Person 타입에서 name과 email 키만을 추출하여 새로운 타입 PersonNameEmail을 생성한다.
        이는 매우 간편하게 특정 속성만을 가진 객체 타입을 정의할 수 있게 해준다.

        Indexed Access Types는 타입스크립트에서 타입 안정성을 높이고, 타입 조작을 더욱 유연하게 할 수 있게 해주는 강력한 기능이다.


    타입의 특정 프로퍼티를 찾기 위해서 인덱싱된 접근 타입을 사용할 수 있다.

      type Person = {
        age: number;
        name: string;
        alive: boolean;
      };
      type Age = Person["age"];

        type Age = number

    인덱싱된 타입은 그 자체로도 타입이라서 유니언, keyof 혹은 타입 전체에 사용할 수 있다.

      type I1 = Person["age" | "name"];

        type I1 = string | number


      type I2 = Person[keyof Person];

        type I2 = string | number | boolean


      type AliveOrName = "alive" | "name";
      type I3 = Person[AliveOrName];

        type I3 = string | boolean

    존재하지 않는 프로퍼티를 인덱싱하려고 하면 오류가 발생한다.

      type I1 = Person["alve"];
      property 'alve' does not exist on type 'Person'.

    또 다른 예로는 임의의 타입을 number로 인덱싱해서 배열 요소의 타입을 가져올 수 있다.
    typeof와 결합하면 편리하게 배열 리터럴의 요소 타입을 캡처할 수 있다.

      const MyArray = [
        { name: "Alice", age: 15 },
        { name: "Bob", age: 23 },
        { name: "Eve", age: 38 },
      ];

      type Person = typeof MyArray[number];

        type Perosn = {
          name: string,
          age: number,
        }

      type Age = typeof myArray[number]["age"];

        type Age = number

      // Or
      type Age2 = Person["age"];

        type Age2 = number

    인덱싱할 때 변수 참조를 위해 사용된 const는 사용할 수 없고, 오로지 타입만 사용 가능하다.

      const key = "age";
      type Age = Person[key];

      Type 'key' cannot be used as an index type.
      'key' refers to a value, but is being used as a type here. Did you mean 'typeof key'?

    하지만, 비슷한 스타일의 리팩터로 타입 별칭을 사용할 수 있다.

      type key = "age";
      type Age = Person[key];
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
