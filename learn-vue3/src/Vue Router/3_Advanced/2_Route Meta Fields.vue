<template>
	<div></div>
</template>
<!--  

  라우트 메타 필드

    때때로, 라우트에 meta라는 속성 객체를 사용하여, 임의의 정보(예: 누가 라우트에 접근 가능한지, 트랜지션 이름 등)를 추가하면,
    라우트 위치 및 탐색 가드에서 접근할 수 있다. meta 속성은 다음과 같이 정의한다.

      const routes = [
        {
          Path: '/posts',
          component: PostLayout,
          children: [
            {
              path: 'new',
              component: PostNew,
              // 유저 인증 필수
              meta: { requiresAuth: true },
            },
            {
              path: ':id',
              component: PostsDetail,
              // 유저 인증 없어도 됨
              meta: { requiresAuth: false },
            },
          ],
        },
      ]

    meta 필드에는 어떻게 접근할까?

    먼저 routes의 각 라우트 객체를 라우트 레코드(route record)라고 하며, 중첩될 수 있다. 
    따라서 매칭된 라우트는 둘 이상의 라우트 레코드와 매칭될 수 있다.

    예를 들어 위처럼 구성된 라우트에서 /posts/new는 부모 라우트 레코드(path: '/posts')와 자식 라우트 레코드(path: 'new') 모두와 일치한다.

    모든 라우트에 의해 일치된 라우트 기록은 route 객체(그리고 네비게이션 가드의 라우트 객체)에 route.matched 배열로 노출된다. 해당 배열을 순회하여 모든 meta 필드를 확인할 수 있지만,
    Vue Router는 부모에서 자식으로 모든 meta 필드의 비재귀적 병합인 route.meta를 제공한다.
    이는 단순히 다음과 같이 작성할 수 있음을 의미한다.

      router.beforeEach((to, from) => {
        // to.matched.some(record => record.meata.requiresAuth)와 깉이
        // 모든 라우트 레코드를 확인하는 대신, to.meta.requiresAuth 처럼 바로 접근 가능함.
        if (to.meta.requiresAuth && !auth.isLoggedIn()) {
          // 이 라우트는 인증이 필수이므로, 로그인 여부를 확인하고,
          // 만약 인증되지 않았다면 로그인 페이지로 리디렉션
          return {
            path: '/login',
            // 나중에 다시 올 수 있도록, 방문한 위치를 저장
            query: { redirect: to.fullPath }
          }
        }
      })

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  TypeScript

    vue-router 에서 RouteMeta 인터페이스를 활장하여 메트 필드를 입력할 수 있다.

      // router.ts와 같은 .ts 파일에 직접 추가할 수 있다.
      // .d.ts 파일에 추가할 수도 있다.
      // 프로젝트의 tsconfig.json 파일에 포함되어 있는지 확인해야 한다.
      import 'vue-router'

      // 모듈로 처리되도록 하려면 export 문을 하나 이상 추가해야 한다.
      export {}

      declare module 'vue-router' {
        interface RouteMeta {
          // 선택적
          isAdmin?: boolean
          // 모든 라우트에서 선언해야 함
          requiresAuth: boolean
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
