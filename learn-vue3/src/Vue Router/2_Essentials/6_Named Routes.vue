<template>
	<div></div>
</template>
<!--  

  이름이 있는 라우트

    모든 라우트에는 path와 더불어 name도 제공할 수 있으며, 다음과 같은 장점이 있다.

    - 하드코딩된 URL 없음
    - params을 자동 인코딩/디코딩.
    - URL에 오타 발생 방지.
    - 라우트의 우선순위를 우회.

      const routes = [
        {
          path: '/user/:username',
          name: 'user',
          component: User
        }
      ]

    <router-link> 컴포넌트의 to에 객체를 전달해 이름이 있는 라우트에 연결할 수 있다.

      <router-link :to="{ name: 'user', params: { username: 'erina' }}">
        유저 링크!
      </router-link>

    프로그래밍 방식의 router.push()에도 같은 방식의 객체를 사용한다.

      router.push({ name: 'user', params: { username: 'erina' } })

    위의 두 경우 모두 /user/erina 라우트로 이동한다.

    참고: 예제
      각 이름은 모든 라우트에서 고유해야 한다. 하나의 이름을 여러 라우트에 추가하면 라우트는 마지막에 추가된 것만 유지한다.

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
