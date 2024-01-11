<template>
	<!-- 
    2.
    현재 부모 컴포넌트에서 class, id, event 등의 속성 상속을 하고싶은데 
    inheritAttrs: false, 설정으로 상속을 못받고 있다
    루트 엘리먼트 div가 아닌 button에 상속하고 싶다면
  -->
	<div class="p-3 bg-danger">
		<!-- 4. v-bind="$attrs"로 적용 가능 -->
		<button class="btn btn-primary" type="button" v-bind="$attrs">
			My Button
		</button>
	</div>
</template>

<script>
export default {
	//1. 부모 컴포넌트에서 내려주는 class, id, event 등의 속성 상속이 안받음
	inheritAttrs: false,
	setup(props, context) {
		//3. setup 함수의 두번째 매개변수인 context의 attrs로 접근 가능
		// console.log(context.attrs);
		// console.log(context.attrs.class);
		// console.log(context.attrs.id);
		// console.log(context.attrs.onClick);
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
