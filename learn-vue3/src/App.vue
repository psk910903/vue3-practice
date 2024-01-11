<template>
	<div>
		<!-- <TheNav />
		<TheVue /> -->
		<!-- <TheNav2 />
		<TheVue2 /> -->
		<TheNav3 />
		<TheVue3 />
	</div>
</template>

<script>
import TheNav from '@/components/TheNav.vue';
import TheVue from '@/components/TheVue.vue';
import TheVue2 from '@/components2/TheVue2.vue';
import TheNav2 from '@/components2/TheNav2.vue';
import TheVue3 from '@/components3/TheVue3.vue';
import TheNav3 from '@/components3/TheNav3.vue';

export default {
	components: {
		// TheNav,
		// TheVue,
		// TheVue2,
		// TheNav2,
		TheVue3,
		TheNav3,
	},
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
