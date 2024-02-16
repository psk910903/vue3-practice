<template>
	<div></div>
</template>
<!--  

  app.mount()

    app.mount() 메서드는 Vue 애플리케이션을 DOM에 마운트(렌더링)하는 데 사용된다.
    Vue 3에서는 이 메서드를 사용하여 Vue 애플리케이션을 HTML 문서의 특정 DOM 요소에 연결한다.

    일반적으로 이 메서드는 Vue 애플리케이션을 초기화한 후에 호출된다.
    다음은 app.mount()를 사용하여 Vue 애플리케이션을 마운트하는 간단한 예이다.

      import { createApp } from 'vue';
      import App from './App.vue';

      const app = createApp(App);

      app.mount('#app');

    위의 코드에서 app.mount('#app')은 App 컴포넌트를 사용하여 새로운 Vue 애플리케이션을 생성하고,
    이를 HTML 문서의 'id'가 'app'인 DOM 요소에 마운트한다. 즉, 'App'컴포넌트가 '#app' 요소 내에서 렌더링된다.

    mount() 메서드에 전달되는 인자는 문자열 CSS 선택자 또는 실제 DOM 요소 객체일 수 있다.
    Vue는 해당 선택자 또는 DOM 요소를 찾아 애플리케이션을 마운트한다.

      app.mount(document.getElementById('app'));

    또한, Vue 애플리케이션을 마운트하는 데 사용되는 'mount()'메서드는 Vue 2에서도 사용되었다.
    Vue 3에서는 'createApp()' 함수를 통해 애플리케이션을 생성한 후 'mount()'를 호출하여 마운트하지만,
    Vue 2에서는 'new Vue()'를 통해 애플리케이션을 생성하고, 이후에 'vm.$mount()'메서드를 사용하여 마운트했다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  요약

    컨테이너 엘리먼트에 앱 인스턴스를 마운트한다.

    타입

      //ts
      interface App {
        mount(rootContainer: Element | string): ComponentPublicInstance
      }

    세부 사항

      인자는 실제 DOM 엘리먼트 또는 CSS 선택자(첫 번째로 일치한 엘리먼트가 사용됨)일 수 있다.
      루트 컴포넌트 인스턴스를 반환한다.

      컴포넌트에 정의된 템플릿 또는 렌더링 한수가 있는 경우, 컨테이너 내부의 기존 DOM 노드를 대체한다.
      만약 런타임 컴파일러를 사용하는 경우, 컨테이너의 innerHTML 이 템플릿으로 사용된다.

      SSR 하이드레이션 모드에서는 컨테이너 내부의 기존 DOM 노드를 하이드레이트한다.
      불일치할 경우, 기존 DOM 노드가 의도하는 출력과 일치하도록 변경된다.

      각 인스턴스는 mount()를 한 번만 호출할 수 있다.



-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
