<template>
	<div></div>
</template>
<!--  

  Template Literal Types

    Template literal types은 JavaScript의 문자열 리터럴 타입을 기반으로 하며, 유니온을 통해 여러 문자열로 확장될 수 있는 능력을 가진다.

    JavaScript에서 템플릿 리터럴 문자열과 같은 문법을 사용하지만 타입 위치에서 사용된다.
    구체적인 리터럴 타입과 함께 사용될 때, 템플릿 리터럴은 내용을 연결하여 새로운 문자열 리터럴 타입을 생성한다.

      type World = "world";

      type Greeting = `hello${World}`;

        type Greeting = "hello world"

    유니온이 보간된 위치에서 사용될 때, 타입은 각 유니온 멤버에 의해 표현될 수 있는 모든 가능한 문자열 리터럴의 집합이 된다.

      type EmailLocaleIDs = "welcome_email" | "email_heading";
      type FooterLocaleIDs = "footer_title" | "footer_sendoff";

      type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`;

        type AllLocaleIDs = "welcome_email_id" | "email_heading_id" | "footer_title_id" | "footer_sendoff_id"

    템플릿 리터럴의 각 보간된 위치에서, 유니온들은 교차 곱셈된다.

      type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`
      type Lang = "en" | "ja" | "pt";

      type LocaleMessageIDs = `${Lang}_{AllLocaleIDs}`;

        type LocaleMessageIDs = "en_welcome_email_id" | "en_email_heading_id" | "en_footer_title_id" | "en_footer_sendoff_id" | "ja_welcome_email_id" | "ja_email_heading_id" | "ja_footer_title_id" | "ja_footer_sendoff_id" | "pt_welcome_email_id" | "pt_email_heading_id" | "pt_footer_title_id" | "pt_footer_sendoff_id"

    일반적으로 큰 문자열 유니온을 위해 사전 생성(ahead-of-time-geneation) 사용을 권장하지만, 이 방법은 작은 경우에 유용하다.


  타입 내의 문자열 유니온

    템플릿 리터럴의 힘은 타입 내부의 정보를 기반으로 새로운 문자열을 정의할 때 나타난다.

    예를 들어, 어떤 함수(makeWatchedObject)가 전달된 객체에 on()이라는 새 함수를 추가하는 경우를 생각해 볼 수 있다.
    JavaScript 에서의 호출은 makeWatchedObject(baseObject)와 같이 보일 수 있다.
    기본 객체는 다음과 같이 생겼을 것으로 상상할 수 있다.

      const passedObject = {
        firstName: "Saoires",
        lastName: "Ronan",
        age: 26,
      };

    on 함수는 기본 객체에 추가되며, 두 개의 인자를 기대한다. (eventName 문자열과 callback 함수)

    eventName은 `attributeInThePassedObject + "Change"`의 형태여야 한다.
    따라서 기분 객체의 firstName 속성에서 유래된 firstNameChanged와 같이 된다.

    callback 함수가 호출될 때는 다음과 같아야 한다.

      - attributeInThePassedObject의 이름과 관련된 타입의 값이 전달되어야 한다.
        따라서 firstName이 문자열로 타입 지정되어 있으므로, firstNameChanged 이벤트에 대한 콜백은 호출 시 문자열이 전달되기를 기대한다.
        마찬가지로 age 와 관련된 이벤트는 숫자 인수로 호출되어야 한다.

      - 반환 타입은 void여야 한다.(시연의 단순화를 위해)

    따라서 on()의 단순한 함수 시그니처는 on(eventName: string, callback: (newValue: any) => void)일 수 있다.
    그러나 앞서의 설명에서 코드에 문서화하고 싶은 중요한 타입 제약을 확인했다.
    템플릿 리터럴 타입을 사용하여 이러한 제약을 코드에 반영할 수 있다.

      const person = makeWatchedObject({
        firstName: "Saoirse",
        lastName: "Ronan",
        age: 26,
      });

      // makeWatchObject가 익명 Object에 on을 추가했다.
      person.on("firstNameChange", (newValue) => {
        console.log(`firstName was change to ${newValue}`);
      });

    on 함수는 단순히 "firstName"이 아니라 "firstNameChanged"이벤트를 감지한다.
    만약 감시하는 객체의 속성 이름들의 합집합에 "Changed"를 추가한 것을 허용 가능한 이벤트 이름의 집합으로 제한하게 한다면, 초기 on() 함수의 명세는 더욱 견고해질 수 있다.
    JavaScript에서 Object.keys(passedObject).map(x => \ `${x}Changed`)와 같은 계산을 하는 것이 편한 것처럼, 타입 시스템 내의 템플릿 리터럴도 유사한 방식으로 문자열 조작을 제공한다.

      type PropEventSource<Type> = {
        on(eventName: `${string & keyof Type}Changed`, callback: (newValue: any) => void): void;
      };

      // on 메서드로 watched object 만들기
      // 속성의 변경 사항을 확인할 수 있다.
      declare function makeWatchedObject<Type>(obj: Type): Type & PropEventSource<Type>;

    이를 통해, 잘못된 속성이 주어졌을 때 오류를 발생시킬 수 있는 구조를 만들 수 있다.

      const person = makeWatchedObject({
        firstName: "Saoires",
        lastName: "Ronan",
        age: 26
      });

      person.on("firstNameChanged", () => {});

      // 사람이 쉽게 할 수 있는 실수를 저지한다(이벤트 이름 대신 키를 사용하는 경우)
      person.on("firstName", () => {});

      Argument of type '"firstName"' is not assignable to parameter of type '"firstNameChanged" | "lastNameChanged" | "ageChanged"'.

      // 오타에 강하다.
      person.on("frstNameChanged", () => {});
      Argument of type '"frstNameChanged"' is not assignable to parameter of type '"firstNameChanged" | "lastNameChanged" | "ageChanged"'.


    템플릿 리터럴을 이용한 추론

      원래 전달된 객체에서 제공된 모든 정보를 활용하지 못했다.
      예를 들어, firstName이 변경되었다면(즉, firstNameChanged 이벤트가 발생하면), 콜백 함수는 string 타입의 인수를 받아야 한다.
      마찬가지로, age가 변경될 때의 콜백은 number 인수를 받아야 한다. 콜백의 인수를 any 타입으로 단순하게 사용하고 있다.
      다시 말하지만, 템플릿 리터럴 타입을 사용하면 속성의 데이터 타입이 해당 속성의 콜백 함수의 첫 번째 인수의 타입과 같게 보장할 수 있다.

      이를 가능하게 하는 핵심적인 통찰력은 다음과 같다.
      제네릭을 사용하는 함수를 사용할 수 있다.

        1. 첫 번째 인수로 사용된 리터럴이 리터럴 타입으로 캡처된다.
        2. 그 리터럴 타입이 제네릭의 유효 속성 연합에 속하는지 검증될 수 있다.
        3. 검증된 속성의 타입은 인덱스 접근을 사용하여 제네릭의 구조에서 조화될 수 있다.
        4. 이 타이핑 정보는 콜백 함수의 인수가 같은 타입이 되도록 적용될 수 있다.

        type PropEventSource<Type> = {
          on<Key extends string & keyof Type>
            (eventName: `${Key}Changed`, callback: (newValue: Type[Key]) => void): void;
        };

        declare function makeWatchedObject<Type>(obj: Type): Type & PropEventSource<Type>;

        const person = makeWatchedObject({
          firstName: "Saoirse",
          lastName: "Ronan",
          age: 26
        });

        person.on("fisrtNameChanged", newValue => {

          (parameter) newName: string

          console.log(`new name is ${newName.toUpperCase()}`);
        });

        person.on("ageChanged", newAge => {

          (parameter) newAge: number

          if (newAge < 0) {
            console.warm("warning negative age")
          }
        })

      여기에서 on을 제네릭 메서드로 만들었다.

      사용자가 "firstNameChanged"라는 문자열을 호출할 때, TypeScript는 Key의 올바른 타입을 추론하려고 한다.
      그렇게 하기 위해, TypeScript는 Changed 전의 내용과 Key를 매칭하여 문자열 firstName을 추론한다.
      TypeScript가 이를 파악하고 나면, on 메서드는 원래 객체에서 firstName의 타입을 가져올 수 있으며, 이 경우에는 문자열이다.
      마찬가지로, ageChanged로 호출될 때, TypeScript는 속성 age의 타입을 찾아내고 이는 숫자이다.

      추론은 다양한 방식으로 결합될 수 있으며, 종종 문자열을 해체하고 다른 방식으로 재구성하는 데 사용된다.


    내장 문자열 조작 타입

      문자열 조작을 돕기 위해, TypeScript는 문자열 조작에 사용할 수 있는 일련의 타입을 포함하고 있다.
      이러한 타입들은 컴파일러에 내장되어 있어 성능을 위해 포함되어 있으며 TypeScript와 함께 제공되는 .d.ts 파일에서는 찾을 수 없다.


      Uppercase<StringType>

        문자열의 각 문자를 대문자 버전으로 변환한다.

        type Greeting = "Hello, world"
        type ShoutyGreeting = Uppercase<Greeting>

          type ChoutGreeting = "HELLO, WORLD"

        type ASCIICacheKey<Str extends string> = `ID-${Uppercase<Str>}`
        type MainID = ASCIICacheKey<"my_app">

          type MainID = "ID-MY_APP"


      lowercase<StringType>

        문자열의 각 문자를 소문자로 변환한다.

        type Greeting = "Hello world"
        type QuiestGreeting = Lowercase<Greeting>

          type QuietGreeting = "hello world"

        type ASCIICacheKey<Str extends string> = `id-${Lowercase<Str>}`
        type MainID = ASCIICacheKey<"MY_APP">

          type MainID = "id-my_app"


      Capitalize<StringType>

        문자열의 첫 번째 문자를 대문자로 변환한다.

        type LowercaseGreeting = "hello, world";
        type Greeting = Capitalize<LowercaseGreeting>;
                
        type Greeting = "Hello, world"


      Uncapitalize<StringType>

        문자열의 첫 번째 문자를 소문자로 변환한다.

        type UppercaseGreeting = "HELLO WORLD";
        type UncomfortableGreeting = Uncapitalize<UppercaseGreeting>;

        type UncomfortableGreeting = "hELLO WORLD"

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
