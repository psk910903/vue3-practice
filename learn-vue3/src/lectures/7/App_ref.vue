<template>
	<div>
		<p>{{ message }}</p>
		<button @click="addMessage">add click</button>
	</div>
</template>

<script>
import { ref } from 'vue';
export default {
	setup() {
		let message = ref('Hello Vue!');
		const addMessage = () => {
			message.value = message.value + '!';
		};
		console.log('message: ', message.value);
		console.log('message typeof: ', typeof message.value); //string

		//ref로 원시값 반응형 데이터 생성하기
		//reactive() 함수는 객체타입에만 동작한다.
		//그래서 기본타입(number, string, boolean)을 반응형으로 만들고자 할 때 ref 메소드를 사용할 수 있다.
		//ref 메서드는 변이가능한 객체를 반환한다. 이 객체 안에는 value라는 하나의 속성만 포함하는데
		//value값은 ref() 메서드에서 매개변수로 받은 값을 가지고 있다.
		//이 객체는 내부의 value 값에 대한 반응형 참조(reference)역할을 한다.
		//그래서 이름이 ref 임

		return {
			message,
			addMessage,
		};
	},
};
</script>

<style lang="scss" scoped></style>
