<template>
	<div class="card" style="width: 10rem">
		<div class="card-body">
			<span class="badge bg-secondary mb-3">{{ typeText }}</span>
			<h5 class="card-title">{{ postTitle }}</h5>
			<p class="card-text">
				{{ postContent }}
			</p>
			<a href="#" class="btn" :class="isLikeClass" @click="isLikeToggle"
				>좋아요</a
			>
		</div>
	</div>
</template>

<script>
import { computed, ref } from 'vue';

export default {
	props: {
		type: {
			type: String,
			requird: true,
		},
		title: {
			type: String,
			requird: true,
		},
		content: {
			type: String,
			requird: false,
			default: '내용입니다.',
		},
		isLike: {
			type: Boolean,
			requird: false,
			default: false,
		},
	},
	setup(props, { emit }) {
		const postTitle = ref(props.title);
		const postContent = ref(props.content);
		const typeText = computed(() => {
			return props.type === 'news' ? 'news' : 'notice';
		});
		const isLikeClass = computed(() => {
			return props.isLike === true ? 'btn-danger' : 'btn-outline-danger';
		});

		const isLikeToggle = () => {
			emit('toggleLike');
		};
		return { typeText, isLikeClass, postTitle, postContent, isLikeToggle };
	},
};
</script>

<style lang="scss" scoped></style>
