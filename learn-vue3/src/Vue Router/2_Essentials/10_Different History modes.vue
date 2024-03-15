<template>
	<div></div>
</template>
<!--  

  다양한 히스토리 모드

    라우터 인스턴스를 생성할 때 history 옵션을 사용하여, 다양한 히스토리 모드 중에서 하나를 적용할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  해시 모드

    해시 모드는 createWebHashHistory()를 사용하여 적용한다.

      import { createRouter, createWebHashHistory } from 'vue-router'

      const router = createRouter({
        history: createWebHashHistroy(),
        routes: [
          //...
        ],
      })

    내부적으로 전달되는 실제 URL 앞에 해시 문자(#)를 사용한다. 
    URL의 이 섹션은 서버로 전송되지 않으므로 서버의 특별한 조치가 필요하지 않다.
    그러나 그것은 SEO에 나쁜 영향을 미치므로, 이것이 문제가 된다면 HTML5 모드를 사용해야 한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  HTML5 모드

    HTML5 모드는 createWebHistory()를 사용하여 적용하며, 권장하는 모드이다.

      import { createRouter, createWebHistory } from 'vue-router'

      const router = createRouter({
        history: createWebHistory(),
        routes: [
          //...
        ],
      })

    createWebHistory()를 사용할 때 URL은 https://example.com/user/id와 같이 "정상"으로 보인다.

    하지만 여기에 문제가 있다. 우리 앱은 적절한 서버 구성이 없는 클라이언트 사이드 SPA이므로,
    사용자가 브라우저에서 바로 https://example.com/user/id에 접근하면 404 오류가 발생한다.

    문제를 해결하려면 서버에 간단한 포괄 대체 라우트를 추가하기만 하면 된다. 
    URL 접근이 배포된 자산과 일치하지 않으면, 앱의 index.html 페이지를 제공해야 한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  메모리 모드

    메모리 모드는 브라우저 환경을 고려하지 않는다. 따라서 URL과 상호작용하지 않으며,
    자동으로 네비게이션 초기화를 트리거하지 않는다. 이것은 Node.js환경의 SSR에 완벽하다.
    createMemoryHistory()를 사용하여 적용하며, app.use(router)를 호출한 후 반드시 네비게이션 초기화를 해야 한다.

      import { createRouter, createMemoryHistory } from 'vue-router'

      const router = createRouter({
        history: createMemoryHistory(),
        routes: [
          //...
        ]
      })

    브라우저에서 이 모드를 사용할 수 있지만 히스토리가 없으므로, 권장되지 않으니 유의해야 한다.
    다시말해 "뒤로" 또는 "앞으로" 이동할 수 없다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  경고

    Vue Router를 사용하여 만든 클라이언트 사이드 SPA는 모든 라우트에 index.html을 제공하므로,
    서버는 더 이상 찾을 수 없는 페이지에 대해서 404오류를 반환하지 않는다.
    이 문제를 해결하려면, 정의되지 않은 모든 라우트를 포괄할 수 있는 "포괄 라우트"를 구현하여, 404 페이지를 표시해야 한다.

      const router = createRouter({
        history: createWebHistory(),
        routes: [{ path: '/pathMatch(/*)', component: NotFoundComponent }],
      })

    또는 Node.js 서버를 사용하는 경우, 서버 측 라우터를 사용하여 수신되는 URL을 매칭하고, 매칭되는 라우트가 없으면 404로 응답하는 폴백을 구현할 수 있다.
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
