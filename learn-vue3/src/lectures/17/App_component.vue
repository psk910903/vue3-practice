<template>
	<div>
		<!--  
      컴포넌트 정의
      컴포넌트 정의 방법
      1. Single-File Component(SFC) -> 실무에서 일반적으로 사용하는 방법
      2. 문자열 템플릿(string template)

      Single-File Component(SFC)
      빌드 도구를 사용할 때 컴포넌트는 일반적으로 Single-File Component(SFC)로 정의할 수 있다.
      SFC는 확장자 *.vue를 가진 단일 파일이다.

      문자열 템플릿(string template)
      빌드 도구를 사용하지 않을 때 컴포넌트 Vue 옵션인을 포함하는 일반 JavaScript 객체로 정의할 수 있다.


      컴포넌트 등록
      Vue 컴포넌트는 <template>안에서 발견 되었을 때 Vue가 구현 위치를 알 수 있도록 "등록"을 해야한다.
      컴포넌트를 등록하는 방법
      1. 전역 등록(Global Registration)
      1. 지역 등록(Local Registration)

      전역 등록
      app.component() 메서드를 사용하여 현재 Vue 애플리케이션에서 전역적으로 사용할 수 있도록 할 수 있다.

      지역 등록
      전역 등록은 편리하지만 다음과 같은 몇 가지 단점이 있다.
      1. Webpack(또는 Vite)과 같은 빌드 시스템을 사용하는 경우 컴포넌트를 전역 등록하는 것은 컴포넌트를 사용하지 않더라도
      계속해서 최종 빌드에 해당 컴포넌트가 포함되는 것을 의미한다.
      이는 사용자가 다운로드하는 자바스크립트 파일의 크기를 불필요하게 증가시킨다.
      2. 전역 등록을 계속 하게 되면 애플리케이션의 컴포넌트간 종속 관계를 확인하기 힘들다.
      상위 컴포넌트, 하위 컴포넌트 구분이 힘들면 유지보수를 하기에 매우 어렵다.

      지역 등록된 컴포넌트는 현재 컴포넌트 영역 안에서만 사용할 수 있다. Vue 컴포넌트 인스턴스의 components 옵션을 사용해서 등록할 수 있다.
      
    -->
		<button @click="counter++">클릭 횟수 {{ counter }}</button>
	</div>
</template>

<script>
import { ref } from 'vue';
export default {
	setup() {
		const counter = ref(0);

		return {
			counter,
		};
	},
};
</script>

<style lang="scss" scoped></style>
