<template>
	<div>
		<!--  
      Single-File Component(SFC)

      개요
        Vue에서 Single-File Component(SFc,*.vue 파일)는 Vue 컴포넌트의 템플릿(template), 로직(script), 스타일(style)을
        하나의 파일로 캡슐화하는 특수 파일 형식이다. 확장자는 *.vue이다.

      언어블록
        <template>
          - 각 *.vue 파일은 한 번에 최대 하나의 top-level <template> 블록을 포함할 수 있다.
          - 콘텐츠는 추출되어 @vue/compiler-dom 으로 전달되고, 
            JavaScript 렌더 기능으로 사전 컴파일되고, render 옵션으로 내보내어 컴포넌트에 첨부된다.
        <script>
          - 각 *.vue 파일은 한 번에 최대 하나의 <script> 블록을 포함할 수 있다.(<script setup> 제외)
          - 스크립트는 ES 모듈로 실행된다.
          - default export는 일반 객체 또는 defineComponent의 반환 값으로 Vue 컴포넌트 옵션 객체여야 한다.
        <script setup>
          - 각 *.vue 파일은 한 번에 최대 하나의 <script setup> 블록을 포함할 수 있다.(normal <script> 제외)
          - <script setup>은 사전에 처리되어 컴포넌트의 setup() 함수로 사용된다. 즉, 컴포넌트의 각 인스턴스에 대해 실행된다. 
            <script setup>의 최상위 바인딩은 템플릿에 자동으로 노출된다.
        <style>
          - 단일 *.vue 파일에는 여러 <style> 태그가 포함될 수 있다.
          - <style> 태그는 현재 컴포넌트에 스타일을 캡슐화하는 데 도움이 되도록 scoped 또는 module 속성을 가질 수 있다.
            캡슐화 모드가 다른 여러 <style> 태그를 동일한 구성 요소에서 혼합할 수 있다.

      Custom Blocks
        프로젝트별 요구사항에 따라 *.vue 파일에 사용자 정의 블록을 추가할 수 있다.

      전처리기
        <script>의 lang속성을 사용하여 전처리기 언어를 선언할 수 있다.
        일반적인 경우는 TypeScript를 사용하는 것이다.
          <script lang="ts">
            // use TypeScript
          </script>
        lang 속성은 모든 블록에 적용할 수 있다. 예를 들어 sass와 pug를 <style>과 <template>에 적용할 수 있다.
          <template lang="pug">
            p {{ msg }}
          </template>

          <style lang="scss">
            $primary-color: #333;
            body {
              color: $primary-color;
            }
          </style>
      
      Src 가져오기
        .vue 컴포넌트를 여러 파일로 분할하려는 경우 src 속성을 사용하여 language block에 대한 외부 파일을 가져올 수 있다.
          <template src="./template.html"></template>
          <style src="./style.css"></style>
          <script src="./script.js"></script>
        scr로 가져오는 것은 webpack 모듈 요청과 동일한 경로 확인 규칠을 따른다. 즉 다음을 의미한다.
        상대 경로는 ./로 시작해야 한다.
        npm 종속성에서 리소스를 가져올 수 있다.
      
      CSS 기능
        Scoped CSS
        <style> 태그에 scoped 속성이 있는 경우 해당 CSS는 현재 컴포넌트의 요소에만 적용된다.
          <template>
            <p class="greeting">greeting</p>
          </template>
          <style scoped>
          .greeting {
            color: red;
            font-weight: bold;
          }
          </style>
      
      CSS 모듈
        <style module>은 CSS 모듈로 컴파일되고, CSS 클래스 $style 객체의 속성을 노출한다.
        
    -->
	</div>
</template>

<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
