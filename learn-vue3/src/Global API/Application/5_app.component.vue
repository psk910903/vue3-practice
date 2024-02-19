<template>
	<div></div>
</template>
<!--  
  
  app.component() 메서드는 Vue3에서 동적으로 컴포넌트를 등록하는 데 사용된다.
  이 메서드를 사용하면 런타임에 새로운 컴포넌트를 생성하고 등록할 수 있다.
  이것은 Vue 애플리케이션의 유연성을 높이고 코드를 동적으로 관리하는 데 도움이 된다.

  'app.component()'를 사용하여 컴포넌트를 등록하는 기본적인 구문은 다음과 같다.
  
    app.component('component-name', ComponentDefinition);

  여기서
  -'component-name'은 등록할 컴포넌트의 이름이다.
  -'ComponentDefinition'은 등록할 컴포넌트의 정의이다. 이것은 객체 또는 컴포넌트 옵션 객체일 수 있다.

  예를 들어, 다음은 'HelloWorld'라는 새로운 컴포넌트를 등록하는 예제이다.

    import { createApp } from 'vue';
    import HelloWorld from './HelloWorld.vue';

    const app = createApp();

    app.component('HelloWorld', HelloWorld);

  이렇게 하면 'HelloWorld' 컴포넌트가 'app'에 등록된다. 이제 'HelloWorl'컴포넌트를 템플릿에서 사용할 수 있다.

    <template>
      <HelloWorld />
    </template>

  'app.component()'을 사용하면 런타임에 동적으로 컴포넌트를 등록할 수 있다.
  이것은 일부 조건에 따라 다른 컴포넌트를 렌더링하거나, 사용자가 입력한 내용에 따라 컴포넌트를 동적으로 로드하고 등록하는 데 유용하다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  이름(문자열)과 컴포넌트정의 모두 전달하는 경우, 전역 컴포넌트를 등록한다. 이름만 전달되는 경우, 이미 등록된 것을 찾는다.

  타입

    interface App {
      component(name: string): Component | undefined
      component(name: string, component: Component): this
    }

  예제

    import { createApp } from 'vue'

    const app = createApp({})

    // 선택적으로 객체를 등록할 수 있음
    app.component('my-component', {
      /* ... */
    })

    // 등록된 컴포넌트 찾기
    const MyComponent = app.component('my-component')




-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
