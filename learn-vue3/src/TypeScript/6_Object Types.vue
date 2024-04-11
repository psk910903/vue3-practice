<template>
	<div></div>
</template>
<!--  

	Object Types

		JavaScript에서 데이터를 그룹화하고 전달하는 기본 방법은 객체를 통해 이루어진다. TypeScript에서는 객체 타입을 통해 이러한 객체를 표현한다.
		이러한 객체는 익명일 수 있다.

			function greet(person: { name: string; age: number }) {
				return "Hello " + person.name;
			}

		또는 인터페이스를 사용하여 이름을 지정할 수 있다.

			interface Person {
				name: string;
				age: number;
			}

			function greet(person: Person) {
				return "Hello " + person.name;
			}

		또는 타입 별칭을 사용할 수도 있다.

			type Person = {
				name: string;
				age: number;
			};

			function greet(person: Person) {
				return "Hello " + person.name;
			}

		위의 세 예제 모두에서 우리는 name 속성(string 이여야 함)과 age 속성(number 이여야 함)을 포함하는 객체를 인자로 사용하는 함수를 작성했다.


		Quick Reference(빠른 참조)

			우리에게는 주요 일상 구문을 한눈에 살펴볼 수 있는 타입과 인터페이스에 대한 치트 시트가 있다.


		Property Modifiers(속성 수정자)

			객체 타입의 각 속성은 몇 가지를 지정할 수 있다. 타입, 속성이 선택적인지 여부 및 속성을 쓸 수 있는지 여부이다.


			Optional Properties(옵셔널 속성)

				대부분의 경우, 속성이 설정된 객체와 작업해야 할 것이다. 이러한 경우에는 속성 이름 끝에 물음표(?)를 추가하여 해당 속성을 선택적으로 표시할 수 있다.

					interface PaintOptions {
						shape: Shape;
						xPos?: number;
						yPos?: number;
					}

					function paintShape(opts: PaintOptions) {
						// ...
					}

					const shape = getShape();
					paintShape({ shape });
					paintShape({ shape, xPos: 100 });
					paintShape({ shape, yPos: 100 });
					paintShape({ shape, xPos: 100, yPos: 100 });

				이 예제에서 xPos와 yPos는 모두 선택적으로 간주된다. 두 속성 중 하나를 제공할 수 있으므로 paintShape에 대한 위의 모든 호출이 유효하다.
				모든 선택적 속성이 실제로 하는 일은 속성이 설정되면 특정 타입을 가져야 한다는 것이다.

				또한 이러한 속성에서 읽을 수도 있다. 그러나 엄격한 null 확인 아래에서 이를 수행하면 TypeScript가 이들이 잠재적으로 정의되지 않았다고 알려준다.

					function paintShape(opts: PaintOptions) {
						let xPos = opts.xPos;

							(property) PaintOptions.xPos?: number | undefined

						let yPos = opts.yPos;

							(property) PaintOptions.xPos?: number | undefined

							//...
					}

				JavaScript에서는 속성이 한 번도 설정되지 않았더라도 여전히 엑세스할 수 있다. 단지 값을 undefined로 반환할 뿐이다.
				우리는 그것을 확인함으로써 undefined를 처리할 수 있다.

					function paintShape(opts: PaintOptions) {
						let xPos = opts.xPos === undefined ? 0 : opts.xPos;

							let xPos: number

						let yPos = opts.yPos === undefined ? 0 : opts.yPos;

							let yPos: number

						// ...
					}

				지정되지 않은 값에 대한 기본값을 설정하는 이 패턴은 너무 흔해서 JavaScript에는 이를 지원하는 구문이 있다.

					function paintShape({ shape, xPos = 0, yPos = 0 }: PaintOptions) {
						console.log("x coordinate at", xPos);

							(parameter) xPos: number

						console.log("y coordinate at", yPos);

							(parameter) yPos: number

						// ...
					}

				여기서는 paintShape의 매개변수에 대한 해체 패턴을 사용하고 xPos와 yPos에 기본값을 제공했다.
				이제 xPos와 yPos는 paintShape의 본문 내에서 확실히 존재하지만 paintShape를 호출하는 모든 호출자에 대해 선택적이다.

					주의할 점은 현재 파괴적인 패턴 내에 타입 주석을 배치하는 방법이 없다는 것이다.
					이것은 구문이 이미 JavaScript에서 다른 의미를 가지고 있기 때문이다.

						function draw({ shape: Shape, xPos: number = 100 /* ... */}) {
							render(shape);

								Cannot find name 'shape'. Did ype mean 'Shape'?

							render(xPos);

								Cannot find name 'xPos'.
						}

					객체 해체 패턴에서 shape: Shape 속성 shape을 가져와서 지역 변수로 다시 정의하고 해당 변수를 Shape으로 지정한다.
					마찬가지로 xPos: number는 xPos 매개변수를 기반으로 값을 가진 변수 number를 생성한다.


				readonly Properties(읽기전용 속성)

					TypeScript에서 속성은 읽기 전용으로 표시될 수도 있다. 
					이것은 런타임 동안 어떤 동작도 변경하지 않지만, readonly로 표시된 속성은 타입 확인 중에 쓰여질 수 없다.

						interface SomeType {
							readonly prop: string;
						}

						function doSomething(obj: SomeType) {
							// obj.prop에서 읽을 수 있다.
							console.log(`prop has the value '$(obj.prop)'.`)

							// 하지만 재할당은 불가능하다.
							obj.prop = "hello";

							Cannot assign to 'prop' because it is a read-only property.
						}

					readonly 수정자를 사용하는 것은 값이 완전히 변경할 수 없다는 것을 반드시 의미하지는 않는다.
					다시 말해, 내부 내용이 변경될 수 없다는 것을 의미하지 않는다. 단지 속성 자체가 다시 쓰일 수 없다는 것을 의미한다.

						interface Home {
							readonly resident: { name: string; age: number };
						}

						function visitForBirthday(home: Home) {
							// home.resident에서 속성을 읽고 업데이트할 수 있다.
							console.log(`Happy birthday $(home.resident.name)!`);
							home.resident.age++;
						}

						function evict(home: Home) {
							// 그러나 'Home'의 'resident' 속성 자체에 쓸 수는 없다.
							home.resident = {

								Cannot assign to 'resident' because it is a read-only property.
								name: "victor the Evictor",
								age: 42,
							}
						}

					readonly가 무엇을 의미하는지에 대한 기대 관리는 중요하다. TypeScript에서 객체를 사용하는 방법에 대한 의도를 개발 시간에 신호로써 전달하는 것이 유용하다.
					TypeScript는 두 타입의 속성이 readonly인지 여부를 확인할 때 해당 타입이 호환되는지 여부를 고려하지 않으므로, readonly 속성은 별칭을 통해 변경될 수도 있다.

						interface Person {
							name: string;
							age: number;
						}

						interface ReadonlyPerson {
							readonly name: string;
							readonly age: number;
						}

						let writablePerson: Person = {
							name: "Person McPersonface",
							age: 42,
						};

						// works
						let readonlyPerson: ReadonlyPerson = writablePerson;

						console.log(readonlyPerson.age); // print '42'
						writablePerson.age++;
						console.log(readonlyPerson.age); // print '43'

					매핑 수정자를 사용하여 readonly 속성을 제거할 수 있다.


				Index Signatures

					가끔은 타입의 모든 속성 이름을 미리 알지 못하지만 값의 형태는 알고 있는 경우가 있다.

					이러한 경우 가능한 값의 타입을 설명하기 위해 인덱스 시그니처를 사용할 수 있다. 예를 들어

						interface StringArray {
							[index: number]: string;
						}

						const myArray: StringArray = getStringArray();
						const secondItem = myArray[1];

							const secondItem: string

					위에서는 index signature를 가진 StringArray 인터페이스가 있다. 이 인덱스 시그니처는 StringArray가 숫자로 색인화될 때 문자열을 반환한다고 명시한다.

					인덱스 시그니처 속성에는 일부 타입만 허용된다. (string, number, symbol, template string  patterns 및 이러한 타입만으로 구성된 유니언 타입.)

					두 타입의 인덱서를 모두 지원하는 것이 가능하다. 숫자 인덱서에서 반환된 타입은 문자열 인덱서에서 반환된 타입의 서브타입이어야 한다.
					왜냐하면 숫자로 인덱싱할 때 JavaScript는 실제로 객체에 색인화하기 전에 해당 값을 문자열로 변환하기 때문이다.
					이것은 숫자로 인덱싱하는 것이 "100"에 색인화하기 전에 해당 값을 문자열로 변환하기 때문이다. 
					이것은 숫자로 인덱싱하는 것이 "100"(문자열)으로 인덱싱하는 것과 동일하다는 것을 의미한다. 따라서 두 가지가 일관되어야 한다.

						interface Animal {
							name: string;
						}

						interface Dog extends Animal {
							bread: string;
						}

						// Error: indexing with a numeric string migth get you a completly separate type of Animal
						interface NotOkay {
							[x: number]: Animal;
							'number' index type 'Animal' is not assignable to 'string' index type 'Dog'.
							[x: string]: Dog;
						}

					문자열 인덱스 시그니처는 "dictionary" 패턴을 설명하는 강력한 방법이지만, 모든 속성이 반환 타입과 일치해야 한다는 것을 강제한다.
					이는 문자열 인덱스가 obj.property도 obj["property"]로 사용할 수 있다고 선언하기 때문이다. 
					다음 예제에서 name의 타입이 문자열 인덱스의 타입과 일치하지 않으므로 타입 검사기가 오류를 표시한다.

						interface NumberDictionary {
							[index: string]: number;

							length: number; // ok
							name: string;

							Property 'name' of type 'string' is not assignable to 'string' index type 'number'.
						}

					그러나 인덱스 시그니처가 속성 타입의 유니언인 경우 다른 타입의 속성도 허용된다.

						interface NumberOrStringDictionary {
							[index: string]: number | string;
							length: number; // ok, length is a number
							name: string; // ok, name is a string
						}

					마지막으로, 인덱스 시그니처를 읽기 전용으로 만들어 해당 인덱스에 할당을 방지할 수 있다.

						interface ReadonlyStringArray {
							readonly [index: number]: string;
						}

						let myArray: ReadonlyStirngArray = getReadonlyStringArray();
						myArray[2] = "Mallory";

						Index stgnature in type 'ReadonlyStringArray' only permits reading.

					인덱스 시그니처가 읽기 전용이기 때문에 myArray[2]를 설정할 수 없다.


				Excess Property Check(초과 속성 검사)

					객체에 할당된 타입의 위치와 방법은 타입 시크템에 영향을 줄 수 있다. 
					이 중요한 예 중 하나는 객체가 생성되고 생성 중에 객체 타입에 할당될 때 객체를 보다 철저하게 유효성을 검사하는 "초과 속성 검사"이다.

						interface createSquare(config: SquareConfig): { color: string; area: number } {
							return {
								color: config.color || "red",
								area: config.width ? config.width * config.width : 20,
							};
						}

						let mySqueare = createSquare({ colour: "red", width: 100 });

						Argument of type '{ colour: string; width: number; }' is not assignable to parameter of type 'SquareConfig'.
							Object literal may only specify known propertes, but 'colour' dose not exist in type 'SquareConfig'. Did you mean to write 'color' ?

					createSquare는 주어진 인수가 color 대신 colour로 철자가 틀렸다. 일반 JavaScript에서는 이러한 타입의 오류가 조용히 실패한다.

					width 속성이 호환되어 있고 color 속성이 없으며 추가된 colour 속성이 중요하지 않으므로 이 프로그램이 올바르게 타입화된 것이라고 주장할 수 없다.

					그러나 TypeScript는 이 코드에서 버그가 있을 가능성이 있다고 주장한다. 객체 리터럴은 특별한 처리를 받으며 다른 변수에 할당되거나 인수로 전달될 때 초과 속성 검사를 받는다.
					객체 리터럴에 대상 타입이 값지 않은 속성이 있는 경우 오류가 발생한다.

						let mySqueare = createSquare({ colour: "red", width: 100 });

						Argument of type '{ colour: string; width: number; }' is not assignable to parameter of type 'SquareConfig'.
							Object literal may only specify known propertes, but 'colour' dose not exist in type 'SquareConfig'. Did you mean to write 'color' ?

					이러한 검사를 피하는 것은 실제로 매우 간단하다. 가장 쉬운 방법은 단순히 type assertion을 사용하는 것이다.

						let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);

					그러나 더 나은 접근 방법은 객체가 특별한 방식으로 사용되는 추가 속성을 가질 수 있다고 확신하는 경우 문자열 인덱스 시그니처를 추가하는 것이다.
					위의 타입을 가진 color 및 width 속성을 가질 수 있지만 다른 여러 속성을 가질 수도 있는 SquareConfig라면 다음과 같이 정의할 수 있다.

						interface SquareConfig {
							color?: string;
							width?: number;
							[propName: string]: any;
						}

					여기서는 SquareConfig가 color또는 width가 아닌 여러 속성을 가질 수 있으며, 그들의 타입은 중요하지 않다고 말하고 있다.

					이러한 검사를 우회하는 마지막 방법은 다른 변수에 객체를 할당하는 것이다. 
					squareOptions를 할당하면 초과 속성 검사가 수행되지 않으므로 컴파일러가 오류를 발생시키지 않는다.

						let squareOptions = { colour: "red", width: 100 };
						let mySquare = createSquare(squareOptions);


					위의 해결 방법은 squareOptions와 SquareConfig 간에 공통 속성이 있는 경우에만 작동한다.
					이 예에서는 속성 width가 그 예이다. 그러나 변수에 공통 객체 속성이 없는 경우 실패할 것이다. 예를 들어

						let squareOptions = { colour: "red" };
						let mySquare = createSquare(squareOptions);

						Type '{ colour: string; }' has no properties in common with type 'SquareConfig'.

					위와 같은 간단한 코드의 경우에는 이러한 검사를 "우회"하려고 하지 않는 것이 좋다.
					메서드를 갖고 상태를 유지하는 더 복잡한 객체 리터럴의 경우에는 이러한 기술을 염두에 두어야 할 수 있지만, 대부분의 초과 속성 오류는 실제로 버그이다.

					즉, 옵션 가방과 같은 것에 대해 초과 속성 검사 문제가 발생하는 경우, 일부 타입 선언을 수정해야 할 수도 있다.
					이 경우 createSquare에 색상 또는 colour 속성을 가진 객체를 전달하는 것이 괜찮다면 SquareConfig의 정의를 수정하여 이를 반영해야 한다.


				Extending Type(확장 타입)

					다른 타입의 보다 구체적인 버전일 수 있는 타입을 가지는 것은 꽤 일반적이다. 
					예를 들어, 미국에서 편지와 소포를 보내기 위해 필요한  필드를 설명하는 BasicAddress type을 가질 수 있다.

						interface BasicAddress {
							name?: string;
							street: string;
							city: string;
							country: string;
							postalCode: string;
						}

					일부 상황에서는 그것만으로 충분하지만, 주소에는 주소에 여러 단위가 있는 경우 해당 주소와 연결된 단위가 있을 수 있다.
					그럴 때 AddressWithUnit을 설명할 수 있다.

						interfaceWithUnit {
							name?: string;
							unit: string;
							street: string;
							city: string;
							coountry: string;
							postalCode: string;
						}

					이렇게 하면 작업이 완료되지만, 여기서의 단점은 변경 사항이 순수하게 추가적인 경우에도 BasicAddress의 모든 다른 필드를 반복해야 한다는 것이다. 
					대신, 우리는 원래의 BasicAddress 타입을 확장하고 AddressWithUnit에 고유한 새 필드만 추가할 수 있다.

						interface BasicAddress {
							name?: string;
							street: string;
							city: string;
							country: string;
							postalCode: string;
						}

						interface AddressWithUnit extends BasicAddress {
							unit: string;
						}

					인터페이스의 extends 키워드를 사용하면 다른 명명된 타입에서 멤버를 효과적으로 복사하고 원하는 새 멤버를 추가할 수 있다.
					이것은 작성해야 하는 타입 선언의 보일러플레이트 양을 줄이는 데 유용할 수 있으며, 동일한 속성의 여러 다른 선언이 관찰될 수 있음을 알리는 의도를 나타내는 데도 유용하다.
					예를 들어, AddressWithUnit은 street 속성을 반복할 필요가 없었으며 street는 BasicAddress에서 비롯되었으므로 독자는 이 두 타입이 어떤 방식으로 관련되어 있는지를 알 수 있다.

					인터페이스는 여러 타입에서 확장될 수 있다.

						interface Colorful {
							color: string;
						}

						interface Circle {
							radius: number;
						}

						interface ColorfulCircle extends Colorful, Circle {}

						const cc: ColorfulCircle = {
							color: "red",
							radius: 42,
						};


				Intersection Types(교차 타입)

					인터페이스는 다른 타입을 확장하여 새 타입을 구축할 수 있도록 허용한다. TypeScript는 기존 객체 타입을 결합하는 데 주로 사용되는 다른 구성 요소를 제공한다.
					이것은 Intersection Type 이라고한다. Intersection Type 은 & 연산자를 사용하여 정의한다.

						interface Colorful {
							color: string;
						}

						interface Circle {
							radius: number;
						}

						type ColorfulCircle = Colorful & Circle;

					여기서는 Colorful과 Circle을 교차하여 Colorful과 Circle의 모든 멤버를 가진 새 타입을 생성했다.

						function draw(circle: Colorful & Circle) {
							console.log(`Color was $(circle.color)`);
							console.log(`Radius was $(circle.radius)`);
						}

						// okay
						draw({ color: "blue", radius: 42 })''

						// oops
						draw({ color: "red", raidus: 42 })''

						Argument of type '{ color: string, raidus: number }' is not assignable to parameter of type 'Colorful & Circle'. 
							Object literal may only specify knwon properties, but 'raidus' does not exist in type 'Colorful & Circle' Did you mean to write 'radius'?


				Interface vs Intersection(인터페이스 vs 교차점)

					우리는 실제로는 약간 다른 두 가지 타입을 결합하는 방법 두 가지를 살펴보았다.
					인터페이스에서는 다른 타입을 확장하기 위해 extends 절을 사용할 수 있었으며, Intersection과 유사한 작업을 수행하고 결과를 타입 별칭으로 지정할 수 있었다.
					두 방법 간의 주요 차이점은 충돌이 처리되는 방식이며, 이 차이는 일반적으로 인터페이스와 Intersection Type 의 타입 별칭 중 하나를 선택할 때 주요 이유 중 하나이다.


			Generic Object Types(일반 개체 타입)

				어떤 값이든 포함할 수 있는 상자 타입(Box type)을 상상해 보자.
				문자열, 숫자, 기린 등을 담을 수 있다.

					interface Box {
						contents: any;
					}

				현재 contents 속성은 any로 타입이 지정되어 있으며 작동은 하지만 나중에 사고를 일으킬 수 있다.

				대신 unknown을 사용할 수도 있지만, 이미 contents의 타입을 알고 있는 경우에는 예방적인 확인이 필요하거나 오류가 발생하기 쉬운 타입 단언을 사용해야 한다.

					interface Box {
						contents: unknwon;
					}

					let x: Box = {
						contents: "hello world",
					}

					// 'x.contents'를 확인할 수 있다.
					if (typeof x.contents === "string") {
						console.log(x.contents.toLowerCase());
					}

					// 또는 type assertion을 사용할 수 있다.
					console.log((x.contents as string).toLowerCase());

				하나의 안전한 접근 방법은 대신 각종 contents 타입 대한 서로 다른 상자(Box) 타입을 만들어내는 것이다.

					interface NumberBox {
						contents: number;
					}

					interface StringBox {
						contents: string;
					}

					interface BooleanBox {
						contents: boolean;
					}

				하지만 이것은 이러한 타입에 작용하는 함수를 만들어거나 함수의 오버로드를 생성해야 한다는 것을 의미한다.

					function setContents(box: StringBox, newContents: string): void
					function setContents(box: NumberBox, newContents: number): void
					function setContents(box: BooleanBox, newContents: boolean): void
					function setContents(box: { contents: any }, newContents: any): {
						box.contents = newContents;
					}

				이것은 많은 보일러플레이트를 의미한다. 게다가, 나중에 새로운 타입과 오버로드를 도입해야 할 수도 있다.
				이것은 box 타입 오버로드가 사실상 모두 같기 때문에 짜증난다.

				대신, 타입 파라미터를 선언하는 제네릭(Box) 타입을 만들 수 있다.

					interface Box<Type> {
						content: Type;
					}

				"타입의 상자(Box)는 내용이 타입인 것이다"라고 읽을 수 있다.
				나중에 Box를 참조할 때, 타입의 자리에 타입 인수를 제공해야 한다.

					let box: Box<string>;

				Box를 실제 타입의 템플릿으로 생각해보자. 여기서 타입은 다른 타입으로 대체될 자리 표시자이다.
				TypeScript가 Box<string>을 볼 때, Box<Type>의 모든 인스턴스를 string으로 대체하고 { contents: stirng }과 같은 것을 처리하게 된다.
				다시 말해, Box<string>과 이전의 StringBox는 동일하게 작동한다.

					interface Box<Type> {
						contents: Type;
					}
					interface StringBox {
						contents: string;
					}

					let boxA: Box<string> = { contents: "hello" };
					boxA.contents;

						(property) Box<string>.contents: string

					let boxB: StringBox = { contents: "world" };
					boxB.contents;

						(property) StringBox.contents: string

				Box는 Type을 어떤 것으로든 대체할 수 있기 때문에 재사용할 수 있다. 이는 새로운 타입에 대한 상자가 필요할 때 새로운 Box 타입을 선언할 필요가 전혀 없다는 것을 의미한다.
				(물론 원한다면 선언할 수 있다.)

					interface Box<Type> {
						contents: Type;
					}

					interface Apple {
						// ...
					}

					// Same as '{ contents: Apple }'.
					type AppleBox = Box<Apple>;

				이것은 또한 대신 제네릭 함수를 사용함으로써 오버로드를 완전히 피할 수 있다는 것을 의마한다.

					function setContents<Type>(box: Box<Type>, newContents: Type) {
						box.contents = newContents;
					}

				타입 별칭도 제네릭일 수 있다는 점을 언급하는 것이 중요하다.
				다음과 같이 새로운 Box<Type> 인터페이스를 정의할 수 있다.

					interface Box<Type> {
						contents: Type;
					}

				대신 타입 별칭을 사용하여

					type Box<Type> = {
						contents: Type;
					}

				타입 별칭은 인터페이스와 달리 객체 유형 이상의 것을 설명할 수 있기 때문에 다른 종류의 제네릭 도우미 타입을 작성하는 데에도 사용할 수 있다.

					type OrNull<Type> = Type | null;

					type OneOrMany<Type> = Type | Type[];

					type OneOrManyOrNull<Type> = OrNull<OneOrMany<Type>>;

						type OneOrManyOrNull<Type> = OneOrMany<Type> | null

					type OneOrManyOrNullStrings = OneOrManyOrNull<string>;

						type OneOrManyOrNUllStrings = OneOrMany<string> | null

				타입 별칭에 다시 돌아갈 것이다.


				The Array Type(배열 타입)

					제네릭 객체 타입은 종종 그들이 포함하는 요소의 타입과는 독립적으로 작동하는 어떤 종류의 컨테이너 타입이다.
					데이터 구조가 이렇게 작동하여 서로 다른 데이터 타입 간에 재사용될 수 있도록 하는 것이 이상적이다.

					Array 타입과 유사한 타입을 계속 사용해왔었다. number[] 또는 string[] 같은 유형을 작성할 때, 그것은 사실 Array<number> 및 Array<string>의 줄임말일 뿐이다.

						function doSomething(value: Array<string>) {
							// ...
						}

						let myArray: string[] = ["hello", "world"];

						// 둘 중의 하나
						doSomething(myArray)
						doSomething(new Array("hello", "world"));

					위의 Box 타입과 마찬가지로 Array 자체도 제네릭 타입니다.

						interface Array<Type> {
							// 배열의 길이를 가져오거나 설정한다.
							length: number;

							//배열의 마지막 요소를 제거하고 반환한다.
							pop(): Type | undefined;

							// 배열에 새 요소를 추가하고 배열의 새 길이를 반환한다.
							push(...items: Type[]): number;

							// ...
						}

					현대 JavaScript에서는 Map<K, V>,Set<T>,Promise<T>와 같은 다른 제네릭 데이터 구조를 제공한다.
					이 모든 것이 실제로 뜻하는 것은 Map, Set 및 Promise가 어떻게 작동하는지에 따라서, 이들은 모든 종류의 타입과 작동할 수 있다는 것이다.


				The Readonly Type(읽기 전용 타입)

					ReadonlyArray는 변경되지 않아야 하는 배열을 나타내는 특수한 타입니다.

						function doStuff(values: ReadonlyArray<string>) {
							// 'values'를 읽을 수 있다.
							const copy = values.slice();
							console.log(`The first value is ${values[0]});

							// 하지만 'values'를 변경할 수는 없다.
							values.push("hello!");

							Property 'push' does not exist on type 'readonly string[]'.
						}

					readonly 수정자와 마찬가지로, 주로 의도를 나타내는 도구이다. 
					ReadonlyArrays를 반환하는 함수를 보면 내용을 전혀 변경하지 않아야 한다는 것을 알 수 있고, 
					ReadonlyArrays를 사용하는 함수를 보면 내용이 변경되지 않을 것을 걱정하지 않고 어떤 배열이든 그 함수에 전달할 수 있다는 것을 알 수 있다.

					Array와 달리 사용할 수 있는 ReadonlyArray 생성자가 없다.

						new ReadonlyArray("red", "green", "blue");
						'ReadonlyArray' only refers to a type, but is being used as a value here.

					대신에, 일반적인 Array를 ReadonlyArray로 할당할 수 있다.

						const roArray: ReadonlyArray<string> = ["red", "green", "blue"];

					TypeScript는 Type[]으로 Array<Type>을, readonly Type[]으로 ReadonlyArray<Type>을 간결하게 표현할 수 있는 단축 구문을 제공한다.

						function doStuff(values: readonly string[]) {
							// 'values'를 읽을 수 있다.
							const copy = values.slice();
							console.log(`The first value is ${values[0]});

							// 하지만 'values'를 변경할 수는 없다.
							values.push("hello!");

							Property 'push' does not exist on type 'readonly string[]'.
						}

					Readonly 속성 수정자와는 달리 일반 배열과 ReadonlyArray간의 할당 가능성은 양방향이 아니다.
					일반 배열을 ReadonlyArray에 할당할 수는 있지만, ReadonlyArray를 직접 일반 배열에 할당할 수는 없다. 이 제한은 ReadonlyArray로 표시된 배열의 불변성을 강제하는 데 도움이 된다.

						let x: readonly string[] = [];
						let y: string[] = [];

						x = y;
						y = x;

						The type 'readonly string[]' is 'readonly' and connot be assigned to the mutable type 'string[]'.


				Tuple Types(튜플 타입)

					튜플 타입은 배열의 한 종류로, 정확히 몇 개의 요소를 포함하고 있는지와 특정 위치에서 정확히 어떤 타입을 포함하고 있는지를 알고 있는 타입이다.
					TypeScript에서 튜플을 사용하면 서로 다른 타입의 요소를 고정된 길이의 배열로 그룹화할 수 있다. 이는 함수에서 여러 값을 반환할 때 유용하거나, 엄격하게 타입이 지정된 배열을 다룰 때 유용하다.

					예를 들어, 문자열과 숫자를 포함하는 튜플을 정의할 수 있다.

						let myTuple: [string, number];
						myTyple = ["hello", 10]; // ok

					튜플은 정의된 순서와 타입에 맞게 각 요소를 할당해야 한다. 또한 TypeScript 4.0이상에서는 튜플에서 나머지 요소(rest element)를 사용하여 여러 타입의 요소를 포함할 수 있는 배열을 나타낼 수 있다.

						let anotherTuple: [stirng, ...number[]];
						anotherTuple = ["hello", 10, 20, 30]; // ok

					위의 예에서 anotherTuple은 첫 번째 요소가 문자열이고, 이후 모든 요소는 숫자 배열이 되어야 함을 나타낸다. 이렇게 튜플을 사용하면 배열의 구조를 더 정확히 타입으로 표현할 수 있다.

						type StringNumberPair = [string, number];

					여기서, StringNumberPair는 문자열과 숫자의 튜플 타입이다. ReadonlyArray 처럼 실행 시점에서는 표현이 없지만, TypeScript에게는 중요하다.
					타입 시스템에게 StringNumberPair는 0번 인덱스에 문자열을, 1번 인덱스에 숫자를 포함하는 배열을 설명한다.

						function doSomething(pair: [string, number]) {
							const a = pair[0];

								const a: string

							const b = pair[1];

								const b: number

							// ...
						}

						doSomething(["hello", 42]);

					인덱스를 통해 요소의 수를 넘어서 접근하려고 하면 오류가 발생한다.

						function doSomething(pair: [string, number]) {
							// ...

							const c = pair[2];

							Tuple type '[string, number]' of length '2' has no element at index '2'.
						}

					튜플도 JavaScript의 배열 구조 분해를 사용하여 분해할 수 있다.

						function doSomething(stringHash: [string, number]) {
							const [inputString, hash] = stringHash;

						console.log(inputString);

							const inputString: string

						console.log(hash);

							const hash: number
						}

					튜플 타입은 각 요소의 의미가 "명확한" 규약 기반 API에서 유용하다. 이는 구조 분해할 때 변수 이름을 원하는 대로 지정할 수 있는 유연성을 제공한다.
					위의 예제에서 0번째와 1번째 요소에 원하는 이름을 지정할 수 있다.

					그러나 모든 사용자가 무엇이 명확한지에 대해 같은 견해를 가지고 있지 않기 때문에, 
					설명적인 속성 이름을 가진 객체를 사용하는 것이 API에 더 적합할 수도 있음을 재고해볼 가치가 있다.

					길이 검사를 제외하고, 이러한 간단한 튜플 타입은 특정 인덱스에 대한 속성을 선언하고, 길이를 숫자 리터럴 타입으로 선언한 배열의 버전과 동등한 타입이다.

						interface StringNumberPair {
							// 전문 속성
							length: 2;
							0: string;
							1: number;
						}

						// 다른 Array 멤버 <string | number>...
						slice(start?: number, end?: number): Array<string | number>;

					튜플에는 요소 타입 뒤에(?)를 작성함으로써 선택적 속성을 가질 수 있다.
					선택적 튜플 요소는 오직 끝에만 올 수 있으며, 길이의 타입에도 영향을 미친다.

						type Either2dOr3d = [number, number, number?];

						function setCoordinate(coord: Either2dOr3d) {
							const [x, y, z] = coord;

								const z: number | undefined

							console.log(`Provided coordinates had ${coord.length} dimensions`);

								(property) length 2 | 3
						}

					튜플은 또한 나머지 요소를 가질 수 있으며, 이 요소는 배열/튜플 타입이어야 한다.

						type StringNumberBooleans = [string, number, ...boolean[]];
						type StringBooleansNumber = [string, ...boolean[], number];
						type BooleansStringNumber = [...boolean[], stirng, number];

					- StringNumberBooleans는 처음 두 요소가 각각 문자열과 숫자이며, 그 뒤에 불리언이 몇 개가 오든 상관없는 튜플을 설명한다.
					- StringBooleansNumber는 첫 번째 요소가 문자열이고 그 다음에 불리언이 몇 개든 오고 마지막에 숫자가 오는 튜플을 설명한다.
					- BooleansStringNumber는 시작 요소가 몇 개든 상관없는 불리언이고 마지막에 문자열 다음 숫자가 오는 튜플을 설명한다.

					나머지 요소가 있는 튜플은 정해진 "길이"가 없다. 다만 다른 위치에서 잘 알려진 요소들의 집합을 가지고 있다.

						const a: StringNumberBooleans = ["hello", 1];
						const b: StringNumberBooleans = ["beautiful", 2, true];
						const c: StringNumberBooleans = ["world", 3, true, false, true, false, ture];

					선택적 요소와 나머지 요소가 있는 튜플은 TypeScript가 함수 매개변수 목록을 더 정확하게 모델링할 수 있게 해준다.
					튜플 타입을 나머지 매개변수와 인자에 사용함으로써, TypeScript는 함수에 대해 올바른 타입과 개수의 인자가 전달되도록 강제할 수 있다.
					이는 코드에서 타입 안정성과 예측가능성을 향상시키는 데 도움이 된다.

					예를 들어, 함수의 매개변수 목록을 모델링할 때, 선택적 요소와 나머지 요소를 포함한 튜플을 사용함으로써, TypeScript는 함수가 받을 수 있는 인자의 최소 및 최대 개수를 정확하게 표현할 수 있다.
					이는 코드를 더욱 명확하고 안전하게 만들어준다.

						function readButtonInput(...args: [string, number, ...boolean[]]) {
							const [name, version, ...input] = args;
							// ...
						}

					다음과 같이 해석될 수 있다.

						function readButtonInput(name: string, version: number, ...input: boolean[]) {
							// ...
						}

					이는 나머지 매개변수를 사용하여 변수 수의 인자를 취하고, 최소한의 요소 수가 필요하지만 중간 변수를 도입하고 싶지 않을 때 유용하다.


				readonly Tuply Types

					튜플 타입에 대한 마지막 주의 사항이다. 튜플 타입은 읽기 전용 변형을 가지고 있으며, 배열 축약 구문과 마찬가지로 앞에 readonly 수정자를 붙여서 지정할 수 있다.

						function doSomething(pair: readonly [string, number]) {
							// ...
						}

					예상할 수 있듯이, TypeScript에서는 readonly 튜플의 어떤 속성에도 쓰기가 허용되지 않는다.

						function doSomething(pair: readonly [string, number]) {
							pair[0] = "hello";

							Cannot assign to '0' because it is a read-only property.
						}

					대부분의 코드에서 튜플은 생성되고 수정되지 않는 경향이 있으므로, 가능할 때 readonly 튜플로 타입을 주석 처리하는 것이 좋은 기본값이다.
					또한 const  단언문이 있는 배열 리터럴은 readonly 튜플 타입으로 추론될 것이기 때문에 이것은 중요하다.

						let point = [3, 4] as const;

						function distanceFromOrigin([x, y]: [number, number]) {
							return Math.sqrt(x ** 2 + y ** 2);
						}

						distanceFromOrigin(point);

						Argument of type 'readonly [3, 4]' is not assignable to parameter of type '[number, number]'.
							The type 'readonly [3, 4]' is 'readonly' and cannot be assigned to the mutable type '[number, number]'.

					여기서, 'distanceFromOrigin'은 그 요소들을 수정하지 않지만 가변 튜플을 기대한다. 
					point의 타입이 readonly [3, 4]로 추론되었기 때문에, 이 타입은 point의 속성들이 변경되지 않을 것이라는 것을 보장할 수 없어서 [number, number]와 호환되지 않는다.



-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
