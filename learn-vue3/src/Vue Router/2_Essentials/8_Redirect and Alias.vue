<template>
	<div></div>
</template>
<!--  

  리디렉션과 별칭

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  리디렉션

    리디렉션은 routes 설정 시 정의할 수도 있다. /home에서 /로 리디렉션하면:

      const routes = [{ paht: '/home', redirect: '/' }]

    리디렉션은 이름이 있는 라우트로 설정할 수도 있다.

      const routes = [{ path: 'home', redirect: { name: 'homepage' } }]

    또는 함수를 사용하여 동적 리디렉션을 구현할 수도 있다.

      const routes = [
        {
          // /search/screens -> /search?q=screens
          path: '/search/:searchText',
          redirect: to => {
            // 함수는 이동하려 했던 라우트(/search/:searchText) 객체를 인자로 받음.
            // 반환 값: 리디렉션 될 라우트 문자열 또는 위치 정보 객체.
            return { path: '/search', query: { q: to.params.searchText } }
          },
        },
        {
          path: '/search',
          // ...
        }
      ]

    네비게이션 가드는 리디렉션이 정의된 라우트에는 적용되지 않는다.
    예를 들어 위의 예에서 /home 라우트에 beforeEnter 가드를 추가해도 아무런 효과가 없다.

    redirect를 정의한 경우, component 옵션을 생략할 수 있다. 이는 해당 라우트에는 도달할 수 없으므로 렌더링할 컴포넌트가 없기 때문이다.
    단, 중첩된 라우트를 사용하는 경우에는 예외이다. 라우트에 children과 redirect 속성이 있으면 component 속성도 있어야 한다.

  상대적인 리디렉션

    상대적인 위치로 리디렉션할 수도 있다.

      const routes = [
        {
          // 항상 /users/123/posts는 /users/:id/prfile로 리디렉트 됨.
          path: '/users/:id/posts',
          redirect: to => {
            // 이 함수는 인자로 이동하려 했던 라우트를 수신하며,
            // 아래 반환 문자열은 상대 위치가 '/' 또는 
            // { path: 'profile' } 로 시작되지 않음.
            return 'profile'
          }
        }
      ]

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  별칭

    리디렉션은 사용자가 /home을 방문할 때 URL이 /keys로 대체되어 /keys로 매칭됨을 의미한다. 하지만 별칭이란 무엇일까?

    /의 별칭이 /home일 경우, /home을 방문할 때 URL이 /home으로 유지되지만, /를 방문한 것과 동일하다는 의미이다.

    이 라우트는 다음과 같이 구성할 수 있다.

      const routes = [{ paht: '/', component: Homepage, alias: '/home' }]

    별칭을 사용하면 중첩 구조 구성에 제약을 받는 대신, UI 구조를 임의의 URL에 자유롭게 매핑할 수 있다.
    중첩 라우트에 절대라우트를 추가하려면, /로 시작하는 별칭을 정의한다.
    여러개의 별칭을 배열로 정의할 수도 있다.

      const routes = [
        {
          path: '/users/:id',
          component: UsersLayout,
          children: [
            // 아래 3개의 URL에 해당하면 UserList가 렌더링 됨.
            // - /users/24
            // - /users/24/profile
            // - /24
            { path: 'profile', component: UserList, alias: ['/:id', ''] }
          ],
        },
      ]

    라우트에 파라미터가 있는 경우, 절대 라우트 별칭에 파라미터를 포함해야 한다.

      const routes = [
        {
          path: '/users/:id',
          component: UsersByIdLayout,
          children: [
            // 아래 3개의 URL에 해당하면 UserDetails가 렌더링 됨.
            // - /users/24
            // - /users/24/profile
            // - /24
            { path: 'profile', component: UserDetails, alias: ['/:id', ''] }
          ],
        },
      ]
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
