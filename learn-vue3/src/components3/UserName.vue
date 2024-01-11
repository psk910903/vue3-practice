<template>
	<label for="">
		성
		<input
			type="text"
			:value="firstName"
			@input="$emit('update:firstName', $event.target.value)"
		/>
	</label>
	<label for="">
		이름
		<input
			type="text"
			:value="lastName"
			@input="$emit('update:lastName', $event.target.value)"
		/>
	</label>
</template>

<script>
export default {
	props: {
		firstName: String,
		lastName: String,
	},
	emits: ['update:firstName', 'update:lastName'],
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
