<template>
	<div class="container py-3 px-3">
		<CreatePost3 @createPost="createPost" />
		<hr />
		<div class="row">
			<div v-for="(post, index) in posts" :key="index" class="col-4 g-3">
				<TheCard3
					:type="post.type"
					:news="post.news"
					:title="post.title"
					:content="post.content"
					:isLike="post.isLike"
					@toggleLike="post.isLike = !post.isLike"
				/>
			</div>
		</div>
		<hr />
		<TheInputLabel @inputChangeValue="inputChangeValue1" />
		<TheInputLabel @inputChangeValue="inputChangeValue2" :label="'이름2'" />
		<hr />
		<UserName v-model:first-name="firstName" v-model:last-name="lastName" />
	</div>
</template>

<script>
import TheCard3 from '@/components3/TheCard3.vue';
import CreatePost3 from '@/components3/CreatePost3.vue';
import TheInputLabel from '@/components3/TheInputLabel.vue';
import UserName from '@/components3/UserName.vue';

import { ref } from 'vue';
export default {
	components: {
		TheCard3,
		CreatePost3,
		TheInputLabel,
		UserName,
	},
	setup() {
		const posts = ref([
			{ type: 'news', title: '제목1', content: '내용1', isLike: true },
			{ type: 'notice', title: '제목2', content: '내용2', isLike: true },
			{ type: 'news', title: '제목3', content: '내용3', isLike: true },
			{ type: 'news', title: '제목4', content: '내용4', isLike: false },
			{ type: 'news', title: '제목5', content: '내용5', isLike: true },
		]);
		const createPost = newPost => {
			posts.value.push(newPost);
		};
		const inputChangeValue1 = newValue => {
			console.log('newValue1', newValue.value);
		};
		const inputChangeValue2 = newValue => {
			console.log('newValue2', newValue.value);
		};
		const firstName = ref('');
		const lastName = ref('');
		return {
			posts,
			createPost,
			inputChangeValue1,
			inputChangeValue2,
			firstName,
			lastName,
		};
	},
};
</script>

<style lang="scss" scoped></style>
