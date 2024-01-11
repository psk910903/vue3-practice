<template>
	<main>
		<div class="container py-4">
			<div>
				<MyButton class="my-button" id="my-button" @click="sayHello"></MyButton>
				<LabelInput :label="'이름'"></LabelInput>
				<!-- 
					현재 LabelInput은 다중노드 상태임
					다중 속성은 루트 엘리먼트에 적용되는데 다중노드기 때문에 루트를 못찾아서 에러가 발생할 수 있음
					LabelInput 내부에서 어느 요소에 바인딩할지 명시적으로 표시해줘야 함
				-->
			</div>
		</div>
	</main>
</template>

<script>
import MyButton from '@/components/MyButton.vue';
import LabelInput from '@/components/LabelInput.vue';
export default {
	components: {
		MyButton,
		LabelInput,
	},
	setup() {
		const sayHello = () => {
			alert('안녕하세요');
		};
		return { sayHello };
	},
};
</script>

<style lang="scss" scoped></style>
