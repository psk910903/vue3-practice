<template>
	<div></div>
</template>
<!--  

	이터러블

		Symbol.iterator 프로퍼티에 대한 구현을 가진 객체는 이터러블로 간주한다.
		Array, Map, Set, String, Int32Array, Uint32Array 등과 같은 내장 타입에는 Symbol.iterator 프로퍼티가 이미 구현되어 있다.
		객체에 있는 Symbol.iterator 함수는 반복할 값 목록을 반환한다.


		for..of 문

			for..of문은 객체 내부 Symbol.iterator 프로퍼티를 호출해 이터러블 객체를 반복한다.
			다음은 배열의 간단한 for..of 루프이다.

				let someArray = [1, "string", false];

				for (let entry of someArray) {
					console.log(entry); // 1, "string", false
				}

			for..of vs. for..in 문

				for..of와 for..in문은 모두 리스트를 반복하지만 반복하는 값이 다르다. 
				for..in은 반복하는 객체의 키 리스트를 반환하지만 for..of는 반복하는 객체의 숫자 프로퍼티인 값 리스트를 반환한다.
				다음은 이러한 차이를 보여주는 예시이다.

					let list = [4, 5, 6];
					for (let i in list) {
						console.log(i); // "0", "1", "2"
					}
					for (let i of list) {
						console.log(i); // "4", "5", "6"
					}

				또 다른 차이는 for..in이 모든 객체에서 작동한다는 것이다.
				그래서 객체의 프로퍼티를 검사하는 방법으로 사용한다. 반면에 for..of는 주로 이터러블 객체의 값에 중점을 둔다.
				Map과 Set 같은 내장 객체는 저장된 값에 접근할 수 있는 Symbol.iterator 프로퍼티를 구현한다.

					let pets = new Set(["Cat", "Dog", "Hamster"]);
					pets["species"] = "mammals";

					for (let pet in pets) {
						console.log(pet); // "species"
					}
					for (let pet of pets) {
						console.log(pet); // "Cat", "Dog", "Hamster"
					}


		코드 생성

			ES5 및 ES3 타겟팅

				ES5 또는 ES3 호환 엔진을 대상으로 하는 경우, 반복자는 Array 타입 값만 허용한다.
				Symbol.iterator 프로퍼티를 구현하더라도 Array가 아닌 값에 for..of 루프를 사용하면 오류가 발생한다.

				컴파일러는 for..of에 대한 간단한 for 루프를 생성한다. 예를 들면

					let number = [1, 2, 3];
					for (let num of numbers) {
						console.log(num);
					}

				컴파일러는 위 코드르 다음과 같이 생성한다.

					var numbers = [1, 2, 3];
					for (var _i = 0; _i < numbers.length; _i++) {
						var num = numbers[_i];
						console.log(num);
					}

				ECMAScript 2015 및 상위 버전 타겟팅

					ECMAScript 2015 호환 엔진을 대상으로 하는 경우, 컴파일러는 엔진에서 내장 반복자 구현을 대상으로 하는 for..of 루프를 생성한다.
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
