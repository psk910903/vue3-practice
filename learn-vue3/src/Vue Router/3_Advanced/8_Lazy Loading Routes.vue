<template>
	<div></div>
</template>
<!--  

  라우트 지연 로딩

    번들러를 사용하여 앱을 빌드할 때 JavaScript 번들이 상당히 커질 수 있으며 페이지 로드 시간에 영향을 준다.
    각 라우트의 컴포넌트를 별도의 청크로 분할하고, 해당 라우트를 방문할 때만 로드할 수 있다면 더 효율적일 것이다.

    Vue Router는 기본적으로 동적 가져오기를 지원하므로, 정적 가져오기를 동적 가져오기로 바꿀 수 있다.

      // 다음 코드를
      // import UserDetails from './views/UserDetails'
      // 아래처럼 변경
      const UserDetails = () => import('./vuews/UserDetails.vue')

      const router = createRouter({
        // ...
        routes: [
          { path: '/user/:id', component: UserDetails }
          // 또는 라우트를 정의할 때, 아래와 같이 직접 사용할 수 있음
          { path: '/user/:id', component: () => import('./vuews/UserDetails.vue') }
        ],
      })

    component (그리고 components) 옵션은 컴포넌트를 반환하는 Promise를 반환하는 함수를 허용하며, Vue Router는 처음 페이지에 진입할 때만 가져온 다음 캐시된 버전을 사용한다.
    즉, Promise를 반환하는 복잡한 함수도 사용할 수 있다.

      const UserDetails = () => 
        Promise.resolve({
          // 컴포넌트 정의
        })

    일반적으로 모든 라우트는 항상 동적 가져오기를 사용하는 것이 좋다.

      TIP
        라우트에 비동기 컴포넌트를 사용하면 안된다. 비동기 컴포넌트는 여전히 라우트 컴포넌트 내에서 사용할 수 있지만, 라우트 컴포넌트 자체는 동적 가져오기에 불과하다.

      webpack과 같은 번들러를 사용하는 경우, 자동으로 코드 분할이 된다.

      Babel을 사용하는 경우, Babel이 문법으로 제대로 파싱할 수 있또록 syntax-dynamic-import 플러그인을 추가해야 한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  동일한 청크에서 컴포넌트 그룹화

    Webpack 사용 시

      때로는 동일한 라우트 내부에 중첩된 모든 컴포넌트를 하나의 비동기 청크로 그룹화할 수 있다.
      이를 구현하려면 특수 주석 문법으로 청크 이름을 제공하여 명명된 청크를 사용해야 한다.
      (webpack > 2.4 필요):

        const UserDetails = () => import('./UserDetails.vue') // webpackChunkName: "group-user"
        const UserDashboard = () => import('./UserDashboard.vue') // webpackChunkName: "group-user"
        const UserProfileEdit = () => import('./UserProfileEdit.vue') // webpackChunkName: "group-user"

      webpack은 동일한 청크 이름을 가진 모든 비동기 모듈을 하나의 비동기 청크로 그룹화한다.

    Vite 사용시

      Vite에서는 rollupOptions 내부에서 청크를 정의할 수 있다.

        // vite.config.js
        export default defineConfig({
          build: {
            rollupOptions: {
              // https://rollupjs.org/guide/en/#outputmanualchunks
              output: {
                manualChunks: {
                  'group-user' : [
                    './scr/UserDetails',
                    './scr/UserDashboard',
                    './scr/UserProfileEdit',
                  ]
                }
              }
            }
          }
        })
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
