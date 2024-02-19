<template>
	<div></div>
</template>
<!--  

  app.use()

    app.use() 메서드는 Vue 애플리케이션에 Vue 플러그인을 추가하는 데 사용된다. Vue 플러그인은 Vue를 확장하거나 기능을 추가하는 데 사용되며,
    전역으로 엑세스할 수 있는 특정 기능이나 라이브러리를 제공할 수 있다.

    'app.use()'를 사용하여 플러그인을 추가하는 구문은 다음과 같다.

      app.use(plugin, option);

    여기서

    -'plugin': 설치할 플러그인이다. 이것은 객체 또는 플러그인을 제공하는 함수일 수 있다.
    -'option'(선택사항): 플러그인에 전달될 옵션이다.

    아래는 Vue Router를 Vue 애플리케이션에 추가하는 예제이다.

      import { createApp } from 'vue';
      import { createRouter, createWebHistory } from 'vue-router';
      import App from './App.vue';
      import Home from './views/Home.vue';

      const app = createApp(App);

      const router = createRouter({
        history: createWebHistory(),
        routes: [
          { path: '/', component: Home }
        ]
      });

      app.use(router);

      app.mount('#app');

    위의 예제에서 'app.use(router)'는 Vue Router를 애플리케이션에 추가한다.
    이제 애플리케이션에서 라우팅을 사용할 수 있게 된다.

    또 다른 예로는 Vuex를 사용하여 전역 상태 관리를 구현하는 것이 있다.

      import { createApp } from 'vue';
      import { createStore } from 'vuex';
      import App from './App.vue';

      const app = createApp(App);

      const store = createStore({
        state() {
          return {
            count: 0
          };
        },
        mutations: {
          increment(state) {
            state.count++;
          }
        }
      });

      app.use(store);

      app.mount('#app');

    이제 app.use(store)를 호출하여 Vuex 스토어를 애플리케이션에 추가했다.
    이제 애플리케이션의 모든 컴포넌트에서 전역 상태를 관리하고 업데이트할 수 있다.

    'app.use()'를 사용하여 플러그인을 추가하면 플러그인이 전역으로 엑세스할 수 있으며 
    애플리케이션의 모든 컴포넌트에서 사용할 수 있게 된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  플러그인 설치.

  타입

    interface App {
      use(plugin: Plugin, ...options: any[]): this
    }

  세부 사항

    첫 번째 인자는 플러그인이다. 선택적인 두 번째 인자는 플러그인 옵션이다.
    플러그인은 install() 메서드가 있는 객체이거나, install() 메서드로 사용될 함수이다.
    app.use()의 두 번째 인자인 옵션은 플러그인의 insatll() 메서드에 전달된다.

    동일한 플러그인을 여러 번 app.use()로 호출하는 경우, 플러그인은 한 번만 설치된다.

    예제

      import { createApp } from 'vue'
      import MyPlugin from './plugins/MyPlugin'

      const app = createApp({
        /* ... */
      })

      app.use(MyPlugin)

      
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
