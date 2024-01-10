<template>
	<div class="card" style="width: 10rem">
		<div class="card-body">
			<!-- type: news, notice -->
			<span class="badge bg-secondary">{{ typeName }}</span>
			<h5 class="card-title mt-2">{{ title }}</h5>
			<!-- <p class="card-text" :class="classes.red">Some quick example text to build on the card title and make up the bulk of the card's content.</p> -->
			<p class="card-text">{{ contents }}</p>
			<!-- <a v-if="isLike" href="#" class="btn btn-danger">좋아요</a>
      <a v-else href="#" class="btn btn-outline-danger">좋아요</a> -->
			<a href="#" class="btn" :class="isLikeClass" @click="toggleLike"
				>좋아요</a
			>
		</div>
	</div>
</template>

<script>
import { computed } from 'vue';

export default {
	// props: ['title', 'contents'],문자열로 간단하게 선언할 수 있지만 권장하지 않음
	props: {
		type: {
			type: String,
			default: 'news',
			validator: value => {
				return ['news', 'notice'].includes(value);
			},
		},
		title: {
			type: String,
			required: true,
		},
		contents: {
			type: String,
			// required: true,
		},
		isLike: {
			type: Boolean,
			default: false,
		},
		obj: {
			type: Object,
			default: () => {},
		},
	},
	emits: ['toggleLike'],
	setup(props, context) {
		const isLikeClass = computed(() =>
			props.isLike ? 'btn-danger' : 'btn-outline-danger',
		);
		const typeName = computed(() =>
			props.type === 'news' ? 'news' : 'notice',
		);
		const toggleLike = () => {
			context.emit('toggleLike');
			//하위 컴포넌트에서 부모 컴포넌트로 이벤트 전달

			//props.isLike = !props.isLike
			//하위 컴포넌트에서 상위에서 내려주는 props의 값을 변경하려고하면 에러
		};
		return { isLikeClass, typeName, toggleLike };
	},
};
</script>

<style></style>
