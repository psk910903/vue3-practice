<template>
	<div></div>
</template>

<script>
import { ref, watch } from 'vue';
export default {
	setup() {
		// Watch
		//우리는 종종 반응형 상태가 변경 되었을 때 감지하여 다른 작업(api call 등)을 수행해야 하는 경우가 있다.
		//예를 들면 어떠한 상태가 변경되었을 때 DOM을 변경하거나 비동기 작업을 해서 다른 상태를 변경하는 일이다.
		//Composition API의 watch 함수를 사용하여 반응형 상태가 변경될 때마다 특정 작업을 수행할 수 있다.

		const message = ref('Hello World');

		//message 데이터 변경
		watch(message, (newValue, oldValue) => {
			console.log('newValue', newValue);
			console.log('oldValue', oldValue);
			//어떠한 작업을 수행하는 "감시자" 역할을 한다.
		});

		return {};
	},
};
</script>

<style lang="scss" scoped></style>
