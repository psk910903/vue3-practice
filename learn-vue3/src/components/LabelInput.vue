<template>
	<label>
		{{ label }}
		<!-- <input type="text" v-model="userName" /> -->
		<input type="text" v-model="value" />
	</label>
	<!-- :value="modelValue"
		@input="$emit('update:modelValue', $event.target.value)" -->
</template>

<script>
import { computed } from 'vue';

export default {
	props: ['userName', 'label'], //로컬 참조의 값이 동기화되는 prop
	emits: ['update:userName'], //로컬 참조의 값이 변경될 때 발생하는 이벤트
	setup(props, { emit }) {
		const value = computed({
			get() {
				return props.userName;
			},
			set(value) {
				emit('update:userName', value);
			},
		});
		return { value };
	},
};
</script>

<style lang="scss" scoped></style>
