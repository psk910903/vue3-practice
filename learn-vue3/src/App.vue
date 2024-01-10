<template>
	<div>
		<TheNav />
		<TheVue />
	</div>
</template>

<script>
import TheNav from '@/components/TheNav.vue';
import TheVue from '@/components/TheVue.vue';

export default {
	components: {
		TheNav,
		TheVue,
	},
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
