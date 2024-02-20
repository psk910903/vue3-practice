<template>
	<div></div>
</template>
<!--  

  app.version

    Vue.js에서 app.version은 애플리케이션의 버전을 나타내는 프로퍼티이다. 
    이 속성은 Vue.js의 생성된 애플리케이션 객체(app)에 포함되어 있으며, Vue CLI로 생성된 프로젝트에서 사용될 수 있다.

    Vue CLI로 프로젝트를 생성하면 package.json 파일에 프로젝트의 버전이 포함되어 있다.
    Vue CLI는 이 버전 정보를 읽어와서 app.version 속성으로 노출한다. 이것은 일반적으로 프로덕션 환경에서 릴리즈 버전을 식별하는 데 사용된다.

    예를 들어, Vue CLI로 생성된 프로젝트의 main.js 파일에서 app.version을 사용하여 애플리케이션의 버전을 알아볼 수 있다.

      import { createApp } from 'vue';
      import App from './App.vue';

      const app = createApp(App);

      console.log('App version:', app.version);

      app.mount('#app');

    이 코드는 콘솔에 애플리케이션의 버전을 출력한다. 이러한 버전 정보는 사용자에게 애플리케이션의 현재 버전을 표시하거나, 디버깅 목적으로 사용될 수 있다.

    app.version은 Vue CLI를 통해 생성된 프로젝트에서만 사용할 수 있으며, 일반적인 프로덕션 환경에서 빌드된 애플리케이션에서 버전을 식별하는데 사용된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    입을 생성한 Vue 버전을 제공한다. 이것은 Vue 버전에 기반한 플러그인 내부에서 조건부 로직이 필요한 경우에 유용하다.

    타입

      interface App {
        version: string
      }

    예제

      플러그인 내부에서 버전 확인 진행

        export default {
          install(app) {
            const version = Number(app.version.split('.')[0])
            if (version < 3) {
              console.warn('이 플러그인은 Vue 3가 필요합니다.')
            }
          }
        }
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
