<template>
	<div></div>
</template>
<!--  

  app.provide()

    app.provide() 메서드는 Vue 3 애플리케이션의 루트 컴포넌트에서 데이터를 제공하는 데 사용된다.
    이 메서드를 사용하면 하위 컴포넌트에서 inject를 사용하여 해당 데이터에 접근할 수 있다.
    이를 통해 전역 상태, 설정, 인스턴스 또는 기타 종속성을 하위 컴포넌트에 쉽게 전달할 수 있다.

    app.provide()를 사용하여 데이터를 제공하는 구문은 다음과 같다.

      app.provide(key, value);

    여기서

      - key : 제공할 데이터의 고유한 키이다. 이 키를 사용하여 하위 컴포넌트에서 해당 데이터를 가져온다.
      - value : 제공할 데이터이다. 이 값은 객체, 함수, 클래스 등이 될 수 있다.

    예를 들어, 애플리케이션에서 전역으로 사용할 테마 설정을 제공하는 방법을 살펴보자

      import { createApp } from 'vue';
      import App from './App.vue';

      const app = createApp(App);

      const theme = 'dark';

      app.provide('theme', theme);

      app.mount('#app');

    위의 예제에서 app.provide('theme', theme)는 theme라는 키로 테마 설정을 제공한다.
    이제 하위 컴포넌트에서 이 데이터에 엑세스할 수 있다.

    예를 들어, 하위 컴포넌트에서 inject를 사용하여 제공된 데이터를 가져와 사용하는 방법은 다음과 같다.

      import { inject } from 'vue';

      export default {
        mounted() {
          const theme = inject('theme');
          console.log('Theme:', theme); // 'dark'
        }
      };

    위의 코드에서 inject('theme')는 루트 컴포넌트에서 제공한 테마 설정을 가져온다.

    app.provide()를 사용하면 애플리케이션의 데이터를 쉽게 전역으로 공유할 수 있으며, 하위 컴포넌트에서 해당 데이터를 필요로 할 때 쉽게 엑세스할 수 있다.
    그러나 주의할 점은 'provide'와 'inject'를 사용하여 상태를 관리할 때 컴포넌트 간의 의존성이 강해질 수 있으므로 적절한 사용이 필요하다.

    

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
