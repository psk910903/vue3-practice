<template>
	<label for="">
		{{ inputLabel }}
		<input type="text" v-model="inputValue" @input="emitEvent" />
	</label>
</template>

<script>
import { ref } from 'vue';

export default {
	props: {
		name: {
			type: String,
			required: true,
		},
	},
	setup(props, { emit }) {
		const inputLabel = ref(props.name);
		const inputValue = ref('');
		const emitEvent = () => {
			emit('emitEvent', inputValue);
		};
		return { inputLabel, inputValue, emitEvent };
	},
};
</script>

<style lang="scss" scoped></style>
