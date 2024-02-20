<template>
	<div></div>
</template>
<!--  

  defineAsyncComponent()

    렌더링될 대 지연 로드되는 비동기 컴포넌트를 정의한다. 인자는 로더 함수이거나 로드 동작의 고급 제어를 위한 옵션 객체일 수 있다.

    타입

      function defineAsyncComponent(
        source: AsyncComponentLoader | AsyncComponentOptions
      ): Component

      type AsyncComponentLoader = () => Promise<Component>

      interface AsyncComponentOptions {
        loader: AsyncComponentLoader
        loadingComponent?: Component
        errorComponent?: Component
        delay?: number
        timeout?: number
        suspensible?: boolean
        onError?: (
          error: Error,
          retry: () => void,
          fail: () => void,
          attempts: number
        ) => any
      }

    defineAsyncComponent() 함수는 Vue 3에서 비동기적으로 컴포넌트를 로드하고 정의하는 데 사용되는 함수이다.
    이 함수를 사용하면 필요한 경우에만 컴포넌트를 로드하여 성능을 최적화할 수 있다.
    특히 대규모 애플리케이션에서 코드 분할을 적용할 때 매우 유용하다.

    defineAsyncComponent() 함수를 사용하여 정의된 컴포넌트는 Vue의 createApp() 함수를 통해 생성된 Vue 애플리케이션에 등록할 수 있다.

    defineAsyncComponent() 함수는 다음과 같은 형식을 가진다.

      import { defineAsyncComponent } from 'vue';

      const MyAsyncComponent = defineAsyncComponent(() => import('./MyAsyncComponent.vue'));

    위의 예제에서 defineAsyncComponent() 함수는 비동기적으로 MyAsyncComponent.vue 파일을 로드하여 컴포넌트를 정의한다.
    이 함수의 인수로는 컴포넌트를 동적으로 로드하기 위한 함수가 전달된다.
    이 함수는 Promise를 반환하는데, 이 Promise는 동적으로 로드된 컴포넌트를 내보내는 모듈을 가리킨다.

    비동기 컴포넌트를 정의할 때 동적으로 로드된 컴포넌트를 내보내는 파일의 경로를 사용하여 컴포넌트를 정의한다.
    Vue CLI와 같은 도구를 사용하여 코드 분할을 구성하면 애플리케이션 번들이 작게 분할되어 처음에 필요한 컴포넌트만 로드된다.

    예를 들어, 다음은 defineAsyncComponent() 함수를 사용하여 비동기적으로 로드된 컴포넌트를 정의하는 예제이다.

      import { defineAsyncComponent } from 'vue';

      const AsyncComponent = defineAsyncComponent(() => import('./AsyncComponent.vue'));

      export default {
        components: {
          AsyncComponent
        },
        // 컴포넌트 내용
      }

    위의 예제에서 AsyncComponent는 비동기적으로 로드된 컴포넌트를 정의하고 있다.
    이 컴포넌트는 필요한 경우에만 로드되며, 그렇지 않으면 애플리케이션이 초기 로드 시간을 줄이는 데 도움이 된다.
    

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
