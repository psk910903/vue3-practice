<template>
	<div>
		<p>{{ message }}</p>
		<p>{{ message.value }}</p>
		<button @click="addMessage">add click</button>
	</div>
</template>

<script>
import { reactive } from 'vue';
export default {
	setup() {
		let message = reactive({
			value: 'Hello Vue!',
		});
		const addMessage = () => {
			message.value = message.value + '!';
		};
		console.log('message: ', message.value);
		console.log('message typeof: ', typeof message.value); //string

		//자바스크립트 언어의 특성
		// let message = reactive('Hello Vue!')
		// const addMessage = () => {
		//   message = message + '!'
		// }
		// console.log('message: ', message)
		// console.log('message typeof: ', typeof message) //string
		//message = message + '!' 에서 message 값 자체를 새로 바꿔버리기 때문에
		// {{ }}로 동기화를 하려면 message의 메모리주소를 서로 가지고 있고 그 값을 서로 공유해야 함

		return {
			message,
			addMessage,
		};
	},
};
</script>

<style lang="scss" scoped></style>
