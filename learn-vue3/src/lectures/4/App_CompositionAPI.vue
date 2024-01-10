<template>
	<div>
		<h2>반응형 메시지</h2>
		<p>{{ reavtiveMessage }}</p>
		<button v-on:click="addReactiveMessage">Add Message</button>
		<h2>일반 메시지</h2>
		<p>{{ normalMessage }}</p>
		<button v-on:click="addNormalMessage">Add Message</button>
	</div>
</template>

<script>
import { isRef, onBeforeMount, onMounted, ref } from 'vue';
export default {
	setup() {
		console.log('setup 1');
		//Composition API

		//1. 반응형 API
		//ref() : 반응형 데이터
		//isRef() : 반응형 데이터 검증. boolean 값으로 반환

		const reavtiveMessage = ref('Hello reavtive message'); //반응형
		const normalMessage = 'Hello normal message'; //일반형
		const addReactiveMessage = () => {
			reavtiveMessage.value = reavtiveMessage.value + '!';
		};
		const addNormalMessage = () => {
			normalMessage.value = normalMessage.value + '!';
		};
		console.log('isRef(reavtiveMessage)', isRef(reavtiveMessage));
		console.log('isRef(normalMessage)', isRef(normalMessage));

		//2. 라이프 사이클 훅
		//라이프 사이클 : Vue 인스턴스가 생성된 후 우리 눈에 보여지고, 사라지기까지의 단계
		//Vue 인스턴스의 라이프사이클 : 생성(create) -> 부착(mount) -> 업데이트(update) -> 없어짐(destroy)
		//라이프 사이클 훅 : 라이프사이클 단계에서 실행되는 함수
		onMounted(() => {
			console.log('onMounted 3');
		});
		onBeforeMount(() => {
			console.log('onBeforeMount 2');
		});
		return {
			reavtiveMessage,
			normalMessage,
			addReactiveMessage,
			addNormalMessage,
		};
	},
};
</script>

<style lang="scss" scoped></style>
