<template>
	<div></div>
</template>
<!--  

  Triple-Slash Directives

    타입스크립트의 Triple-Slash Directives는 주로 TypeScript 파일에서 사용되며, 컴파일러에게 추가적인 정보를 제공한다.
    이 디렉티브는 주로 세 개의 슬래시(///)로 시작하며, 다른 TypeScript 파일이나 라이브러리의 정의 파일과 같은 외부 종속성을 지정하는 데 사용된다.

    Triple-Slash Directives는 세 가지 주요 유형이 있다.

    1. '/// <reference path="..." />' 
      이 디렉티브는 다른 TypeScript 파일을 현재 파일에 참조할 때 사용된다.
      이를 통해 컴파일러가 참조하는 파일을 알 수 있게 해준다. 주로 정의 파일(*.d.ts)을 참조할 때 사용된다.
      예를 들어,
        /// <reference path="myLibrary.d.ts" />

    2. '/// <reference types="..." />'
      이 디렉티브는 @types 패키지에서 가져온 타입 정의를 사용할 때 사용된다.
      이는 주로 외부 라이브러리나 프레임워크에 대한 타입 정의를 가져올 때 사용된다.
      예를 들어,
        /// <reference types="node" />

    3. '/// <reference lib="..." />'
      이 디렉티브는 특정 JavaScript 런타임 환경에서 사용 가능한 내장 라이브러리를 선택할 때 사용된다.
      기본적으로 TypeScript는 브라우저 및 Node.js와 같은 환경에서 기본 라이브러리를 제공한다.
      이 디렉티브를 사용하여 특정 라이브러리를 사용하도록 설정할 수 있다.
      예를 들어,
        /// <reference lib="es2017.object" />

    Triple-Slash 디렉티브는 TypeScript 컴파일러에게 추가적인 정보를 제공함으로써 프로젝트를 구성하고 외부 종속성을 관리하는 데 도움을 준다.
    그러나 최신 버전의 TypeScript에서는 대부분 tsconfig.json 파일을 사용하여 구성을 관리하므로 Triple-Slash 디렉티브를 사용하는 것이 필요한 경우는 줄어들었다.
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
