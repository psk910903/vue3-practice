<template>
	<div></div>
</template>
<!--  

  트렌지션

    라우트 컴포넌트에 트렌지션을 사용하고 탐색에 애니메이션을 적용하려면, <RouterView> 슬롯을 사용해야 한다.

      <router-view v-slot="{ Component }">
        <transition name="fade">
          <component :is="Component" />
        </transition>
      </router-view>

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  라우트 별 트렌지션

    위 예제는 모든 라우트에 동일한 트렌지션을 적용한다. 각 라우트의 컴포넌트가 서로 다른 트렌지션을 갖도록 하려면,
    메타 필드와 <transition>의 name을 동적으로 조합해 사용하면 된다.

      const routes = [
        {
          path: '/custom-transition',
          component: PanelLeft,
          meta: { transition: 'silde-left' },
        },
        {
          path: '/other-transition',
          component: PanelRight,
          meta: { transition: 'silde-right' },
        },
      ]

      <router-view v-slot="{ Component, route }">
        // 커스텀 트랜지션 사용하거나 fade로 대체
        <transition :name="route.meta.transition || 'fade'">
          <component :is="Component" />
        </transition>
      </router-view>

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  라우트 기반 동적 트렌지션

    대상 라우트와 현재 라우트 간의 관계를 기반으로, 사용할 트렌지션을 동적으로 설정할 수도 있다. 이전 예제 코드와 매우 유사하다.

      // 동적인 트랜지션의 name 사용
      <router-view v-slot="{ Component, route }">
        <transition :name="route.meta.transition">
          <component :is="Component" />
        </transition>
      </router-view>

    라우트의 깊이에 따라 meta 필드에 정보를 동적으로 추가하기 위해, 탐색 후 훅을 추가할 수 있다.

      route.afterEach((to, from) => {
        const toDepth = to.path.split('/').length
        const fromDepth = from.path.split('/').length
        to.meta.transition = toDepth < fromDepth ? 'slide-rigth' : 'slide-left'
      })

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  재사용된 뷰에서 강제로 트랜지션

    Vue는 동일한 컴포넌트를 자동으로 재사용하여 트랜지션이 작동하지 않을 수 있다.
    다행히도 key 속성을 추가해 강제로 트랜지션을 트리거 하는 것이 가능하다. 이렇게 하면 라우트를 유지하면서 파라미터 변경에 의한 트랜지션을 구현할 수 있다.

      <route-view v-slot="{ Component, route }">
        <transition name="fade">
          <component :is="Component" :key="route.path" />
        </transition>
      </route-view>

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
