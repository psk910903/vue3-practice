<template>
	<div></div>
</template>
<!--  

  파라미터를 사용한 동적 라우트 매칭

    주어진 라우트의 패턴에 해당하는 컴포넌트를 매핑해야하는 경우가 자주 있다.
    예를 들어 모든 사용자에게 렌더링되어야 하지만, 사용자 ID가 다른 User 컴포넌트가 있을 수 있다.
    Vue Router에서 라우트에 동적 세그먼트를 사용하여 구현할 수 있다.
    이 세그먼트를 파라미터 (param)라고 한다.

      const User = {
        template: '<div>사용자</div>',
      }

      // 이 라우트들은 createRouter에 전달됨.
      const routes = [
        // 동적 세그먼트는 콜론으로 시작.
        { path: '/users/:id', component: User },
      ]

    이제 /user/mike나 /user/john과 같은 URL은 모두 동일한 라우트로 매핑된다.

    파라미터는 콜론 : 으로 표시한다. 라우트가 일치하면 파라미터 값은 모든 컴포넌트에서 this.$route.params로 노출된다.
    따라서 User 템플릿을 다음과 같이 변경하면, 현재 사용자 ID를 렌더링할 수 있다.

      const User = {
        template: '<div>사용자 {{ $route.params.id }}</div>',
      }

    동일한 라우트에 여러 파라미터가 있을 수 있으며, $route.params의 해당 필드에 매핑된다.

    예

      패턴
      1. /users/:username
      2. /users/:username/posts/:postId

      매치된 라우트
      1. /users/eduardo
      2. /users/eduardo/posts/123

      $route.params
      1. { username: 'eduardo' }
      2. { username: 'eduardo', postId: '123' }

    $route 객체는 $route.params 외에도 $route.query(URL에 쿼리가 있는 경우), $route.hash등과 같은 다른 유용한 정보도 노출한다.
    자세한 내용은 API 참조에서 확인할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  파라미터 변경에 반응하기

    파라미터가 있는 라우트를 사용하는 경우, 사용자가 /user/mike에서 /user/john 으로 이용할 때,
    동일한 컴포넌트 인스턴스가 재사용된다는 것에 주의해야 한다.
    두 라우트 모두 동일한 컴포넌트를 렌더링하므로 이전 인스턴스를 삭제한 다음 새 인스턴스를 만드는 것보다 더 효율적이다.
    그러나 이것은 컴포넌트의 수명 주기 훅이 호출되지 않음을 의미한다.

    동일한 컴포넌트에서 파라미터 변경 사항에 반응하기 위해, $route 객체의 어떠한 속성이라도 감시(watch)할 수 있다.

      const User = {
        template: '...',
        created() {
          this.$watch(
            () => this.$route.params,
            (toParams, previousParams) => {
              // 라우트 변경에 반응하기...
            }
          )
        },
      }

    또는 네비게이션 가드 beforeRouteUpdate를 사용하여 탐색을 취소할 수도 있다.

      const User = {
        template: '...',
        async beforeRouteUpdate(to, from) {
          // 라우트 변경에 반응하기...
          this.userData = await fetchUser(to.params.id)
        },
      }

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  모두 예외처리 / 404 Not found 라우트

    일반 파라미터는 / 로 구분된 일부 URL 문자만 매치한다. 모든 것과 일치시키려면 파라미터 바로 뒤에 괄호 안에 정규식을 추가하여 맞춤 파라미터를 사용할 수 있다.

      const routes = [
        // 모든 것과 매치되며, 값은 $route.params.pathMatch에 할당됨.
        { path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound },
        // /user-로 시작하는 모든 것과 일치하고, 값은 $route.params.afterUser에 할당됨.
        { path: '/user-:afterUser(.*)', component: UserGeneric },
      ]

    이 특정 시나리오에서는 괄호 사이에 커스텀 정규식 표현식을 사용하고, pathMatch 파라미터를 선택적으로 반복 가능하게 만든다.
    이를 통해 필요한 경우 path를 배열로 분할하여 라우트를 직접 탐색할 수 있다.

      this.$router.push({
      name: 'NotFound',
      // //로 시작하는 URL을 피하기 위해, 첫 번째 문자 /만 제거하고 현재 라우트를 유지
      params: { pathMatch: this.$route.path.substring(1).split('/') },
      // 존재하는 경우, 기존 쿼리 및 해시 유지
      query: this.$route.query,
      hash: this.$route.hash,
    })

    히스토리 모드를 사용하는 경우, 지침에 따라 서버를 올바르게 구성해야 한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  고급 매칭 패턴

    Vue Router는 express에서 사용하는 자체 라우트 매칭 문법에서 영감을 받았다.
    선택적 매개변수, 0개 이상/하나 이상의 요구사항, 커스텀 정규식 패턴과 같은 많은 고급 매칭 패턴을 지원한다.
    
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
