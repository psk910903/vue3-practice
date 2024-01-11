<template>
	<label for="">
		{{ labelText }}
		<input type="text" v-model="inputValue" @input="inputChangeValue" />
	</label>
</template>

<script>
import { ref } from 'vue';

export default {
	props: {
		label: {
			type: String,
			required: false,
			default: '이름',
		},
	},
	setup(props, { emit }) {
		const inputValue = ref('');
		const inputChangeValue = () => {
			emit('inputChangeValue', inputValue);
		};
		const labelText = ref(props.label);
		return { inputValue, inputChangeValue, labelText };
	},
};
</script>

<style lang="scss" scoped></style>
