<template>
	<div class="card" style="width: 12rem">
		<div class="card-body">
			<span class="badge bg-secondary mb-3">{{ postType }}</span>
			<h5 class="card-title">{{ postTitle }}</h5>
			<p class="card-text">
				{{ postContent }}
			</p>
			<a href="#" class="btn" :class="isLikeClass" @click="toggleLike"
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
		},
		index: {
			type: Number,
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
		const isLikeClass = computed(() => {
			return props.isLike === true ? 'btn-danger' : 'btn-outline-danger';
		});
		const postType = computed(() => {
			return props.type === 'news' ? 'news' : 'notice';
		});
		const postTitle = props.title;
		const postContent = props.content;

		const toggleLike = () => {
			emit('toggleLike');
		};
		return { postType, postTitle, postContent, isLikeClass, toggleLike };
	},
};
</script>

<style scoped></style>
