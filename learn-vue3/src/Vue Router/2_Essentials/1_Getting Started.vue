<template>
	<div></div>
</template>
<!--  

  시작하기

    Vue + Vue Router를 사용해 싱글 페이지 앱을 만드는 것은 매우 쉽다.
    Vue.js를 사용했다면, 이미 컴포넌트로 앱을 구성했을 것이다. 
    여기에 Vue Router를 사용할 경우, 라우트에 컴포넌트를 매핑하고 Vue Router에게 렌더할 라우트를 알려주기만 하면 된다.
    다음은 기본 예제이다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  HTML

    <script src="https://unpkg.com/vue@3"></script>
    <script src="https://unpkg.com/vue-router@4"></script>

    <div id="app">
      <h1>멋진 앱!</h1>
      <p>
        // 탐색을 위해 router-link 컴포넌트를 사용.
        // to 라는 prop으로 링크를 지정
        // <router-link> 태그는 올바른 href 속성을 가진 <a> 태그로 렌더링 될 것
          <router-link to="/">Home</router-link>
          <router-link to="/about">About</router-link>
        </p>
        // 라우터 출력
        // 현재 라우트에 매핑된 컴포넌트가 렌더링됨.
        <router-view></router-view>
      </div>

    router-link

      a 태그 대신 커스텀 컴포넌트인 router-link를 사용하여 링크를 생성해야 한다. 이를 통해 Vue Router는 페이지를 다시 로드하지 않고도 URL 변경, 생성 및 인코딩을 처리할 수 있다.
      나중에 이러한 기능을 활용하는 방법을 살펴보자.

    router-view

      router-view는 URL에 해당하는 컴포넌트를 표시한다. 어디에나 배치 가능하여 레이아웃에 맞게 조정할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    JavaScript

      import { createRouter, createWebHashHistory } from 'vue-router'

      // 1. 라우트에 사용할 컴포넌트를 정의
      // import를 사용해 파일을 가져올 수 있음
      const Home = { template: '<div>Home</div>' }
      const About = { template: '<div>About</div>' }

      // 2. 라우트를 정의하고, 각 라우트를 컴포넌트와 매핑
      // 중첩 라우트에 대해서는 나중에 설명함.
      const routes = [
        { path: '/', component: Home },
        { path: '/about', component: About },
      ]

      // 3. routes를 옵션으로 전달해 라우터 인스턴스를 생성.
      // 여기에 추가 옵션을 전달할 수 있지만, 지금은 간단하게 나타냄.
      const router = createRouter({
        // 4. 사용할 히스토리 모드 정의
        // 여기서는 간단하게 해시 모드를 사용
        history: createWebHashHistory(),
        routes, // `routes: routes`와 같음
      })

      // 5. 루트 인스턴스를 생성하고 마운트
      const app = Vue.createApp({})

      // 앱이 라우터를 인식하도록,
      // 라우터 인스턴스를 use()로 등록해야 함.
      app.use(router)

      app.mount('#app')
      // 앱 시작

    app.use(router) 를 호출하여 초기 탐색을 트리거하고, this.$router 내부의 모든 컴포넌트에서 현재 this.$router에 대한 접근 권한을 부여한다.

      // Home.vue
      export default {
        computed: {
          username() {
            // params가 무엇인지는 나중에 설명
            return this.$route.params.username
          },
        },
        methods: {
          goToDashboard() {
            if (isAuthenticated) {
              this.$router.push('/dashboard')
            } else {
              this.$router.push('/login')
            }
          },
        },
      }

    setup 함수 내부에서 라우터 또는 라우트에 접근하려면, useRouter 또는 useRoute 함수를 호출해야 한다.
    컴포지션 API에서 이에 대해 자세히 알아볼 수 있다.

    문서 전체에서 우리는 종종 router 인스턴스를 사용한다. this.$router는 createRouter를 통해 생성된 router 인스턴스와 동일하다.
    this.$router를 사용하는 이유는 라우팅을 조작해야 하는 모든 컴포넌트에서 라우터를 import 하고 싶지 않기 때문이다.

    


-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
