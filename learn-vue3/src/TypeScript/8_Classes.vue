<template>
	<div></div>
</template>
<!--  

	Classes

		TypeScript는 ES2015에서 소개된 class 키워드를 완벽하게 지원한다.

		다른 JavaScript 언어 기능과 마찬가지로 TypeScript는 타입 주석 및 다른 구문을 추가하여 클래스와 다른 타입 간의 관계를 표현할 수 있다.


		클래스 멤버

			가장 기본적인 클래스는 다음과 같다. 아무것도 없는 클래스:

				class Point {}

			이 클래스는 아직 유용하지 않으므로 몇 가지 멤버를 추가해 보자.


			필드

				필드 선언은 클래스에 공개적인 쓰기 가능한 속성을 만든다.

					class Point {
						x: number;
						y: number;
					}

					const pt = new Point();
					pt.x = 0;
					pt.y = 0;

			다른 위치와 마찬가지로 타입 주석은 선택 사항이지만, 지정되지 않으면 암묵적으로 any가 된다.
			필드에는 initializer도 있을 수 있다. 이들은 클래스가 인스턴스화될 때 자동으로 실행된다.

				class Point {
					x = 0;
					y = 0;
				}

				const pt = new Point();
				// Prints 0, 0
				console.log(`${pt.x}, ${pt.y}`);

			const, let, var와 마찬가지로 클래스 속성의 initializer는 해당 유형을 추론하는 데 사용된다.

				const pt = new Point();
				pt.x = "0";

				Type 'string' is not assignable to type 'number'.


			--strictPropertyInitialization

			설정은 클래스 필드가 생성자에서 초기화되어야 하는지 여부를 제어한다.

				class BadGreeter {
					name: string;

				Property 'name' has no initializer and is not definitely assigned in the constructor.
				}

				class GoodGreeter {
					name: string;

					constructor() {
						this.name = "hello";
					}
				}

			! 연산자는 필드가 생성자 와의 방법으로 초기화되리라는 의도를 나타낸다.
			예를 들어, 외부 라이브러리가 클래스의 일부를 채워넣는 경우처럼 생성자 이외의 방법으로 필드를 확실히 초기화활 때 사용할 수 있다.

				class OKGreeter {
					// 초기화되지 않았지만 오류가 없다.
					name!: string;
				}


		readonly

			readonly 한정자를 사용하여 필드를 선언할 수 있다. 이렇게 하면 생성자 외부에서 해당 필드에 대한 할당이 금지된다.

				class Greeter {
					readonly name: string = "world";

					constructor(otherName?: stirng) {
						if (otherName !== undefined) {
							this.name = otherName;
						}
					}

					err() {
						this.name = "not ok";
				Cannot assign to 'name' because it is a read-only property.
					}
				}

				const g = new Greeter();
				g.name = "also not ok";
				Connot assign to 'name' because it is a read-only property.


		Constructors

			클래스 생성자는 함수와 매우 유사하다. 타입 주석, 기본 값 및 오버로드와 함께 매개변수를 추가할 수 있다.

				class Point {
					x: number;
					y: number;
					
					// 기본 값이 있는 일반적인 시그니처
					constructor(x = 0, y = 0) {
						this.x = x;
						this.y = y;
					}
				}

				class Point {
					// 오버로드
					constructor(x: number, y: string);
					constructor(s: string);
					constructor(xs: any, y: any) {
						// TBD
					}
				}


		클래스 생성자 시그니처와 함수 시그니처 간에는 몇 가지 차이가 있다.

			- 생성자는 타입 매개변수를 가질 수 없다. 이러한 매개변수는 나중에 배울 외부 클래스 선언에 속한다.
			- 생성자에는 반환 타입 주석을 붙일 수 없다. 클래스 인스턴스 타입이 항상 반환되기 때문이다.


		슈퍼 호출

			자바스크립트와 마찬가지로 기본 클래스가 있는 경우, this 멤버를 사용하기 전에 constructor 본문에서 super()를 호출해야 한다.

				calss Base {
					k =4;
				}

				class Derived extends Base {
					constructor() {
						// ES5에서 잘못된 값을 출력하고, ES6에서 예외를 발생시킨다.
						console.log(this.k);

				'super' must be called before accessing 'this' in the constuctor of a derived class.
						super();
					}
				}

			슈퍼를 호출하는 것을 잊는 것은 자바스크립트에서 쉽게 발생할 수 있는 실수이다.
			그러나 TypeScript는 이를 필요로 할 때 알려준다.


		메서드

			Background Reading:
			메서드 정의
			클래스의 함수 속성을 메서드라고 한다. 메서드는 함수와 생성자와 동일한 타입 주석을 사용할 수 있다.

				class Point {
					x = 10;
					y = 10;

					scale(n: number): void {
						this.x *= n;
						this.y *= n;
					}
				}

			표준 타입 주석 이외에 TypeScript는 메서드에 새로운 것을 추가하지 않는다.

			메서드 본문 내부에서는 여전히 필드와 다른 메서드에 this를 통해 접근해야 한다.
			메서드 본문에서 자격 없는 이름은 항상 둘러싸는 범위에 있는 것을 참조한다.

				let x: number = 0;

				class C {
					x: string = "hello";

					m() {
						// 이것은 클래스 속성이 아닌 line1에서 'x'를 수정하려고 한다.
						x = "world";

				type 'string' is not assignable to type 'number'.
					}
				}


		Getters / Setters

			클래스에는 다음과 같은 접근자가 있을 수도 있다.

				class C {
					_length = 0;
					get length() {
						return this._length;
					}
					set length(value) {
						this._length = value;
					}
				}

				get/set이 별도의 로직이 없는 필드를 지원하는 것은 JavaScript에서 거의 사용되지 않는다.
				get/set 작업 중에 추가적인 로직을 추가할 필요가 없는 경우에는 공개 필드를 노출하는 것이 괜찮다.

			TypeScript에서는 접근자에 대해 특별한 추론 규칙이 있다.

				- get이 존재하지만 set이 없는 경우, 속성은 자동으로 읽기 전용이다.
				- setter 매개변수의 타입이 명시되지 않은 경우, 이는 getter의 반환 타입에서 유추된다.
				- getter와 setter는 동일한 멤버 가시성을 가져야 한다.
				- TypeScript 4.3부터는 가져오기와 설정하기 위한 서로 다른 타입을 가진 접근자를 가질 수 있다.

				class Thing {
					_size: 0;

					get size(): number {
						return this._size;
					}

					set size(value: string | number | boolean) {
						let num = Number(value);

						// NaN, Infinity 등을 허용하지 않는다.

						if (!Number.isFinite(num)) {
							this._size = 0;
							return;
						}

						this._size = num;
					}
				}


		인덱스 시그니처

			클래스는 인덱스 시그니처를 선언할 수 있다.
			이것들은 다른 객체 유형의 인덱스 시그니처와 동일하게 작동한다.

				class MyClass {
					[s: string]: boolean | ((s: string) => boolean);

					check(s: stirng) {
						return this[s] as boolean;
					}
				}

			인덱스 시그니처 유형은 메서드의 유형도 캡처해야 하기 때문에 이러한 유형을 유용하게 사용하는 것은 쉽지 않다.
			일반적으로 클래스 인스턴스 자체에 인덱스화된 데이터를 저장하는 것보다 다른 위치에 저장하는 것이 좋다.


		클래스 상속

			자바스크립트의 클래스는 객체 지향 가능을 가진 다른 언어와 마찬가지로 기본 클래스에서 상속할 수 있다.

			implements 절

				implements 절을 사용하여 클래스가 특정 인터페이스를 올바르게 구현하는지 확인할 수 있다.
				클래스가 올바르게 구현되지 않으면 오류가 발생한다.

				interface Pingable {
					ping(): void;
				}

				class Sonar implements Pingable {
					ping() {
						console.log("ping!");
					}
				}

				class Ball implements Pingable {

				Class 'Ball' incorrectly implements interface 'Pingable'.
					Property 'ping' is missing in type 'Ball' but required in type 'Pingable'.

					pong() {
						console.log("pong!")
					}
				}

			클래스는 여러 인터페이스도 구현할 수 있다. 예를 들어 class C implement A, B {...}.

				주의 사항

					implements 절은 클래스를 인터페이스 유형으로 처리할 수 있는지 확인하는 것일 뿐이라는 점을 이해하는 것이 중요하다.
					이것은 클래스 또는 그 메서드의 유형을 전혀 변경하지 않는다. 일반적인 오류 원인은 implements 절이 클래스 유형을 변경할 것이라고 가정하는 것이다. - 그렇지 않다!

					interface Checkable {
						check(name: string): boolean;
					}

					class NmaeChecker implements Checkable {
						check(s) {
					Parameter 's' implicitle has an 'any' type.

							// 여기에 오류가 없음을 알 수 있다.
							return s.toLowerCase() === "OK";
						}
					}

				이 예제에서는 아마도 s의 유형이 check의 name 매개변수에 영향을 받을 것으로 예상할 수 있다.
				- 그렇지 않다. implements 절은 클래스 본문의 확인 방법이나 유추된 유형을 변경하지 않는다.

				마찬가지로, 옵션 속성이 있는 인터페이스를 구현해도 해당 속성을 만들지 않는다.

					interface A {
						x: number;
						y?: number;
					}

					class C implements A {
						x = 0;
					}

					const c = new C();
					c.y = 10;

					Property 'y' dose not exist on type 'C'.


		extends 절

			class Animal {
				move() {
					console.log("Moving along!");
				}
			}

			class Dog extends Animal {
				woof(times: number) {
					for (let i = 0; i < times; i++) {
						console.log("woof!")
					}
				}
			}

			const d = new Dog();
			// 베이스 클래스
			d.move();
			// 파생수업방식
			d.move(3);


			오버라이딩 메서드

				파생 클래스는 기본 클래스의 필드나 속성을 재정의할 수도 있다.
				super 구문을 사용하여 기본 클래스의 메서드에 접근할 수 있다. JavaScript 클래스는 단순한 조회 객체이기 때문에 "슈퍼 필드"라는 개념이 없다.

				TypeScript는 파생 클래스가 항상 기본 클래스의 하위 타입이 되도록 강제한다.

				예를 들어, 다음은 메서드를 재정의하는 합법적인 방법이다.

					class Base {
						greet() {
							console.log("Hello, world");
						}
					}

					class Derived extends Base {
						greet(name?: string) {
							if (name === undefined) {
								super.greet();
							} else {
								console.log(`Hello, ${name.toUpperCase()}`);
							}
						}
					}

					const d = new Derived();
					d.greet();
					d.greet("reader");

				파생 클래스가 기본 클래스 계약을 따르는 것이 중요하다. 기본 클래스 참조를 통해 파생 클래스 인스턴스를 참조하는 것은 매우 일반적이며(항상 합법적이다.) 중요하다.

					// 기본 클래스 참조를 통해 파생 클래스 인스턴스를 참조하는 것을 별칭(alias)을 사용한다.
					const b: Base = d;
					// 문제 없다.
					b.greet();

				만약 파생 클래스가 기본 클래스의 계약을 따르지 않는다면 어떻게 될까?

					class Base {
						greet() {
							console.log("Hellow world");
						}
					}

					class Derived extends Base {
						// 이 매개 변수를 필수로 지정한다.
						greet(name: string) {

					Property 'greet' in type 'Derived' is not assignable to the same property in base type 'Base'.
						Type '(name: stirng) => void' is not assignable to type '() => void'.
							Target signature provides too few arguments. Expected 1 or more, but got 0.

					Derived의 'greet' 속성이 Base의 동일한 속성에 할당할 수 없다. 
						'(name: string) => void' 타입은 '() => void' 타입에 할당할 수 없다. 
							이유는 목표 서명이 1개 이상의 인수를 제공하도록 요구하지만, 여기서는 0개의 인수를 받았기 때문이다.

							console.log(`Hellow, ${name.toUpperCase()}`);
						}
					}

				이 코드를 오류 없이 컴파일한다면, 다음 예제가 실행 중에 충돌할 것이다.

					const b: Base = new Derived();
					// "name"이 정의되지 않기 때문에 출돌한다.
					b.greet();


			유형 전용 필드 선언(Type-only Field Declarations)

				타겟이 ES2022 이상이거나 useDefineForClassFields가 ture로 설정된 경우, 클래스 필드는 부모 클래스 생성자가 완료된 후에 초기화된다.
				이는 상속된 필드에 대해 더 정확한 타입을 다시 선언하려는 경우 문제가 될 수 있다.
				이러한 경우를 처리하기 위해 TypeScript에게 이 필드 선언에 대해 런타임 영향이 없어야 함을 나타내는 declare를 작성할 수 있다.

					interface Animal {
						dateOfBirth: any;
					}

					interface Dog extends Animal {
						breed: any;
					}

					class AnimalHouse {
						resident: Animal;
						constructor(animal: Animal) {
							this.resident = animal;
						}
					}

					class DogHouse extends AnimalHouse {
						// JavaScript 코드를 생성하지 않고,
						// 타입만 올바른지 확인한다.
						declare resident: Dog;
						constructor(dog: Dog) {
							super(dog);
						}
					}


			초기화 순서

				클래스 초기화 순서는 경우에 따라 놀라울 수 있다. 다음 코드를 살펴보자.

					class Base {
						name = "base";
						constructor() {
							console.log("My name is " + this.name);
						}
					}

					class Derived extends Base {
						name = "derived";
					}

					// "derived"가 아닌 "base"를 출력한다.
					const d = new Derived();

				JavaScript에서 정의한 클래스 초기화 순서는 다음과 같다.

				1 기본 클래스 필드가 초기화 된다.
				2 기본 클래스 생성자가 실행된다.
				3 파생된 클래스 필드가 초기화된다.
				4 파생된 클래스 생성자가 실행된다.

				즉, 기본 클래스 생성자는 자신의 생성자 동안 자신의 name 값을 본 것이다. 왜냐하면 파생된 클래스 필드 초기화가 아직 실행되지 않았기 때문이다.


			기본 제공 유형 상속(Inheriting Built-in Types)

				참고: Array, Error, Map 등의 내장형에서 상속할 계획이 없거나 컴파일 대상이 ES6/ES2015 이상으로 명시적으로 설정되어 있는 경우 이 섹션을 건너뛸 수 있다.

				ES2015에서 객체를 반환하는 생성자 super(...)의 호출자를 암묵적으로 이 값으로 대체한다.
				생성된 생성자 코드는 super(...)의 잠재적인 반환 값을 캡처하여 이 값으로 대체하는 것이 필요하다.

				따라서 Error, Array 등을 하위 클래스화하는 것이 더 이상 예상대로 작동하지 않을 수 있다.
				Error, Array 등에 대한 생성자 함수가 ECMAScript 6의 new.target을 사용하여 프로토타입 체인을 조정하기 때문이다.
				그러나 ECMAScript 5에서 생성자를 호출할 때 new.target에 대한 값을 보장할 방법이 없다. 다른 다운레벨 컴파일러도 기본적으로 동일한 제한이 있다.

				다음과 같은 하위 클래스의 경우

					class MsgError extends Error {
						constructor(m: string) {
							super(m);
						}
						sayHello() {
							return "hello " + this.message;
						}
					}

				여기에는 두 가지 문제가 있을 수 있다.

					1 이러한 하위 클래스를 구성하는 객체의 경우 메서드가 정의되지 않을 수 있으므로 sayHello를 호출하면 오류가 발생할 수 있다.
					2 하위 클래스와 해당 인스턴스 간에 instanceof가 제대로 작동하지 않을 수 있으므로(new MsgError()) instanceof MsgError 가 false를 반환할 수 있다.

				권장사항으로, super(...) 호출 후에는 수동으로 프로토타입을 즉시 조정하는 것이 좋다.

				MsgError의 하위 클래스는 또한 프로토타입을 수동으로 설정해야 한다. Object.setPrototypeOf를 지원하지 않는 런타임의 경우 __proto__를 대신 사용할 수도 있다.
				그러나 이러한 해결책은 Internet Explorer 10 및 이전 버전에서는 작동하지 않는다. 프로토타입 체인 자체를 수정할 수는 없지만, 프로토타입에서 메서드를 수동으로 인스턴스로 복사할 수 있다.
				(this에서 MsgError.prototype으로)


		멤버 가시성(Member Visibility)

			TypeScript를 사용하여 클래스 외부 코드에서 특정 메서드나 속성이 보여지는 여부를 제어할 수 있다.


			public

				클래스 멤버의 기본 가시성은 public이다. public 멤버는 어디에서든 엑세스할 수 있다.

					class Greeter {
						public greet() {
							console.log("hi!");
						}
					}
					const g = new Greeter();
					g.greet();

				public은 이미 기본 가시성 한정자이기 때문에 클래스 멤버에 대해 이를 작성할 필요는 없지만, 스타일이나 가독성을 위해 작성할 수도 있다.


			protected

				protected 멤버는 선언된 클래스의 하위 클래스에서만 볼 수 있다.

					class Greeter {
						public greet() {
							console.log("Hello, " + this.getName());
						}
						protected getName() {
							return "hi";
						}
					}

					class SpecialGreeter extends Greeter {
						public howdy() {
							// 여기서 보호된 멤버에 엑세스한다.
							console.log("Howdy, " + this.getName());
						}
					}

					const g = new SpecialGreeter();
					g.greet(); // OK
					g.getName();
					Property 'getName' is protected and only accessible within class 'Greeter' and its subclasses.


				보호되는 멤버의 노출(Exposure of protected members)

					하위 클래스는 기본 클래스 계약을 따라야 하지만, 더 많은 기능을 갖춘 기본 클래스의 하위 유형을 노출시킬 수도 있다.
					이는 protected 멤버를 public으로 공개하는 것을 포함한다.

						class Base {
							protected m = 10;
						}
						class Derived extends Base {
							// 수식어가 없으므로 기본값은 public이다.
							m = 15;
						}
						const d = new Derived();
						console.log(d.m); // OK

					Derived가 이미 m을 자유롭게 읽고 쓸 수 있었기 때문에 이 작업은 상황의 "보안"을 의미적으로 변경하지 않는다.
					여기서 주목해야 할 주요 사항은 파생된 클래스에서 이러한 노출이 의도적이지 않은 경우 protected 수정자를 반복해서 사용해야한다는 것이다.


				계층 간 보호된 엑세스(Cross-hierarchy protected access)

					다른 OOP언어는 기본 클래스 참조를 통해 protected 멤버에 엑세스 하는 것이 합법적인지에 대해 의견이 분분하다.

						class Base {
							protected x: number = 1;
						}
						class Derived1 extends Base {
							protected x: number = 5;
						}
						class Derived2 extends Base {
							f1(other: Derived2) {
								other.x = 10;
							}
							f2(other: Derived2) {
								other.x = 10;
						Property 'x' is protected and only accessible within class 'Derived1' and its subclasses.
							}
						}
						
					예를 들어 Java는 이를 합법적인 것으로 간주한다. 반면에 C# 및 C++은 이 코드가 불법적이어야 한다고 선택했다.

					TypeScript는 여기서 C# 및 C++과 같은 입장을 취하는데, Derived에서 x에 엑세스하는 것은 Derived2의 하위 클래스에서만 허용되어야 하며, Derived1은 하위 클래스가 아니다.
					게다가 Derived1 참조를 통해 x에 엑세스 하는 것이 불법적이라면 (확실히 그렇게여야 한다) 기본 클래스 참조를 통해 엑세스 하는 것이 상황을 개선시키지 않아야 한다.


			private

				private는 protected와 비슷하지만 하위 클래스에서도 멤버에 엑세스할 수 없다.

					class Base {
						private x = 0;
					}
					const b = new Base();
					// 클래스 외부에서 액세스할 수 없음
					console.log(b.x);
					Property 'x' is private and only accessible within class 'Base'.

					class Derived extends Base {
						showX() {
							// 하위 클래스에서 액세스할 수 없음
							console.log(this.x);
					Propterty 'x' is private and only accessible within class 'Base'.
						}
					}

				private 멤버는 파생된 클래스에서 보이지 않기 때문에 파생된 클래스에서 가시성을 높일 수 없다.

					class Base {
						private x = 0;
					}
					class Derived extends Base {
					Class 'Derived' incorrectly extends base class 'Base'.
						Property 'x' is private in type 'Base' but not in type 'Derived'.
						x = 1;
					}

				Cross-instance private access

					다양한 객체지향 프로그래밍 언어들은 동일한 클래스의 다른 인스턴스가 서로의 private 멤버에 접근할지 여부에 대해 의견이 분분하다.
					Java, C#, C++, Swift 및 PHP와 같은 언어들은 이를 허용하지만 Ruby는 허용하지 않는다.

					TypeScript는 인스턴스 간에 private 멤버에 접근할 수 있다.

						class A {
							private x = 10;

							public sameAs(other: A) {
								// No error
								return other.x === this.x;
							}
						}

				주의사항

					타입스크립트의 타입 시스템의 다른 측면들과 마찬가지로, private와 protected는 타입 체크 중에만 강제된다.

					이는 JavaScript 런타임 구조인 in 이나 간단한 속성 조회와 같은 것들이 여전히 private나 protected 멤버에 접근할 수 있다는 것을 의미한다.

						class MySafe {
							private secretKey = 12345;
						}

						// 자바스크립트 파일에서
						const s = new MySafe();
						// 12345를 출력할 것이다.
						console.log(s.secretKey);

					private 키워드를 사용한 경우, 타입 체크 중에도 대괄호 표기법을 사용하여 접근할 수 있다.
					이렇게 함으로써 private로 선언된 필드를 유닛 테스트와 같은 작업에서 더 쉽게 접근할 수 있게 된다.
					그러나 이러한 방법은 필드가 소프트한 private가 되어 엄격한 privacy를 강제하지 않는다는 단점이 있다.

						class MySafe {
							private secretKey = 12345;
						}

						const s = new MySafe();

						// 유형 확인 중에는 허용되지 않음
						console.log(s.secretKey);
						Property 'secretKey' is private and only accessible within class 'MySafe'.

						// OK
						console.log(s["secretKey"]);

					TypeScript의 private와는 달리 JavaScript의 private 필드(#)는 컴파일 후에도 계속해서 private 상태를 유지하며, 이전에 언급한 대괄호 표기법 접근과 같은 탈출구를 제공하지 않는다.
					이러한 특성으로 인해 JavaScript의 private 필드는 엄격한 privacy를 제공하는 'hard private'로 볼 수 있다.

						class Dog {
							#barkAmount = 0;
							personality = "happy";

							constructor() {}
						}

						"use strict";
						class Dog {
							#barkAmount = 0;
							personality = "happy";
							constuctor() { }
						}

					ES2021 이하로 컴파일할 때 TypeScript는 # 대신 WeakMaps을 사용한다.

						"use strict"
						var _Dog_barkAmount;
						class Dog {
							constructor() {
								_Dog_barkAmount.set(this, 0);
								this.personality = "happy";
							}
						}
						_Dog_barkAmount = new WeakMap();

					악의적인 사용자로부터 클래스 내의 값을 보호해야 하는 경우, 클로저, WeakMaps 또는 private 필드와 같은 런타임에서 엄격한 개인 정보 보호를 제공하는 매커니즘을 사용해야 한다.
					런타임에서 추가된 개인 정보 보호 검사는 성능에 영향을 줄 수 있다.


			Static Members

				클래스에는 정적 멤버(static members)가 있을 수 있다. 이러한 멤버들은 클래스의 특정 인스턴스와 관련되지 않는다. 
				대신에 클래스 생성자 객체를 통해 접근할 수 있다.

					class MyClass {
						static x = 0;
						static printX() {
							console.log(MyClass.x);
						}
					}
					console.log(MyClass.x);
					MyClass.printX();

				정적 멤버도 동일한 public, protected 및 private 가시성 수정자를 사용할 수 있다.

					class MyClass {
						private static x = 0;
					}
					console.log(MyClass.x);
					Property 'x' is private and only accessible within class 'MyClass'.

				정적 멤버도 상속된다.

					class Base {
						static getGreeting() {
							return "Hello wordl";
						}
					}
					class Derived extends Base {
						myGreeting = Derived.getGreeting();
					}


				특별한 정적 이름들(Special Static Names)

					특정 정적 이름들은 Function 프로토타입의 속성을 덮어쓰는 것이 안전하지 않거나 불가능할 수 있다.
					클래스 자체가 new로 호출될 수 있는 함수이므로 name, length, call과 같은 함수 속성은 정적 멤버로 정의할 수 없다.

						class S {
							static name = "S!";
						Static property 'name' confilcts with built-in property 'Function.name' of constructor function 'S'.
						}


				정적 클래스가 없는 이유는 무엇일까?

					TypeScript (그리고 JavaScript)에는 예를 들어 C#에서처럼 정적 클래스(static class)라는 구조가 없다.
					이러한 구조들은 해당 언어에서 모든 데이터와 함수를 클래스 내부에 강제로 넣기 때문에 존재한다.
					그러나 TypeScript에서는 이러한 제한이 없기 때문에 이러한 구조가 필요하지 않다.

					예를 들어, TypeScript에서는 "static class"구문이 필요하지 않다. 일반 객체(또는 최상위 함수)가 동일한 작업을 수행할 수 있다.

						// 불필요한 "static" 클래스
						class MyStaticClass {
							static doSomething() {}
						}

						// 선호 대안 1
						function doSomething() {}

						// 선호 대안 2
						const MyHelperObject() {
							dosomething() {},
						}


		정적인 클래스의 블록(static Blocks in Classes)

			static 블록을 사용하면 자체 스코프를 가진 문장 시퀀스를 작성할 수 있으며, 이를 통해 포함된 클래스 내부의 private 필드에 엑세스할 수 있다.
			이는 문자을 작성하는 모든 기능과 변수 누출 없이 클래스의 내부에 완전한 엑세스 권한을 가진 초기화 코드를 작성할 수 있음을 의미한다.

				class Foo {
					static #count = 0;

					get count() {
						return Foo.#count;
					}

					static {
						try {
							const lastInstances = loadLastInstances();
							Foo.#count += lastInstances.length;
						}
						catch {}
					}
				}


		제네릭 클래스(Generic Classes)

			클래스는 인터페이스와 마찬가지로 제네릭할 수 있다. 제네릭 클래스가 new로 인스턴스화될 때, 그 타입 매개변수는 함수 호출과 동일한 방식으로 추론된다.

				class Box<Type> {
					contents: Type;
					constructor(value: Type) {
						this.contents = value;
					}
				}

				const b = new Box("hello!");

					const b: Box<string>

			클래스도 인터페이스와 마찬가지로 제네릭 제약 조건과 기본값을 사용할 수 있다.


			정적 부재의 매개 변수 입력

				이 코드는 유효하지 않다. 그 이유가 명확하지 않을 수도 있다.

					class Box<Type> {
						static defaultValue: Type;
					Static members cannot reference class type parameter.
					}

				타입은 항상 완전히 삭제된다. 실행 중에는 Box.defaultValue 속성 슬롯이 하나뿐이다.
				이는 Box<string>.defaultValue를 설정하면 Box<number>.defaultValue도 변경된다는 것을 의미한다.
				일반 클래스의 정적 멤버는 클래스의 형식 매개변수를 참조할 수 없다.


		클래스 런타임의 this

			JavaScript에서의 this 처리는 실제로 특이할 수 있다. JavaScript에서의 this는 실행 컨텍스트에 따라 동적으로 결정된다.
			함수가 호출될 때 this는 호출 방법에 따라 결정되며, 일반적으로는 호출한 객체를 가리킨다. 
			그러나 함수가 콜백 함수로 사용될 때나 객체의 메서드로 사용될 때와 같이 호출 방식이 달라질 수 있다.
			이로 인해 this의 동작이 예상치 못하게 변할 수 있으며, 이것이 종종 개발자들 사이에서 혼란을 일으키는 원인 중 하나이다.
			TypeScript는 JavaScript의 이러한 특이한 동작을 변경하지 않으며, JavaScript의 동작을 그대로 유지한다.

				class MyClass {
					name = "MyClass";
					getName() {
						return this.name;
					}
				}
				const c = new MyClass();
				const obj = {
					name: "obj",
					getName: c.getName;
				}

				// "obj"출력
				console.log(obj.getName());

			길게 설명하면, 기본적으로 함수 내부의 this 값은 함수가 호출된 방식에 따라 달라진다. 이 예제에서 함수가 obj 참조를 통해 호출되었기 때문에 this의 값은 클래스 인스턴스가 아닌 obj가 된다.

			이런 경우가 거의 바람직하지 않은 경우가 많다 TypeScript는 이러한 종류의 오류를 완화하거나 방지하기 위한 몇 가지 방법을 제공한다.


			화살표 함수

				만약 자주 호출될 함수가 this 컨텍스트를 잃어버리는 경우, 메서드 정의 대신 화살표 함수 속성을 사용하는 것이 좋을 수 있다.

					class MyClass {
						name = "MyClass";
						getName = () => {
							return this.name;
						};
					}
					const c = new MyClass();
					const g = c.getName;
					// "MyClass" 출력
					console.log(g());

				이 방법으로 정의된 각 함수마다 클래스 인스턴스가 별도의 복사본을 가지기 때문에 더 많은 메모리를 사용한다.
				하지만 런타임에서 this 값은 TypeScript로 확인되지 않은 코드에도 올바르게 보장된다.
				하지만 이 방법을 사용할 경우, 파생 클래스에서 super.getName을 사용할 수 없다.
				왜냐하면 프로토타입 체인에 기본 클래스 메서드를 가져올 항목이 없기 때문이다.


				this 파라미터

					메서드나 함수 정의 내에서 this 라는 초기 매개변수는 TypeScript에서 특별한 의미를 갖는다. 이러한 매개변수는 컴파일 중에 삭제된다.

						// 'this' 매개 변수로 Script 입력
						function fn(this: SomeType, x: number) {
							// ...
						}

						// JavaScript output
						function fn(x) {
							// ...
						}

					TypeScript는 함수로 호출할 때 this 매개변수가 올바른 컨텍스트로 전달되는지를 확인한다.
					화살표 함수 대신에, 메서드 정의 this 매개변수를 추가하여 메서드가 올바르게 호출되는지를 정적으로 강제할 수 있다.

						class Myclass {
							name = "MyClass";
							getName(this: MyClass) {
								return this.name;
							}
						}
						const c = new MyClass();
						// OK
						c.getName();

						// Error
						const g = c.getName;
						console.log(g());
						The 'this' context of type 'void' is not assignable to method's 'this' of type 'MyClass'.

					이 방법은 화살표 함수 접근 방식과 정반대의 트레이드오프를 제공한다.

					- JavaScript 호출자들은 실수로 클래스 메서드를 여전히 잘못 사용할 수 있다.
					- 클래스 정의당 하나의 함수만 할당되며 클래스 인스턴스당 하나씩이 아니다.
					- 기본 메서드 정의는 여전히 super를 통해 호출할 수 있다.


		this Types

			클래스 내부에서, this라는 특별한 타입은 현재 클래스의 타입을 동적으로 참조한다. 이것이 어떻게 유용한지 살펴보자.

				class Box {
					contents: string = "";
					set(value: string) {

						(method) Box.set(value: string): this

						this.contents = value;
						return this;
					}
				}

			여기서 TypeScript는 set의 반환 타입은 Box 대신 this로 추론했다. 이제 Box의 하위 클래스를 만들어 보자.

				class ClearbleBox extends Box {
					clear() {
						this.contents = "";
					}
				}

				const a = new ClearbleBox();
				const b = a.set("hello");

					const b: ClearbleBox

			여기서 TypeScript는 set의 반환 타입을 Box 대신 this로 추론했다. 이제 Box의 하위 클래스를 만들어 보자.

				class Box {
					contens: stirng = "";
					sameAs(other: this) {
						return other.content === this.content;
					}
				}

			이것은 다른 것을 Box로 작성하는 것과 다르다. - 파생 클래스가 있는 경우, 그것의 sameAs 메서드는 이제 해당 파생 클래스의 다른 인스턴스만 수락한다.

				class Box {
					content: string = "";
					sameAs(other: this) {
						return other.content === this.content;
					}
				}

				class DerivedBox extends Box {
					otherContent: string = "?";
				}

				const base = new Box();
				const derived = new DerivedBox();
				derived.sameAs(base);
				Argument of type 'Box' is no assignable to parameter of type 'DerivedBox'.
					Property 'otherContent' is missing in type 'Box' but required in type 'DerivedBox'.


		this를 기반으로 하는 타입 가드

			클래스나 인터페이스의 메서드에서 반환 위치에 this 타입으로 사용할 수 있다.
			이것을 타입 좁힘과 함께 사용하면(예: if문), 대상 객체의 타입이 지정된 타입으로 좁혀진다. 이렇게 하면 해당 객체의 타입을 특정 조건에 따라 좁혀서 사용할 수 있다.

				class FileSystemObject {
					isFile(): this is FileRep {
						return this instanceof FileRap;
					}
					isDirectory(): this is Directory {
						return this instanceof Directory;
					}
					isNetworked(): this is Networked & this {
						return this.networked;
					}
					constructor(public path: stirng, private networked: boolean) {}
				}

				class FileRep extends FileSystemObject {
					constructor(path: string, public content: string) {
						super(path, false);
					}
				}

				class Directory extends FileSystemObject {
					children: FileSystemObject[];
				}

				interface Networked {
					host: string;
				}

				const fso: FileSystemObject = new FileRep("foo/bar.txt", "foo");

				if (fso.isFile()) {
					fso.content;

						const fso: FileRep
				} else if (fso.isDirectory()) {
					fso.children

						const fso: Directory
				} else if (fso.isNetworked()) {
					fso.host;

						const fso: Networked & FileSystemObject
				}

			this를 기반으로 한 타입 가드의 일반적인 사용 사례 중 하나는 특정 필드의 지연 유효성 검사를 허용하는 것이다.
			예를 들어, 이 경우에는 hasValue가 true로 확인된 경우 box 내부에 보관된 값에서 undefined를 제거한다.


		파라미터 속성

			TypeScript에는 생성자 매개변수를 동일한 이름과 값으로 클래스 속성을 ㅗ변환하는 특별한 구문이 있다.
			이를 매개변수 속성이라고하며 생성자 인수 앞에 가시성 수정자 public, private, protected 또는 readonly 중 하나를 접두사로 붙여서 생성된다.
			결과 필드는 해당 수정자를 가져온다.

				class Params {
					consturctor(
						public readonly x: number,
						protected y: number,
						private: z: number
					) {
						// ...
					}
				}
				const s = new Params(1, 2, 3);
				console.log(a.x);

					(property) Params.x: number

				console.log(a.z)
				Property 'z' is private and only accessible within class 'Params'.


		클래스 표현식

			클래스 표현식은 클래스 선언과 매우 유사하다. 유일한 실제 차이점은 클래스 표현식에는 이름이 필요하지 않지만, 해당 표현식이 묶인 식별자를 통해 참조할 수 있다.

				const someClass = class<Type> {
					content: Type;
					constructor(value: Type) {
						this.content = value;
					}
				};

				const m = new someClass("Hello world");

					const m: someClass<string>


		생성자 시그니처

			자바스크립트 클래스는 new 연산자를 사용하여 인스턴스화된다. 클래스 자체의 타입을 감안할 때 instanceType 유틸리티 타입은 이 작업을 모델링한다.

				class Point {
					createeAt: number;
					x: number;
					y: number
					constructor(x: number, y: number) {
						this.createdAt = Date.now()
						this.x = x;
						this.y = y;
					}
				}
				type PointInstance = InstanceType<typeof Point>

				fnction moveRigth(point: PointInstance) {
					point.x += 5;
				}

				const point = new Point(3, 4);
				moveRight(point);
				point.x; // => 8


		abstract 클래스와 멤버

			타입스크립트의 클래스, 메서드 및 필드는 추상일 수 있다.

			추상 메서드 또는 추상 필드는 구현이 제공되지 않은 것이다. 이러한 멤버들은 직접적으로 인스턴스화될 수 없는 추상 클래스 내에 존재해야 한다.

			추상 클래스의 역할은 모든 추상 멤버를 구현하는 하위 클래스의 기본 클래스로서 역할이다.
			클래스에 추상 멤버가 없을 때 그것은 구체적이라고 한다.

			예를 들어,

				abstract class Base {
					abstract getName(): string;

					printName() {
						console.log("Hello, " + this.getName());
					}
				}

				const b = new Base();
				Cannot create an istance of an abstract class.

			Base를 새로운 키워드로 인스턴스화할 수 없다. 왜냐하면 이것은 추상 클래스이기 때문이다.
			대신, 파생 클래스를 만들고 추상 멤버를 구현해야 한다.

				class Derived extends Base {
					getName() {
						return "world";
					}
				}

				const d = new Derived();
				d.printName();

			추상 클래스의 추상 멤버를 구현하는 것을 잊으면 오류가 발생한다.

				class Derived extend Base {
				Non-abstract class 'Derived' dose not implement all abstract members of 'Base'
				}


			추상 생성자 시그니처

				어떤 추상 클래스에서 파생된 클래스의 인스턴스를 생성하는 클래스 생성자 함수를 받아들이고 싶을 때가 있다.

				예를 들어, 다음과 같은 코드를 작성하고 싶을 수 있다.

					function greet(ctor: typeof Base) {
						const instance = new ctor();
					Cannot create an instance of an abstract class.
						instance.printName();
					}

				TypeScript가 올바르게 말하고 있다. 추상 클래스를 인스턴스화하려고 하고 있다는 것이다.
				결국 greet의 정의를 고려하면 다음과 같은 코드를 작성할 수 있다. 이 코드는 추상 클래스를 생성하게 된다.

					// Bad
					greet(Base);

				대신, 생성자 시그니처를 갖는 것을 받는 함수를 작성하려고 한다.

					function greet(ctor: new () => Base) {
						const instance = new ctor();
						instance.printName();
					}
					greet(Derived);
					greet(Base);
					Argument of type 'typeof Base' is not assignable to parameter of type 'new () => Base'.
						Cannot assign an abstract constructor type to a non-abstact constructor type.

				이제 TypeScript는 생성자 함수를 호출할 수 있는 클래스에 대해 올바르게 알려준다.
				Derived는 구체적이기 때문에 호출할 수 있지만, Base는 호출할 수 없다.


		클래스간의 관계

			대부분의 경우 TypeScript에서 클래스는 다른 타입과 구조적으로 비교된다. 예를 들어, 이 두 클래스는 동일하므로 서로 대신 사용할 수 있다.

				class Point1 {
					x = 0;
					y = 0;
				}

				class Point2 {
					x = 0;
					y = 0;
				}

				// OK
				const p: Point1 = new Point2();

			마찬가지로, 명시적인 상속이 없더라도 클래스 간의 하위 유형 관계가 존재한다.

				class Person {
					name: string;
					age: number;
				}

				class Employee {
					name: string;
					age: number;
					salary: number;
				}

				// OK
				const p: Person = new Employee();

			이것은 직관적으로 보이지만, 다른 경우보다 더 이상한 몇 가지 경우가 있다.

				빈 클래스는 멤버가 없다. 구조적인 유형 시스템에서는 멤버가 없는 유형이 일반적으로 다른 모든 것의 상위 유형이다.
				따라서 빈 클래스를 작성하면(하면 안된다.) 아무 것이나 해당 위치에 사용할 수 있다.

					class Empty {}

					function fn(x: Empty) {}

					// All OK
					fn(window);
					fn({});
					fn(fn);

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
