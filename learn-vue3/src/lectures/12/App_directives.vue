<template>
	<div>
		<!-- 
  디렉티브(directives)
  디렉티브는 v- 접두사가 있는 특수 속성이다.
  기능상에서 중요한 역할인 컴포넌트 (또는 DOM 요소)에게 "~~하게 작동하라"하고 지시하는 지문을 의미한다.

  Vue는 여러내장 디렉티브를 제공한다.
  v-text
  v-html
  v-show
  v-if
  v-else
  v-else-if
  v-for
  v-on(단축표기 @)
  v-bind(단축표기 :)
  v-model
  v-slot(단축표기 #)
  v-pre
  v-once
  v-cloak
  v-memo(v3.2+)

  디렉티브 구성
  - 디렉티브 : v- 접두사가 있는 특수 속성으로 
               디렉티브의 값(value)이 변경될 때 특정 효과를 반응적으로 DOM에 적용하는 것을 말한다.
  - 전달인자 : 일부 디렉티브는 디렉티브명 뒤에 콜론(:)으로 표기되는 전달인자를 가질 수 있다.
               예를 들어, v-bind 디렉티브는 반응적으로 html 속성을 갱신하는 데 사용한다.
  - 수식어 : 수식어는 점(.)으로 표시되는 특수 접미사로 디렉티브가 특별한 방식으로 바인딩되어야 함을 나타낸다.
-->

		<p>{{ msg }}</p>
		<p v-text="msg"></p>
		<p v-html="htmlMsg"></p>
		<p v-text="htmlMsg"></p>
		<p v-once>{{ msg }}!!!</p>
	</div>
</template>

<script>
import { ref } from 'vue';
export default {
	setup() {
		const msg = ref('안녕하세요');
		const htmlMsg = ref('<strong>안녕하세요</strong>');

		return {
			msg,
			htmlMsg,
		};
	},
};
</script>

<style lang="scss" scoped></style>
