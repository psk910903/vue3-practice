<template>
	<div></div>
</template>
<!--  

  라우트 컴포넌트에 props 전달하기

    컴포넌트에 $route를 사용하면 특정 URL에서만 사용할 수 있으므로, 라우트와 긴밀하게 결합되고 컴포넌트의 유연성이 제한된다.
    이것이 반드시 나쁜 것은 아니지만, props 옵션으로 이러한 작용을 분리할 수 있다.

      const User = {
        template: '<div>사용자: {{ $route.params.id }}</div>'
      }
      const routes = [{ path: '/user/:id', component: User }]

    이렇게 바꿀 수 있다.

      const User = {
        //라우트의 파라미터와 동일한 이름으로 prop을 추가해야 함.
        props: ['id'],
        template: '<div>사용자: {{ id }}</div>'
      }
      const routes = [{ path: '/user/:id', component: User, props: ture }]

    이렇게 하면 어디에서나 컴포넌트를 더 쉽게 재사용하고 테스트할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  불리언 모드

    라우트의 props가 ture로 설정되면, route.params가 컴포넌트의 props로 설정된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  이름이 있는 뷰

    이름이 있는 뷰가 있는 라우트의 경우, 이름이 있는 뷰 각각에 props 옵션을 정의해야 한다.

      const routes = [
        {
          path: '/user/:id',
          components: { default: User, sidebar: Sidebar },
          props: { default: true, sidebar: false }
        }
      ]

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  객체 모드

    props가 객체인 경우, 이는 그대로 컴포넌트의 props로 설정된다. Props가 정적일 때 유용하다.

      const routes = [
        {
          path: '/promotion/from-newsletter',
          components: Promotion,
          props: { newsletterPopup: false }
        }
      ]

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  함수 모드

    Props를 반환하는 함수를 만들 수 있다. 
    이를 통해 파라미터를 다른 유형으로 캐스팅하고, 정적 값을 라우트 기반의 값과 결합하는 등의 작업을 수행할 수 있다.

      const routes = [
        {
          path: '/search',
          component: SearchUser,
          props: route => ({ query: route.query.q })
        }
      ]

    /search?q=vue는 SearchUser 컴포넌트의 props로 { query: 'vue' }를 전달한다.

    라우트가 변경될 때만 props를 정의하는 함수가 평가되므로, 상태가 변경될 때도 반응하는 props를 전달하려면, 함수 내부의 상태를 사용하는 대신,
    래퍼 컴포넌트를 사용해 계산된 속성 { computed }를 props로 전달해야 한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  RouterView에서 직접 

    또한 <RouterView> 슬롯을 통해 어떤 props도 전달할 수 있다.

      <RouterView v-slot="{ Component }">
        <component
          :is="Component"
          view-prop="value"
        />
      </RouterView>

    경고
      이 경우, 모든 Vuew 컴포넌트가 view-prop을 받게 된다. 이는 보통 좋은 아이디어가 아니다.
      왜냐하면 모든 view 컴포넌트가 view-prop prop을 선언했다는 것을 의미하기 때문이다.
      그렇지 않을 수도 있다. 가능하다면 위의 옵션 중 하나를 사용해야 한다.

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
