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

				
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
