<template>
	<div>
		<TheNav />
		<TheVue />
	</div>
</template>

<script>
import TheNav from '@/components/TheNav.vue';
import TheVue from '@/components/TheVue.vue';
// import TheVue2 from '@/components2/TheVue2.vue';
// import TheNav2 from '@/components2/TheNav2.vue';
// import TheVue3 from '@/components3/TheVue3.vue';
// import TheNav3 from '@/components3/TheNav3.vue';

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
