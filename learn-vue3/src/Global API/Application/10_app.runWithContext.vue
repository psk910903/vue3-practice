<template>
	<div></div>
</template>
<!--

  app.runWithContext()

    현재 앱을 인젝션(injection) 컨텍스트로 사용하여 콜백을 실행한다.

    타입

      interface App {
        runWithContext<T>(fn: () => T): T
      }

    세부사항

      콜백 함수를 기대하고 즉시 콜백을 실행한다. 콜백의 동기 호출 중에 inject() 호출은 현재 활성 컴포넌트 인스턴스가 없는 경우에도 
      현재 앱에서 제공하는 값에서 주입(provide)을 조회할 수 있다. 콜백의 반환 값도 반환된다.

    예시

      import { inject } from 'vue'

      app.provide('id', 1)

      const injected = app.runWithContext(() => {
        return inject('id')
      })

      console.log(injected) // 1
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
