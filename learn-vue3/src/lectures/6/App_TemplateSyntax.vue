<template lang="">
	<div>
		<h2>보간법</h2>
		<div>{{ message }}</div>
		<div v-once>{{ message }}</div>
		<button @click="message = message + '!'">Click</button>
		<hr />
		<h2>HTML</h2>
		<p>텍스트: {{ rawHtml }}</p>
		<p>v-html: <span v-html="rawHtml"></span></p>
		<hr />
		<h2>속성 바인딩</h2>
		<div title="안녕하세요">마우스를 올려보세요</div>
		<div v-bind:title="dynamicTitle">마우스를 올려보세요</div>
		<input type="text" value="홍길동" disabled="true" />
		<br />
		<input type="text" value="홍길동" v-bind:disabled="isInputDisabled" />
		<br />
		<br />
		<div>단축속성 v-bind: -> : 가능</div>
		<div :title="dynamicTitle">마우스를 올려보세요</div>
		<input type="text" value="홍길동" :disabled="isInputDisabled" />
		<br />
		<br />
		<div>다중속성 가능</div>
		<input v-bind="attrs" />
		<h2>JavaScript</h2>
		{{ message.split('').reverse().join('') }} <br />
		{{ isInputDisabled ? '예' : '아니오' }}
	</div>
</template>
<script>
import { ref } from 'vue';
export default {
	setup() {
		const message = ref('안녕하세요');
		const rawHtml = ref('<strong>안녕하세요</strong>');
		const dynamicTitle = ref('안녕하세요!!');
		const isInputDisabled = ref(true);
		const attrs = ref({
			type: 'password',
			value: '12345678',
			disabled: false,
		});
		return {
			message,
			rawHtml,
			dynamicTitle,
			isInputDisabled,
			attrs,
		};
	},
};
//텍스트 보간법

//데이터 바인딩의 가장 기본형태는 {{ data }} (이중 중괄호)를 사용하는 것이다.
//이중 중괄호를 사용하면 해당 문법은 컴포넌트 인스턴스의 message 값으로 대체된다.
//message 속성이 변경될 때 마다 갱신(반응)된다.

//v-once 디렉티브를 사용하여 데이터가 변경되어도 갱신되지 않는 일회성 보간을 수행할 수 있다.

//HTML(v-html)
//이중 중괄호는 데이터를 HTML이 아닌 일반 텍스트로 해석한다. 실제 HTML을 출력하려면 v-html 디렉티브를 사용해야 한다.
//웹사이트에서 임의의 HTML을 동적으로 렌더링하면 XSS 취약점으로 쉽게 이어질 수 있고 이는 매우 위험할 소지가 있음
//HTML 보간법은 반드시 신뢰할 수 있는 콘텐츠에서만 사용
//사용자가 제공한 콘텐츠에서는 절대 사용하면 안됨.

//속성 바인딩(v-bind)
//이중 중괄호는 HTML 속성에 사용할 수 없다. v-bind 디렉티브를 사용해야 한다.
//단축속성도 가능 v-bind: -> : 로 표기가 가능하다.
</script>
<style lang=""></style>
