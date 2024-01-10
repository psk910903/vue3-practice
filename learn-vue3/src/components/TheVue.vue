<template>
	<main>
		<div class="container py-4">
			<div class="row g-3">
				<!-- <div class="col-4">
          <AppCard title="제목" contents="내용"/>
        </div>
        <div class="col-4">
          <AppCard :title="post.title" :contents="post.contents"/>
        </div> -->
				<div v-for="post in posts" :key="post.id" class="col-4">
					<AppCard
						:title="post.title"
						:contents="post.contents"
						:type="post.type"
						:is-like="post.isLike"
						@toggle-like="post.isLike = !post.isLike"
					/>
				</div>
			</div>
		</div>
	</main>
</template>

<script>
import AppCard from '@/components/AppCard.vue';
import { reactive } from 'vue';
export default {
	components: {
		AppCard,
	},
	setup() {
		const post = reactive({
			title: '제목2',
			contents: '내용2',
		});

		const posts = reactive([
			{ id: 1, title: '제목1', contents: '내용1', isLike: true, type: 'news' },
			{ id: 2, title: '제목2', contents: '내용2', isLike: true, type: 'news' },
			{ id: 3, title: '제목3', contents: '내용3', isLike: true, type: 'news' },
			{ id: 4, title: '제목4', contents: '내용4', isLike: true, type: 'news' },
			{
				id: 5,
				title: '제목5',
				contents: '내용5',
				isLike: false,
				type: 'notice',
			},
			{ id: 6, title: '제목6', contents: '내용6', isLike: true, type: 'news' },
		]);

		return { post, posts };
	},
};
</script>

<style lang="scss" scoped></style>
