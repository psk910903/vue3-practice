<template>
	<div></div>
</template>
<!--  
  
  createSSRApp()

    createSSRApp() 함수는 Vue.js 3에서 서버 사이드 렌더링(SSR)을 위한 애플리케이션을 생성하는 데 사용된다.
    SSR은 서버에서 Vue 앱을 렌더링하여 클라이언트에 더 빠르게 초기 로딩 및 검색 엔진 최적화(SEO)를 제공하는 기술이다.

    일반적인 createApp() 함수와는 달리 createSSRApp() 함수는 렌더링할 앱을 초기화하는 데 필요한 추가적인 내부 설정을 포함하여 SSR을 위해 최적화되어 있다.

    createSSRApp()을 사용하여 앱을 생성하는 기본적인 방법은 다음과 같다.

      import { createSSRApp } from 'vue';
      import App from './App.vue';

      const app = createSSRApp(App);

      export default app;

    일반적으로 createSSRApp() 함수로 생성된 앱은 서버에서 렌더링되므로,
    일반적으로 클라이언트에서만 사용되는 기능(예: 브라우저 API에 직접 접근하는 것)은 사용할 수 없다.
    이를 위해 클라이언트 전용 기능을 사용하려면 'isClient'와 같은 환경 변수를 사용하여 적절한 조건부 처리를 해야 한다.

    또한, 서버 사이드 렌더링을 구현하려면 Vue의 SSR 라이브러리와 함께 사용해야 한다.
    가장 인기 있는 Vue SSR 라이브러리 중 하나는 Vue Router와 Vuex를 포함한 전체 스택을 제공하는 Vue SSR 라이브러리인
    'vite-plugin-vue-server-side-render'이다.

      import { createSSRApp } from 'vue';
      import App from './App.vue';
      import { createRouter } from './router';
      import { createStore } from './store';

      export function createApp() {
        const app = createSSRApp(App);
        const router = createRouter();
        const store = createStore();

        app.use(router);
        app.use(store);

        return { app, router, store };
      }

    위의 예제에서는 createApp() 함수를 사용하여 서버 및 클라이언트 측에서 동일한 앱을 생성한다.
    서버에서는 라우터 및 스토어 인스턴스를 생성하고, 클라이언트에서는 이를 재사용하여 앱을 마운트한다.
    


-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
