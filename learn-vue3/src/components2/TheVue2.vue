<template>
	<div class="py-3 px-3">
		<TheInputGroup @new-post="callback" />
		<div class="row">
			<div v-for="(post, index) in posts" class="col col-4 my-2" :key="index">
				<TheCard2
					:index="index"
					:type="post.type"
					:title="post.title"
					:content="post.content"
					:isLike="post.isLike"
					@toggleLike="post.isLike = !post.isLike"
				/>
			</div>
		</div>
		<hr />
		<TheInputLabel :name="'이름'" @emitEvent="callbackInput" />
		<hr />
		<UserName v-model:first-name="first" v-model:last-name="last" />
	</div>
</template>

<script>
import TheCard2 from '@/components2/TheCard2.vue';
import TheInputGroup from '@/components2/TheInputGroup.vue';
import TheInputLabel from '@/components2/TheInputLabel.vue';
import UserName from '@/components2/UserName.vue';
import { ref } from 'vue';
export default {
	components: {
		TheCard2,
		TheInputGroup,
		TheInputLabel,
		UserName,
	},
	setup() {
		const posts = ref([
			{ type: 'news', title: '제목1', content: '내용1', isLike: true },
			{ type: 'news', title: '제목2', content: '내용2', isLike: false },
			{ type: 'notice', title: '제목3', content: '내용3', isLike: true },
			{ type: 'news', title: '제목4', content: '내용4', isLike: true },
			{ type: 'news', title: '제목5', content: '내용5', isLike: true },
		]);
		const callback = newPost => {
			posts.value.push(newPost);
		};
		const callbackInput = inputValue => {
			//하위 컴포넌트에서 전달받은 input value
			// console.log('inputValue.value', inputValue.value);
		};

		const first = ref('');
		const last = ref('');
		return { posts, callback, callbackInput, first, last };
	},
};
</script>

<style lang="scss" scoped></style>
