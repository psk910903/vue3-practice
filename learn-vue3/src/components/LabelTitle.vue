<template>
	<label>
		{{ label }}
		<input type="text" v-model="value" />
	</label>
</template>

<script>
import { computed } from 'vue';

export default {
	props: ['content', 'label'], //값을 입력 받음
	emits: ['update:content'], //이벤트 발생
	setup(props, { emit }) {
		const value = computed({
			get() {
				return props.content;
			},
			set(value) {
				emit('update:content', value);
			},
		});
		return { value };
	},
};
</script>

<style lang="scss" scoped></style>
