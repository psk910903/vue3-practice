<template>
	<div class="input-group">
		<select
			v-model="selectType"
			class="form-select"
			id="inputGroupSelect04"
			aria-label="Example select with button addon"
		>
			<option value="news" selected>news</option>
			<option value="notice">notice</option>
		</select>
		<input v-model="title" type="text" class="col-8" placeholder="title" />
		<button class="btn btn-outline-secondary" type="button" @click="test">
			추가
		</button>
	</div>
</template>

<script>
import { ref } from 'vue';

export default {
	setup(props, { emit }) {
		const title = ref('');
		const selectType = ref('news');
		const test = () => {
			console.log('title', title.value);
			const newPost = {
				type: selectType.value,
				title: title.value,
			};
			console.log('newPost', newPost);
			emit('newPost', newPost);
			title.value = '';
			selectType.value = 'news';
		};
		return { title, selectType, test };
	},
};
</script>

<style lang="scss" scoped></style>
