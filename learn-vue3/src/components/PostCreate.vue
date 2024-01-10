<template>
	<div class="row g-3">
		<div class="col col-3">
			<select
				class="form-select"
				aria-label="Default select example"
				v-model="type"
			>
				<option selected value="news">news</option>
				<option value="notice">notice</option>
			</select>
		</div>
		<div class="col col-7">
			<input type="text" class="form-control" v-model="title" />
		</div>
		<div class="col col-2 d-grid">
			<button class="btn btn-primary" @click="createPost">추가</button>
		</div>
	</div>
</template>

<script>
import { ref } from 'vue';

export default {
	// emits: ['createPost'],
	emits: {
		//createPost: null 유효성 체크가 없을 때
		createPost: newPost => {
			if (!newPost.title || !newPost.type) {
				return false;
			}
			return true;
		},
	},
	setup(props, { emit }) {
		const type = ref('news');
		const title = ref('');
		const createPost = () => {
			const newPost = {
				type: type.value,
				title: title.value,
			};
			emit('createPost', newPost);
			type.value = 'news';
			title.value = '';
		};
		return { createPost, type, title };
	},
};
</script>

<style lang="scss" scoped></style>
