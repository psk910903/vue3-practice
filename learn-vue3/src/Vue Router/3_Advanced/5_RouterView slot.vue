<template>
	<div></div>
</template>
<!--  

  RouterView 슬롯

    RouterView 컴포넌트는 라우트 컴포넌트를 렌더링하는 데 사용할 수 있는 슬롯을 노출한다.

      <router-view v-slot="{ Component }">
        <component :is="Component" />
      </router-view>

    위의 코드는 슬롯 없이 <router-view />를 사용하는 것과 동일하지만, 슬롯은 다른 기능과 함께 작업할 때 추가적인 유연성을 제공한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  KeepAlive 및 Transition

    KeepAlive 컴포넌트와 작업할 때 일반적으로 라우트 컴포넌트를 유지하려고 할 것이지만
    RouterView 자체를 유지하려고 하지는 않을 것이다. 이를 위해 KeppAlive를 슬롯 내에 넣으면 이를 달성할 수 있다.

      <router-view v-slot="{ Component }">
        <keep-alive>
          <component :is="Component" />
        </keep-alive>
      </router-view>

    마찬가지로 슬롯을 사용하면 라우트 컴포넌트 간에 전환을 위해 Transition 컴포넌트를 사용할 수 있다.

      <router-view v-slot="{ Component }">
        <transition>
          <component :is="Component" />
        </transition>
      </router-view>

    또한 Transition 내에서 KeepAlive를 사용할 수 있다.

      <router-view v-slot="{ Component }">
        <transition>
          <keep-alive>
            <component :is="Component" />
          </keep-alive>
        </transition>
      </router-view>

    Transition 컴포넌트와 함께 RouterView를 사용하는 자세한 정보는 Transitions 가이드를 참조

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  Props 및 슬롯 전달

    슬롯을 사용하여 라우트 컴포넌트에 props 또는 슬롯을 전달할 수 있다.

      <router-view v-slot="{ Component }">
        <component :is="Component" some-prop="a value">
          <p>Some slotted content</p>
        </component>
      </router-view>

    실제로 이것은 모든 라우트 컴포넌트가 동일한 props와 슬롯을 사용해야 하는 경우가 아니라면 사용하고 싶지 않은 것이 대부분이다.
    다른 방법으로 props를 전달하는 방법은 라우트 컴포넌트에 Props 전달을 참조

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  템플릿 참조

    슬롯을 사용하면 템플릿 참조를 라우트 컴포넌트에 직접 넣을 수 있다.

      <router-view v-slot="{ Component }">
        <component :is="Component" ref="mainContent" />
      </router-view>

    만약 ref를 <router-view>에 넣는다면 ref는 라우트 컴포넌트 대신 RouterView 인스턴스로 채워질 것이다.

  
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
