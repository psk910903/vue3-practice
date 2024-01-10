<template>
	<div>
		<button @click="increment">Click {{ state.count }}</button>
		<button @click="increment">Deep Click {{ state.deep.count }}</button>
	</div>
</template>

<script>
import { reactive } from 'vue';
export default {
	setup() {
		const state = reactive({
			count: 0,
			deep: {
				count: 0,
			},
		});
		const increment = () => {
			state.count++;
			state.deep.count++;
		};
		return {
			state,
			increment,
		};
	},
};
//반응형 상태 선언하기
//JavaScript 객체에서 반응형 상태를 생성하기 위해서는 reactive() 함수를 사용할 수 있다.

//컴포넌트<template> 에서 반응형 객체를 사용하려면 setup() 함수에서 선언하고 리턴해야 한다.
//반환된 상태는 반응형 객체이다. 반응형 변환은 "깊다"
//컴포넌트의 data()에서 객체를 반환할 때, 이것은 내부적으로 reactive()에 의해 반응형으로 만들어진다.
</script>

<style lang="scss" scoped></style>
