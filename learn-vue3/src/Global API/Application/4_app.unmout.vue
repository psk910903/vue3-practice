<template>
	<div></div>
</template>
<!--  

  app.unmount()

    app.unmount() 메서드는 Vue 애플리케이션을 DOM에서 언마운트(제거)하는 데 사용된다.
    Vue 3에서는 이 메서드를 사용하여 Vue 애플리케이션을 삭제하고, 애플리케이션의 모든 이벤트 처리기와 DOM 리스너를 제거할 수 있다.

    일반적으로 'app.unmout()'는 Vue 애플리케이션의 라이프사이클 중 하나인 "소멸"단계에서 호출된다.
    애플리케이션을 언마운트하는 것은 해당 애플리케이션이 더 이상 필요하지 않을 때 발생할 수 있다.
    예를 들어, SPA(Single Page Application)에서 라우팅을 사용하여 새로운 페이지로 이동할 때 이전 페이지의 애플리케이션을 언마운트할 수 있다.

      import { createApp } from 'vue';
      import App from './App.vue';

      const app = createApp(App);

      // 앱을 #app 요소에 마운트한다.
      app.mount('#app');

      // 일정 시간 후 앱을 언마운트한다.
      setTimeout(() => {
        app.unmount();
      }, 5000);

    위의 코드에서 'app.unmout()'는 5초 후에 호출되어 애플리케이션을 DOM에서 제거한다.
    이는 간다한 데모일 뿐이며, 실제 애플리케이션 수명 주기를 관리할 수 있다.

    app.unmout()는 Vue 2에서의 vm.$destory() 메서드와 유사하다. 이를 사용하면 Vue 애플리케이션을 깔끔하게 정리하고, 메모리 누수를 방지할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  마운트된 앱 인스턴스를 마운트 해제하여, 앱의 컴포넌트 트리에 있는 모든 컴포넌트에 마운트 해제 생명 주기 훅을 트리거한다.

  타입

    interface App {
      unmount(): void
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
