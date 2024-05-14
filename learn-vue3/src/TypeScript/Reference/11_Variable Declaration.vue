<template>
	<div></div>
</template>
<!--  

  변수 선언(Variable Declaration)

    let과 const는 JavaScript에서 비교적 새로운 두 가지 유형의 변수 선언이다.
    앞에서 언급했듯이, let은 var와 어느 정도 유사하지만, 사용자가 JavaScript에서 마주치는 결함을 피할 수 있게 해준다.
    const는 let의 기능이 강화된 것으로 변수에 재할당을 방지한다.

    TypeScript는 JavaScript의 상위 집합이므로, 당연히 let과 const를 지원한다. 여기서는 새로운 선언 방식들과 왜 그 방식들이 var보다 선호되는지를 더 자세히 설명한다.

    JavaScript에서 var 선언의 단점들에 대해 모두 알고 있다면 쉽게 넘어갈 수 있다.


  var 선언 (var declarations)

    기존 JavaScript에서는 변수 선언을 할 때 var 키워드를 사용했다.

      var a = 10;

    알다시피, a라는 변수를 10이라는 값으로 선언했다.

    또한, 변수를 함수 내부에 선언할 수도 있다.

      function f() {
        var message = "Hello, world";

        return message;
      }

    그리고 같은 변수를 다른 함수 안에서 접근할 수도 있다.

      function f() {
        var a = 10;
        return function g() {
          var b = a + 1;
          return b;
        }
      }
      var g = f();
      g(); // '11'을 반환

    위 예제에서, g는 f안에 선언된 a를 잡아 둔다. g가 호출될 때, a의 값은 f안의 a값을 가리킨다.
    f가 실행되면서 g가 한번 호출된 후에도, a에 접근해 수정할 수 있다.

      function f() {
        var a = 1;

        a = 2;
        var b = g();
        a = 3;

        return b;

        function g() {
          return a;
        }
      }

      f(); // '2' 반환


  스코프 규칙(Scoping rules)

    var 선언은 다른 언어와 다른 이상항 스코프 규칙들을 가지고 있다. 아래 예제를 살펴보자.

      function f(shouldInitialize: boolean) {
        if (shouldInitialize) {
          var x = 10;
        }
        return x;
      }
      f(true);  // '10' 반환
      f(false); // 'undefined' 반환

    변수 x는 if 블록 안에 선언되어 있지만, 블록의 바깥에서도 이 변수에 접근할 수 있다. 
    이 이유는 var 선언은 이를 감싸고 있는 블록과 관계없이 이를 감싸고 있는 함수, 모듈, 네임스페이스, 전역 스코프에서 접근할 수 있기 때문이다.
    매개 변수 또한 함수 스코프이다.

    이런 스코프 규칙은 몇 가지 실수를 유발할 수 있다. 더욱 문제를 심각하게 하는 것은 변수를 여러 번 선언하는 것이 에러가 아니라는 것이다.

      function sumMatrix(matrix: number[][]) {
        var sum = 0;
        for (var i = 0; i < matrix.length; i++) {
          var currentRow = matrix[i];
          for (var i = 0; i < currentRow.length; i++) {
            sum += currentRow[i];
          }
        }
        return sum;
      }

    아마 쉽게 찾을 수 있겠지만, i가 같은 함수 스코프의 변수를 참조하고 있기 때문에 for-loop안에서 실수로 변수 i를 덮어 쓸 수도 있다.


  변수 캡쳐링의 단점 (Variable capturing quirks)

    다음 코드의 출력 결과를 예상해 보자.

      for (var i = 0; i < 10; i ++) {
        setTimeout(function() { console.log(i); }, 100 * i);
      }

      // 출력 결과
      10
      10
      10
      10
      10
      10
      10
      10
      10
      10

    많은 사람들이 출력 결과가 다음과 같을 거라고 생각한다.

      0
      1
      2
      3
      4
      5
      6
      7
      8
      9

    setTimeout에 전달하는 모든 함수 표현식은 사식 같은 스코프에서 같은 i를 참조한다.

    setTimeout은 함수를 몇 밀리 초 후에 실행 시키겠지만. 항상 for루프가 실행을 멈추고 난 뒤에 실행된다.
    for 루프가 실행을 중지했을 때, i의 값은 10이다. 따라서 매번 주어진 함수가 호출될 때마다 10을 출력할 것이다.

      for (var i = 0; i < 10; i++) {
        // 현재 값으로 함수를 호출시켜
        // 현재 상태의 'i'를 잡아둔다.
        (function(i) {
          setTimeout(function() { console.log(i); }, 100 * i);
        })(i);
      }

    이런 이상해 보이는 패턴이 사실 일반적인 패턴이다. 매개변수에 i가 for 루프의 i를 감춰 버린다. 
    하지만 이름을 같게 했기 때문에 루프의 실행 부를 크게 수정할 필요가 없다.


  let 선언 (let declarations)

    var의 몇가지 문제점에 대해 알게 되었는데, 이런 이유 때문에 let을 도입하게 되었다.
    사용되는 키워드를 빼고는 let문은 var와 동일한 방법으로 작성된다.

      let hello = "Hello";

    주요한 차이점은 구문 보단 의미에 있는데, 이제 이 내용을 살펴볼 것이다.


  블록 스코프(Block-scoping)

    변수가 let을 이용해 선언되었을 때, 이는 렉시컬 스코핑(lexical-scoping) 혹은 블록 스코핑(block-scoping)이라 불리는 것을 사용한다.
    var로 선언된 변수가 이를 포함한 함수까지 흘러나오는 것과 달리, 블록-스코프 변수들은 이를 가장 가깝게 감싸고 있는 블록 혹은 for-루프 밖에서 접근할 수 없다.

      function f(input: boolean) {
        let a = 100;
        if (input) {
          // 'a'를 참조할 수 있다.
          let b = a + 1;
          return b;
        }
        // 오류: 'b'는 여기서 존재하지 않다.
        return b;
      }

    여기, 두 지역 변수 a와 b가 있다. a의 스코프는 f의 블록으로 한정되지만, b는 이를 감싸고 있는 if문의 블록까지로 한정된다.

    catch문에 선언된 변수 또한 비슷한 스코프 규칙을 가진다.

      try {
        throw "oh no!";
      }
      catch (e) {
        console.log("Oh well.");
      }
      // 오류: 'e'는 여기서 존재하지 않다.
      console.log(e);

    또 다른 블록-스코프 변수의 특징은 변수들이 선언되기 전에 읽거나, 쓰는 것이 불가능하다는 것이다.
    이 변수들은 스코프에 걸쳐 "존재"하지만, 선언되는 부분 전까지 모든 부분들이 temploral dead zone이다. 
    이것은 let 문 이전에 변수들에 접근할 수 없다는 정교한 방식이며, 다행히 TypeScript가 알려준다.

      a++; // a가 선언되기 전에 잘못된 사용.
      let a;

    주의할 점은 여전히 선언되기 전에 블록-스코프 변수를 잡아둘 수 있다는 것이다. 선언되기 전에 함수를 실행하는 것이 안된다는 것만 알아두면 된다.
    ES2015를 대상으로한, 최신 런타임은 오류를 던질 것이다. 하지만 현재 TypeScript에서는 허용되며, 오류를 보고하지 않는다.

      function foo() {
        // 'a' 캡처는 성공
        return a;
      }
      // `a`가 선언되기 전에 `foo` 를 호출
      // 런타임에 오류를 던질 것 이다.
      foo();
      let a;


  재선언과 쉐도잉 (Re-declarations and Shadowing)

    var로 선언하면 얼마나 변수를 많이 선언하는지 중요하지 않다고 했다. 단 하나만 생성된다.

      function f(x) {
        var x;
        var x;

        if (ture) {
          var x;
        }
      }

    위 예제를 보면 모든 x의 선언은 사실 같은 x를 가르키며, 이는 유효하다. 이건 종종 버그의 원인이 된다.
    고맙게도 let 선언은 이것을 허용하지 않는다.

      let x = 10;
      let x - 20; // 오류 'x'를 같은 스코프에 선언할 수 없다.

    TypeScript가 문제를 알려주기 때문에, 변수를 반드시 블록 범위로 지정할 필요는 없다.

      function f(x) {
        let x = 100; // 오류: 매개 변수 선언을 방해한다.
      }
      function g() {
        let x = 100;
        var x = 100; // 오류: `x`를 중복해서 선언할 수 없다.
      }

    블록-스코프 변수가 함수-스코프 변수로 선언될 수 없다는 것은 아니다. 블록 스코프 변수는 단지 별개의 다른 블록에 선언되어야 한다.

      function f(condition, x) {
        if (condition) {
          let x = 100;
          return x;
        }
        return x;
      }
      f(false, 0); // '0' 반환
      f(true, 0);  // '100' 반환

    더 중첩된 스코프에서 바깥 스코프의 변수 이름을 사용하는 것을 shadowing이라고 한다.
    shadowing은 양날의 검이라고 할 수 있는데, 이는 실수로 방생되어 특정 버그를 일으키거나, 혹은 특정 버그를 막기 위해 쓰이기 때문이다.
    예를 들어, 위에서 사용했던 sumMatrix 함수를 let을 이용해 작성했다고 생각해보자.

      function sumMatrix(matrix: number[][]) {
        let sum = 0;
        for (let i = 0; i < matrix.length; i++) {
          var currentRow = matrix[i];
          for (let i = 0; i < currentRow.length; i++) {
            sum += currentRow[i];
          }
        }
        return sum;
      }

    이 루프는 합을 올바르게 계산할 것이다. 왜냐하면 안쪽 루프의 i가 바깥 루프의 i를 가리기 때문이다.

    보통 더 명확한 코드 작성을 위해 shadowing 사용을 피한다. 하지만 shadowing의 이점을 활용할 수 있는 적합한 상황이 있으므로, 최선의 판단을 내려야 한다.


  블록 스코프 변수 캡쳐링 (Block-scoped variable capturing)

    var 선언에 변수 캡쳐링을 하는 것을 처음 보았을 때, 변수가 한번 캡쳐되면 어떻게 동작하는지 간단히 살펴보았다.
    이를 더 잘 이해해 보면, 스코프가 각각 실행될 때마다 변수의 "환경"을 만든다. 변수의 환경과 캡쳐된 변수들은 심지어 스코프가 포함한 모든 것이 실행을 종료한 후에도 존재한다.

      function theCityThatAlwaysSleeps() {
        let getCity;
        if (true) {
          let city = "Seattle";
          getCity = function() {
            return city;
          }
        }
        return getCity();
      }

    city를 해당 환경 안에 캡쳐했기 때문에, if 블록의 실행이 완료되었음에도 여전히 city에 접근할 수 있다.
    
    let 선언은 루프의 일부로 선언될 때 동작이 크게 다르다. 이 선언은 루프 자체에 새로운 환경을 만드는 대신, 반복마다 새로운 환경을 만들어 낸다.

      for (let i = 0; i < 10 ; i++) {
        setTimeout(function() { console.log(i); }, 100 * i);
      }

    그리고 예상 했던 대로, 다음과 같은 결과가 출력된다.

      0
      1
      2
      3
      4
      5
      6
      7
      8
      9


  const 선언 (const declarations)

    const 선언은 변수를 선언하는 또 다른 방법이다.

      const numLiveForcat = 9;

    이 방법은 let 선언과 비슷하지만 그 이름에서 말해주듯이, 일단 바인딩 되면 값을 변경할 수 없다.
    다른 말로 const는 let과 같은 스코프 규칙을 가지고 있지만, 재할당 할 수 없다.

    이를 const가 참조하는 값이 불변이라고 혼동하면 안 된다.

      const numLivesForCat = 9;
      const kitty = {
        name: "Aurora",
        numLives: numLivesForCat,
      }
      // 오류
      kitty = {
        name: "Danielle",
        numLives: numLivesForCat
      };
      // 모두 "성공"
      kitty.name = "Rory";
      kitty.name = "Kitty";
      kitty.name = "Cat";
      kitty.numLives--;

    위와 같은 상황을 피하기 위해 특별한 조치를 취하지 않는 한, const 변수의 내부 상태는 여전히 수정 가능하다.
    다행히, TypeScript를 사용하면 객체의 멤버가 읽기 전용(readonly)이라고 지정할 수 있다.


  let vs const

    유사한 스코프의 의미를 가지는 두 가지 유형의 변수 선언이 있기 때문에, 어느 것을 사용하는지는 스스로 선택해야 한다.
    광범위한 질문처럼, 답은 "때에 따라 다르다"이다.

    최소 권한의 원칙을 적용하면, 수정하려는 선언 이외에 모든 선언은 const를 사용해야 한다.
    그 이유는, 만약 변수가 수정될 필요가 없다면 같은 코드베이스에서 작업하는 다른 사람들이 자동으로 객체를 수정할 수 없어야 하고, 그들이 정말 변수에 재할당할 필요가 있는지는 고려할 필요가 있다.
    const를 사용하는 것은 데이터의 흐름을 추론할 때 코드를 더 예측하기 쉽게 해준다.

    이 핸드북은 대부분 let 선언을 사용한다.


  구조 분해 (Destructuring)

    TypeScript가 가진 또 다른 ECMAScript 2015 특징은 구조 분해이다.


  배열 구조 분해 (Array destructurting)

    구조 분해의 가장 단순한 형태는 배열 구조 분해 할당이다.

      let input = [1, 2];
      let [first, second] = input;
      console.log(first); // 1 출력
      console.log(second); // 2 출력

    이는 first, second라는 이름의 새로운 두 변수를 생성한다. 이는 인덱싱을 사용하는 것과 동일하지만 더 편리하다.

      first = input[0];
      second = input[1];

    구조 분해 할당은 이미 선언된 변수에도 동작한다.

      // 변수를 스왑
      [first, second] = [second, first];

    그리고, 함수의 매개변수에도 동작하다.

      function f([first, second]: [number, number]) {
        console.log(first);
        console.log(second);
      }
      f([1, 2]);

    나머지 요소들에 대해 ... 구문을 사용하여 변수를 생성할 수 있다.

      let [first, ...rest] = [1, 2, 3, 4];
      console.log(first); // 1 출력
      console.log(rest); // [ 2, 3, 4 ] 출력

    물론 JavaScript이기 때문에, 필요하지 않은 뒤따라 오는 요소들을 무시할 수 있다.

      let [first] = [1, 2, 3, 4];
      console.log(first); // 1 출력

    또는 그 밖에 요소들을 무시할 수 있다.

      let [, second, , fourth] = [1, 2, 3, 4];
      console.log(second); // 2 출력
      console.log(fourth); // 4 출력


  튜플 구조 분해 (Tuple destructuring)

    튜플은 배열처럼 구조 분해된다. 구조 분해된 변수는 튜플 요소와 일치하는 타입을 얻게 된다.

      let tuple: [number, string, boolean] = [7, "hello", true];
      let [a, b, c] = tuple; // a: number, b: string, c: boolean

    튜플의 범위를 넘어선 구조 부해는 오류이다.

      let [a, b, c, d] = tuple; // 오류, 인덱스 3에 요소가 없습니다.

    배열과 마찬가지로, 더 짧은 튜플을 얻기 위해 ...로 튜플의 나머지를 구조 분해할 수 있다.

      let [a, ...bc] = tuple; // bc: [string, boolean]
      let [a, b, c, ...d] = tuple; // d: [], 비어있는 튜플

    또는 뒤따라 오는 요소나 다른 요소를 무시할 수 있다.

      let [a] = tuple; // a: number
      let [, b] = tuple; // b: string


  객체 구조 분해 (Object destructuring)

    또한 객체를 구조 분해할 수 있다.

      let o = {
        a: "foo",
        b: 12,
        c: "bar"
      };
      let { a, b } = o;

    이는 o.a, o.b로 부터 새로운 변수 a와 b를 생성한다. 필요 없다면 c를 건나 뛸 수 있다
    배열 구조 분해처럼, 선언 없이 할당할 수 있다.

      ({ a, b } = { a: "baz", b: 101 });

    이 구문을 괄호를 감싸고 있다는 것을 주의하자. JavaScript는 보통 {} 블록을 시작으로 파싱한다.
    객체 안에 나머지 요소들을 ... 구문을 사용하여 변수로 생성할 수 있다.

      let { a, ...passthrough } = o;
      let total = passthrough.b + passthrough.c.length;

    프로퍼티 이름 바꾸기 (Property renaming)

      프로퍼티들에 다른 이름을 붙이는 것도 가능하다.

        let { a: newName1, b: newName2 } = o;

      여기서 구문이 혼란스러워지기 시작한다. a: newName1 을 "a를 newName1로" 와 같이 읽을 수 있다.

        let newName1 = o.a;
        let newName2 = o.b;

      혼란스럽게도 여기서 콜론은 타입을 나타내지 않는다. 타입을 지정하는 경우, 전체 구조 분해 뒤에 작성해야 한다.

        let { a, b }: { a: string, b: number } = o;

      기본값 (Default values)

        기본 값은 프로퍼티가 정의되지 않았을 대 기본값을 사용하도록 하는 것이다.

          function keepWholeObject(wholeObject: { a: string, b?: number }) {
            let { a, b = 1001 } = wholeObject;
          }

        예제에서 b?는 b가 선택적이라는 것을 의미한다. 따라서 이는 undefined 일 수도 있다. keepWholeObject는 이제 b가 undefined 이더라도 a, b 프로퍼티와 함께 wholeObject라는 변수를 가진다.


  함수 선언 (Function declarations)

    구조 분해는 함수 선언에서도 동작한다. 이것은 간단한 경우에는 직관적이다.

      type C = { a: string, b?: number }
      function f({ a, b }: C): void {
        // ...
      }

    그러나 매개 변수에는 기본값을 명시하는 것이 더 일반적이며, 구조 분해와 함께 기본값을 제대로 사용하는 것은 까다로울 수 있다.
    가장 먼저, 구조 분해 패턴을 기본값 앞에 넣어야 한다는 것을 기억해야 한다.

      function f({ a="", b=0 } = {}): void {
        // ...
      }
      f();

    그런 다음, 선택적 프로퍼티를 위해 기본 초기화 대신 구조 분해될 프로퍼티에 기본 값을 주어야 한다는 걸 기억해야 한다.
    C가 b를 선택적으로 정의했다는 것을 기억하자.

      function f({ a, b = 0 } = { a: "" }): void {
        // ...
      }
      f({ a: "yes" }); // 성공, 기본값으로 b = 0 입니다.
      f(); // 성공, 기본 값은 { a: "" } 이고 b = 0 입니다.
      f({}); // 오류, 매개 변수가 주어지면 `a`가 필요합니다.

    구조 분해를 주의해서 사용해야한다. 앞에 예제에서 알 수 있듯이, 가장 간단한 구조 분해 표현식 이외의 것들은 혼란스럽다.
    심지어 이름 변경, 기본값, 타입 표시가 없더라도 깊게 중첩된 구조 분해는 정말 이해하기 힘들다. 구조 분해 표현식을 작고 간단하게 유지해야 한다.


  전개 (Spread)

    전개 연산자는 구조 분해와 반대이다. 이는 배열을 다른 배열 안에, 혹은 객체를 다른 객체 안에 전개하도록 해준다. 예를 들어,

      let first = [1, 2];
      let second = [3, 4];
      let bothPlus = [0, ...first, ...second, 5];

    이는 bothPlus에 [0, 1, 2, 3, 4, 5]라는 값을 준다. 전개는 first와 second의 얕은 복사를 만든다. 이들은 전개에 의해 변하지 않는다.

    또한 객체를 전개할 수 있다.

      let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
      let search = { ...defaults, food: "rich" };

    여기서 search는 { food: "rich", price: "$$", ambiance: "noisy" } 이다. 객체 전개는 배열 전개보다 훨씬 복잡하다.
    배열 전개처럼 왼쪽에서 -오른쪽으로 진행되지만 그 결과는 여전히 객체이다. 이는 전개 객체 안에서 나중에 오는 프로퍼티가 이전에 오는 프로퍼티를 덮어쓰는 것을 의미한다.
    그래서 만약에 우리가 이전 예제를 마지막에 전개하도록 수정하면

      let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
      let search = { food: "rich", ...defaults };

    defaults 안에 food 프로퍼티는 food: "rich"를 덮어쓰는데, 이 경우 우리가 의도한 것은 아니다.

    객체 전개는 또한 몇몇의 놀라운 제한점이 있다. 첫째로, 이는 오직 객체 본인의 열거 가능한 프로퍼티만 해당한다는 것이다.
    기본적으로, 이는 객체의 인스턴스를 전개하면 메서드를 잃게 된다는 것을 뜻한다.

      class C {
        p = 12;
        m() {
        }
      }
      let c = new C();
      let clone = { ...c };
      clone.p; // 성공
      clone.m(); // 오류!

    두 번째로, TypeScript 컴파일러는 제네릭 함수에서 타입 매개변수를 전개하는 것을 허용하지 않는다. 이 기능은 이후 버전에서 예상되는 기능이다.
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
