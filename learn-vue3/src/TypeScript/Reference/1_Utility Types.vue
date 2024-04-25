<template>
	<div></div>
</template>
<!--  

  Utility Types

    TypeScript는 일반적인 타입 변환을 쉽게 하귀 위해서 몇 가지 유틸리티 타입을 제공한다.
    이러한 유틸리티는 전역으로 사용가능하다.


    Pratial<Type>

      Type 집합의 모든 프로퍼티를 선택적으로 타입을 생성한다. 이 유틸리티는 주어진 타입의 모든 하위 타입 집합을 나타내는 타입을 반환한다.

      예를 들어, 다음과 같은 인터페이스가 있다고 가정해보자.

        interface User {
          name: string;
          age: number;
        }

      이제 이 인터페이스를 Partial을 사용하여 변환해보자.

        type PartialUser = Partial<User>;

        // PartialUser의 타입은 다음과 같다.
        // {
        //  name?: string | undefined; 
        //  age?: number | undefined; 
        // }

      위에서 볼 수 있듯이, Partial<User>를 사용하면 User 인터페이스의 모든 속성이 선택적으로 처리되어 name과 age 속성이 필수가 아니라 선택적으로 정의된다.
      이것은 객체를 생성할 때 모든 속성을 반드시 제공할 필요가 없다는 의미이다.


    Required<Type>

      Type 집합의 모든 프로퍼티를 필수로 설정한 타입을 생성한다. Partial의 반대이다.
      Required<Type> 유틸리티 타입은 주어진 타입 Type의 모든 속성을 필수로 만들어 준다.
      다시 말해, Required<Type>을 사용하면 해당 타입의 모든 속성이 선택적이 아니라 필수로 다루어 진다.

      예를 들어, 다음과 같은 인터페이스가 있다고 가정해보자.

        interface User {
          name?: string,
          age?: number,
        }

      이제 이 인터페이스를 Required를 사용하여 변환해보자.

        type RequiredUser = Required<User>;

        // RequiredUser의 타입은 다음과 같다.
        // {
        //  name: string; 
        //  age: number; 
        // }

      위에서 볼 수 있읏이, Required<User>를 사용하면 User 인터페이스의 모든 속성이 선택적이 아니라 필수로 정의된다.
      이것은 객체를 생성할 때 모든 속성을 반드시 제공해야 한다는 의미이다.


    Readonly<Type>

      Type 집합의 모든 프로퍼티가 읽기 전용(readonly)으로 설정한 타입을 생성한다. 즉 생성된 타입의 프로퍼티는 재할당될 수 없다.
      Readonly<Type>을 사용하면 해당 타입의 모든 속성의 값을 변경할 수 없게 만든다.
      이 유틸리티는 런타임에 실패할 할당 표현식을 표현할 때 유용하다.

        interface User {
          name: string;
          age: number;
        }

      이제 이 인터페이스를 Partial을 사용하여 변환해보자.

        type ReadonlyUser = Readonly<User>;

        // ReadonlyUser의 타입은 다음과 같다.
        // {
        //  readonly name: string; 
        //  readonly age: number; 
        // }

      위에서 볼 수 있듯이, Readonly<User>를 사용하면 User 인터페이스의 모든 속성이 읽기 전용으로 처리된다.
      이것은 해당 객체를 생성한 후에는 속성의 값을 변경할 수 없다는 것을 의미한다.


    Record<Keys, Type>

      타입 Type의 프로퍼티 key의 집합으로 타입을 생성한다. 이 유틸리티는 타입의 프로퍼티를 다른 타입에 매핑 시키는데 사용될 수 있다.
      Record<Keys, Type> 유틸리티 타입은 특정 키와 해당 키에 매핑되는 값의 타입을 지정하여 객체 타입을 생성한다.
      즉, 주어진 키 배열 Keys와 해당 키에 대한 값을 나타내는 타입 Type을 사용하여 새로운 객체를 만든다.

      예를 들어, 다음과 같이 사용할 수 있다.

        type Person = {
          name: string;
          age: number;
        };

        type PersonRecord = Record<'Alice' | 'Bob' | 'Charlie', Person>;

        // PersonRecord의 타입은 다음과 같다.
        // {
        //    Alice: Person;
        //    Bob: Person;
        //    Charlie: Person;
        // }


    Pick<Type, Keys>

      Type에서 프로퍼티 Keys의 집합을 선택해 타입을 생성한다.
      Pick<Type, Keys> 유틸리티 타입은 주어진 타입 Type에서 지정된 키 집합 Keys에 해당하는 속성만 선택하여 새로운 타입을 생성한다.
      즉, 특정 키들에 해당하는 속성만을 가진 부분 집합을 만들어낸다.

      예를 들어, 다음과 같이 사용할 수 있다.

        interface Person {
          name : string;
          age: number;
          address: string;
        }

        type PersonInfo = Pick<Person, 'name' | 'age'>;

        // PersonInfo의 타입은 다음과 같다.
        // {
        //    name: string;
        //    age: number;
        // }

      위의 예제에서는 Person이라는 인터페이스를 정의하고 이를 Pick을 사용하여 name과 age 속성만을 가진 새로운 타입인 PersonInfo를 생성했다.
      따라서 PersonInfo는 Person 인터페이스의 일부 속성만을 포함하게 된다.

      이러한 유틸리티 타입은 기존 타입에서 필요한 속성만을 선택하여 새로운 타입을 만들어낼 때 유용하며, 객체의 일부 속성만을 활용할 필요가 있는 경우에도 매우 유용하다.


    Omit<Type, Keys>

      Type 에서 모든 프로퍼티를 선택하고 키를 제거한 타입을 생성한다.
      Omit<Type, Keys> 유틸리티 타입은 주어진 타입 Type에서 지정된 키 집합 Keys에 해당하는 속성을 제외한 나머지 속성을 가진 새로운 타입을 생헝한다.
      즉, 특정 키들을 제외한 속성들의 부분집합을 만들어낸다.
      
      예를 들어, 다음과 같이 사용할 수 있다.

        interface Person {
          name: string;
          age: number;
          address: string;
        }

        type PersonWithoutAge = Omit<Person, 'age'>;

        // PersonWithoutAge의 타입은 다음과 같다.
        // {
        //    name: string;
        //    address: string;
        // }

      위의 예제에서는 Person이라는 인터페이스를 정의하고, 이를 Omit을 이용하여 age 속성을 제외한 새로운 타입인 PersonWithoutAge를 생성했다.
      따라서 PersonWithoutAge는 name과 address 속성만을 가지게 된다.

      이러한 유틸리티 타입은 기존 타입에서 특정 속성을 제외하고 새로운 타입을 만들어낼 때 유용하다.


    Exclude<Type, ExcludedUnion>

      Exclude<Type, ExcludeUnion> 유틸리티 타입은 주어진 타입 Type에서 ExcludedUion에 포함된 모든 타입을 제외한 타입을 생성한다.
      즉, Type에 포함된 타입 중 ExcludeUion에 있는 타입들을 제외한 나머지 타입을 반환한다.

      예를 들어, 다음과 같이 사용할 수 있다.

        type Color = 'Red' | 'Green' | 'Blue';

        type PrimaryColor = Exclude<Color, 'Green' | 'Blue'>;

        // PrimaryColor의 타입은 다음과 같다.
        // 'Red'

      위의 예제에서는 Color라는 유니온 타입을 정의하고, 이를 Exclude를 사용하여 Green과 Blue를 제외한 타입인 PrimaryColor를 생성했다.
      결과적으로 PrimaryColor는 Red만을 포함하는 타입이 된다.

      이러한 유틸리티 타입은 특정 유니온 타입에서 원치 않는 타입을 제외하고자 할 때 유용하다.


    Extract<Type, Union>

      Extract<Type, Union> 유틸리티 타입은 주어진 타입 Type에서 Union에 포함된 모든 타입을 추출하여 새로운 타입을 생성한다.
      즉, Type에 포함된 타입 중 Union에 있는 타입들만을 추출하여 반환한다.

      예를 들어, 다음과 같이 사용할 수 있다.

        type Color = 'Red' | 'Green' | 'Blue';

        type PrimaryColors = Extract<Color, 'Red' | 'Green'>;

        // PrimaryColors의 타입은 다음과 같다.
        // 'Red' | 'Green'

      위의 예제에서는 Color라는 유니온 타입을 정의하고, 이를 Extract를 사용하여 Red와 Green을 추출한 타입인 PrimaryColors를 생성했다.
      결과적으로 PrimaryColors는 Red와 Green을 포함하는 타입이 된다.

      이러한 유틸리티 타입은 특정 유니온 타입에서 필요한 타입들만을 추출하고자 할 때 유용하다.


    NonNullable<Type>

      NonNullable<Type> 유틸리티 타입은 주어진 타입 Type에서 null과 undefined를 제외한 모든 타입을 추출하여 새로운 타입을 생성한다.
      즉, Type에 포함된 타입 중에서 null과 undefined를 제외한 나머지 타입을 반환한다.

      예를 들어, 다음과 같이 사용할 수 있다.

        type NullableString = string | null | undefined;

        type NonNullable<NullableString>;

        // NonNullString의 타입은 다음과 같다.
        // string

      위의 예제에서는 NullableString이라는 유니온 타입을 정의하고, 이를 NonNullable을 사용하여 null과 undefined를 제외한 타입인 NonNullString을 생성했다.
      결과적으로 NonNullString은 string 타입만을 포함하는 타입이 된다.

      이러한 유틸리티 타입은 주어진 타입에서 null 또는 undefined 값을 제외하고자 할 때 유용하다.


    Parameters<Type>

      Parameters<Type> 유틸리티 타입은 주어진 함수 타입 Type의 매개변수 타입들을 추출하여 튜플 형태로 반환한다.

      예를 들어, 다음과 같이 사용할 수 있다.

        type MyFunction = (x: number, y: string) => void;

        type MyFunctionParameters = Parameters<MyFunction>;

        // MyFunctionParameters의 타입은 다음과 같다.
        // [number, string]

      위의 예제에서는 MyFunction 이라는 함수 타입을 정의하고, 이를 Parameters를 사용하여 함수의 매개변수 타입들을 추출한 MyFunctionParameters를 생성했다.
      결과적으로 MyFucntionsParameters는 함수 MyFunction의 매개변수 타입을 타나내는 튜플 타입이 된다.

      이러한 유틸리티 타입은 함수의 매개변수를 추출하여 제네릭이나 다른 용도로 활용할 때 유용하다.


    ConstructorParameters<Type>

      생성자 함수 타입의 타입에서 튜플 또는 배열 타입을 생성한다. 모든 매개변수 타입을 가지는 튜플 타입(또는 Type이 함수가 아닌 경우 타입 never)를 생성한다.
      주어진 생성자 함수 타입 Type의 매개변수 타입들을 추출하여 튜플 형태로 반환한다.
      생성자 함수 타입은 일반적으로 클래스의 생성자를 나타낸다. 이 유틸리티 타입은 주어진 클래스의 생성자 함수에서 사용된 매개변수의 타입들을 추출하여 튜플로 반환한다.

      예를 들어, 다음과 같이 사용할 수 있다.

        class Person {
          constructor(name: string, age: number) {
            // constructor implementation...
          }
        }

        type ConstructorParams = ConstructorParameters<typeof Person>;

        // ConstructorParams의 타입은 다음과 같다.
        // [name: string, age: number]

      위의 예제에서는 Person 클래스의 생성자 함수에서 사용된 매개변수의 타입들을 추론하여 ConstructorParams라는 타입에 할당했다.
      결과적으로 ConstructorParams는 생성자 함수의 매개변수 타입들을 튜플로 나타낸다.

      이러한 유틸리티 타입은 클래스 생성자의 매개변수를 추출하여 제네릭 등의 다양한 용도로 활용할 때 유용하다.


    ReturnType<Type>

      ReturnType<Type> 유틸리티 타입은 주어진 함수 타입 Type의 반환 타입을 추출한다.

      예를 들어, 다음과 같이 사용할 수 있다.

        function greet(): string {
          return "Hello";
        }

        type GreetReturnType = ReturnType<typeof greet>;

        // GreetReturnType의 타입은 다음과 같다.
        // string

      위의 예제에서는 greet 함수의 반환 타입을 추출하여 GreetReturnType에 할당했다. 
      결과적으로 GreetReturnType은 greet 함수의 반환 타입을 나타내는 문자열 타입이 된다.

      이러한 유틸리티 타입은 함수의 반환 타입을 추출하여 제네릭 등의 다양한 용도로 활용할 때 유용하다.


    instanceType<Type>

      instanceType<Type> 유틸리티 타입은 주어진 생성자 함수 타입 Type의 인스턴스 타입을 추출한다.

      생성자 함수 타입은 일반적으로 클래스를 나타내며, 이 유틸리티 타입은 클래스의 인스턴스 타입을 추출하여 반환한다.

      예를 들어, 다음과 같이 사용할 수 있다.

        class Person {
          constructor(public name: string, bluck age: number) {}
        }

        type PersonInstance = InstanceType<typeof Person>;

        // PersonInstance의 타입은 다음과 같다.
        // Person

      위의 예제에서는 Person 클래스의 생성자 함수 타입을 추출하여 PersonInstance에 할당했다.
      결과적으로 PersonInstance는 Person 클래스의 인스턴스 타입을 나타낸다.

      이러한 유틸리티 타입은 클래스의 인스턴스 타입을 추출하여 제네릭 등의 다양한 용도로 활용할 때 유용하다.


    ThisParameterType<Type>

      함수 타입의 this 매개변수의 타입, 또는 함수 타입에 this 매개변수가 없을 경우 unknown을 추출한다.
      일반적으로 이 유틸리티는 함수에서 this 매개변수를 사용할 때 유용하다.
      TypeScript는 함수에 대한 this의 타입을 추론할 수 있지만, 때로는 명시적으로 this의 타입을 지정해야 할 때가 있다.
      이 때, ThisParameterType을 사용하여 해당 함수의 this 매개변수의 타입을 추출할 수 있다.

      예를 들어, 다음과 같이 사용할 수 있다.

        function greet(this: {name: string}): void {
          console.log(`Hello, ${this.name}`);
        }

        type ThisType = ThisParameterType<typeof greet>;

        // ThisType의 타입은 다음과 같다.
        // { name: string }

      위의 예제에서는 greet 함수의 this 매개변수의 타입을 추출하여 ThisType에 할당했다.
      결과적으로 ThisType은 { name: string } 타입을 나타낸다.

      이러한 유틸리티 타입은 함수 내에서 this를 명시적으로 지정하고 해당 this의 타입을 추출할 때 유용하다.


    OmitThisParameter<Type>

      Type에서 this 매개변수를 제외한 나머지 매개변수들의 타입을 추출한다.

      일부 함수에는 this 매개변수가 있을 수 있다. 이러한 경우에는 함수의 실제 매개변수가 this를 포함하여 추출되기 때문에, 때로는 this 매개변수를 제외하고 나머지 매개변수들만을 추출하고 싶을 때가 있다.
      이 때 OmitThisParameter 유틸리티 타입을 사용할 수 있다.

        function greet(this: {name: string}, message: string): void {
          console.log(`${this.name} says: ${message}`)
        }

        type GreetParameters = OmitThisParameter<typeof greet>;

        // GreetParameters의 타입은 다음과 같다.
        // [message: string]

      위의 예제에서는 greet 함수의 매개변수에서 this 매개변수를 제외한 나머지 매개변수들의 타입을 추출하여 GreetParameters에 할당했다.
      결과적으로 GreetParameters는 문자열 타입의 매개변수를 담고 있는 배열 형태의 타입을 나타낸다.

      이러한 유틸리티 타입은 함수의 매개변수에서 this 매개변수를 제외하고 싶을 때 유용하다.


    ThisType<Type>

      ThisType<Type> 유틸리티 타입은 주어진 함수 타입 Type 내에서 this 표현식의 타입을 추론한다.

      이 유틸리티는 주로 다음 두 가지 상황에서 사용된다.

      1. 객체 리터럴의 메서드에서 this의 타입을 명시적으로 지정할 때
      2. ThisType<Type>을 사용하여 특정 타입의 메서드를 별도의 인터페이스로 추출할 때

        type MyObject = {
          name: string;
          sayHello(this: MyObject): void;
        };

        const obj: MyObject = {
          name: "Alice",
          sayHello() {
            consoel.log(`Hello, ${this.name}`)
          }
        };

        obj.sayHello(); // "Hello, Alice"

      위의 예제에서는 MyObject라는 타입을 정의하고 이 타입에 sayHello라는 메서드를 추가했다.
      sayHello 메서드에서 this의 타입을 MyObject로 명시적으로 지정했다.
      이 때문에 obj.sayHello()를 호출할 때 TypeScript는 this가 MyObject 타입임을 알고 있으므로 에러가 발생하지 않는다.

      이러한 유틸리티 타입은 객체 리터럴의 메서드에서 this의 타입을 정의할 때 유용하다.

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
