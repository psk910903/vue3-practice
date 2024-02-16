<template>
	<div></div>
</template>
<!--

  createApp()

    createApp() 함수는 Vue 애플리케이션을 생성하는 데 사용된다.
    이것은 Vue.js 애플리케이션의 진입점(entry point)역할을 한다. 기본적으로 Vue3에서는 전체 애플리케이션을 직접 마운트하지 않고,
    대신에 'createApp()'을 사용하여 애플리케이션 인스턴스를 만들고, 이후 이를 마운트할 때 사용된다.

      //createApp() 사용 예시
      import { createApp } from 'vue';
      import App from './App.vue';

      const app = createApp(App);

      app.mount('#app');

    여기서 createApp()은 App 컴포넌트를 기반으로 새로운 Vue 애플리케이션을 생성한다.
    'App'은 일반적으로 애플리케이션의 루트 컴포넌트이다.
    그런 다음 'app.mount('#app')'을 호출하여 이 애플리케이션을 HTML DOM 요소에 마운트한다.
    '#app'은 마운트할 DOM 요소의 CSS 선택자이다.

    'createApp()'은 단순히 Vue 애플리케이션 인스턴스를 생성하는 것뿐만 아니라, 
    그 애플리케이션 인스턴스에 필요한 전역 설정, 플러그인, 컴포지션 API의 전역 믹스인 등을 추가할 수 있다.

    예를 들어, 다음은 Vuex 스토어를 Vue 3 애플리케이션에 추가하는 방법이다.

      import { createApp } from 'vue';
      import App from './App.vue';
      import store from './store';

      const app = createApp(App);
      app.use(store);
      app.mount('#app');

    여기서 'app.use(store)'는 Vuex 스토어를 애플리케이션에 추가하고, 이제 앱 전체에서 상태를 관리할 수 있도록 만든다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  요약

    타입

      //ts
      function createApp(rootComponent: Component, rootProps?: object): App

    세부 사항

      첫 번째 인자는 루트 컴포넌트이다. 선택적인 두 번째 인자는 루트 컴포넌트에 전달할 props이다.

    예제

      루트 컴포넌트를 직접 서술하는 방식(inline)으로 사용하는 경우

        import { createApp } from 'vue'

        const app = createApp({
          /* 루트 컴포넌트 옵션 */
        })

      컴포넌트 불러오기(import)로 사용하는 경우

        import { createApp } from 'vue'
        import App from './App.vue'

        const app = createApp(App)
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
