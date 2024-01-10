<template>
	<div class="card" style="width: 18rem">
		<div class="card-body">
			<h5 class="card-title red">Card title</h5>
			<!-- <p class="card-text" :class="classes.red">Some quick example text to build on the card title and make up the bulk of the card's content.</p> -->
			<p class="card-text">
				Some quick example text to build on the card title and make up the bulk
				of the card's content.
			</p>
			<a href="#" class="btn btn-primary">Go somewhere</a>
		</div>
	</div>
</template>

<script>
import { ref } from 'vue';
export default {
	setup() {
		const color = ref('red');
		color.value = 'blue';
		return { color };
	},
};
</script>

<!-- scoped를 선언하면 해당 컴포넌트에서만 적용-->
<!-- <style scoped>
.red {
  color: red
}
</style> -->
<!-- 
<style module="classes">
.red{
  color: red
}
</style> -->

<style>
.red {
	color: v-bind(color);
}
</style>
<style></style>
