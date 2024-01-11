<template>
	<label class="form-label">
		{{ label }}
	</label>
	<input v-bind="$attrs" type="text" v-model="value" class="form-control" />
</template>

<script>
import { computed } from 'vue';

export default {
	props: ['modelValue', 'label'], //로컬 참조의 값이 동기화되는 prop
	emits: ['update:modelValue'], //로컬 참조의 값이 변경될 때 발생하는 이벤트
	setup(props, { emit }) {
		const value = computed({
			get() {
				return props.modelValue;
			},
			set(value) {
				emit('update:modelValue', value);
			},
		});
		return { value };
	},
};
</script>

<style lang="scss" scoped></style>
