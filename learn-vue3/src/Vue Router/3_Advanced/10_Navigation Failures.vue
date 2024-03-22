<template>
	<div></div>
</template>
<!--  

  탐색 결과 기다리기

    router-link를 사용할 때, Vue Router는 router.push를 호출하여 탐색을 트리거한다. 
    대부분의 링크에서 예상되는 동작은 사용자를 새 페이지로 이동하는 것이지만,사용자가 동일한 페이지에 남아 있는 몇 가지 상황이 있다.

      1.사용자가 탐색하려는 페이지에 이미 있음.
      2.네비게이션 가드가 return false로 탐색을 중단함.
      3.이전 탐색이 완료되지 않은 상태에서 새로운 탐색 가드가 실행됨.
      4.네비게이션 가드가 새 위치를 반환하여 다른 곳으로 리디렉션(예 return '/login').
      5.네비게이션 가드가 Error를 던짐

    탐색이 끝난 후 무언가를 하고 싶다면, router.push를 호출한 후 기다릴 방법이 필요하다.
    다른 페이지로 이동할 수 있는 모바일 메뉴가 있고, 새 페이지로 이동한 후에만 메뉴를 숨기고 싶을 때, 다음과 같이 구현했다고 가정해보자.

      router.push('/my-profile')
      this.isMenuOpen = false

    하지만 탐색이 비동기식이기 때문에 메뉴는 즉시 닫힌다. router.push가 변환하는 Promise를 await 해야 한다.

      await router.push('/my-profile')
      this.isMenuOpen = false

    이제 메뉴는 탐색이 완료되면 닫히지만 탐색이 금지된 경우에도 닫힌다. 현재 있는 페이지를 실제로 변경했는지 여부를 감지할 방법이 필요하다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  탐색 실패 감지하기

    탐색이 금지되어 사용자가 같은 페이지에 머무르는 경우, router.push가 반환하는 Promise의 해결된 값은 NavigateionFailure이다.
    반면, 탐색이 금지되지 않은 경우에는 falsy 값이 반환된다.(일반적으로 undefined) 따라서 이를 통해 현재 위치에서 다른 곳으로 이동했는지 구분할 수 있다.

      const navigationResult = await router.push('/my-profile')

      if(navigationResult) {
        // 탐색이 금지된 경우
      } else {
        // 탐색이 성공한 경우(리디렉션의 경우가 포함됨.)
        this.isMenuOpen = false
      }

    NavigationFailure는 탐색이 차단된 이유와 이유를 알 수 있는 충분한 정보를 제공하는 몇 가지 추가 속성이 있는 Error 인스턴스이다.
    탐색 결과의 종류를 확인하려면 isNavigationFailure 함수를 사용해야 한다.

      import { NavigationFailureType, isNavigationFailure } from 'vue-router'

      // 가정: 저장하지 않고 문서 편집 페이지에서 나가려는 경우
      const failure = await router.push('/articles/2')

      if(isNavigationFailure(failure, NavigationFailureType.aborted)) {
        // 알림 표시 커스텀 함수
        showToast('저장하지 않은 변경사항이 있습니다. 삭제하고 나가시겠습니까?')
      }

    TIP
      두 번째 파라미터를 생략하면 isNavigationFailure(failure)의 failure가 NavigationFailure인지 여부만 확인한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  전역 탐색 실패

    router.afterEach(), 네비게이션 가드를 사용하여 전역 탐색 실패를 감지할 수 있다.

      router.afterEach((to, from, failure) => {
        if(failure) {
          sendToAnlytics(to, from, failure)
        }
      })

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  탐색 실패 구별하기

    서두에 언급했듯이 탐색이 중단되는 여러 상황이 있으며, NavigationFailure를 반환한다. 이것은 isNavigationFailure와 NavigationFailureType을 사용하여 구분할 수 있으며, 세 가지 타입이 있다.

    1.aborted: 네비게이션 가든 내부에서 false가 반환됨.
    2.cancelled: 현재 탐색이 완료되기 전에 새 탐색이 진행됨. 예를 들어 네비게이션 가드 내부에서 기다리는 동안 router.push가 호출됨.
    3.duplicated: 이미 해당 위치에 있기 때문에 탐색이 차단됨.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  탐색 실패의 속성

    모든 탐색 실패는 to와 from 속성을 노출하여, 현재 위치와 실패한 탐색의 대상 위치를 제공한다.

      // 가정: 관리자 페이지에 접근하려는 경우
      router.push('/admin').then(failure => {
        if(isNavigationFailure(failure, NavigationFailureType.aborted)) {
          failure.to.path // '/admin'
          failure.from.path // '/'
        }
      })

    to와 from은 항상 정규화된 라우트 위치이다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  리디렉션 감지

    네비게이션 가드 내부에서 새로운 위치를 반환할 때, 진행 중인 탐색을 재정의하는 새로운 탐색을 트리거한다. 
    리디렉션은 탐색을 중지하지 않으며 새로운 값을 생성한다. 따라서 리디렉션 여부는 라우트 위치에서 redirecteFrom 속성에 접근하는 방식으로 확인해야 한다.

      await router.push('/my-profile')
      if(router.currentRoute.value.redirectedFrom) {
        // redirectedFrom은 네비게이션 가드에서 to와 from처럼 라우트 위치는 나타냄.
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
