<template>
	<div>
		<!-- <TheNav />
		<TheVue /> -->
		<TheNav2 />
		<TheVue2 />
	</div>
</template>

<script>
import TheNav from '@/components/TheNav.vue';
import TheVue from '@/components/TheVue.vue';
import TheVue2 from '@/components2/TheVue2.vue';
import TheNav2 from '@/components2/TheNav2.vue';

export default {
	components: {
		// TheNav,
		// TheVue,
		TheVue2,
		TheNav2,
	},
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
