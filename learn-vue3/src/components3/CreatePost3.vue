<template>
	<div class="input-group">
		<select
			v-model="selectValue"
			class="form-select"
			aria-label="Example select with button addon"
		>
			<option value="news">news</option>
			<option value="notice">notice</option>
		</select>
		<input v-model="inputValue" type="text" class="col-8" />
		<button class="btn btn-outline-secondary" type="button" @click="createPost">
			추가
		</button>
	</div>
</template>

<script>
import { ref } from 'vue';

export default {
	setup(props, { emit }) {
		const selectValue = ref('news');
		const inputValue = ref('');
		const createPost = () => {
			if (validate()) {
				const newPost = {
					type: selectValue.value,
					title: inputValue.value,
				};
				emit('createPost', newPost);
				selectValue.value = 'news';
				inputValue.value = '';
			}
		};
		const validate = () => {
			if (!inputValue.value) {
				alert('제목을 입력해주세요');
				return false;
			}
			return true;
		};

		return { selectValue, inputValue, createPost };
	},
};
</script>

<style lang="scss" scoped></style>
