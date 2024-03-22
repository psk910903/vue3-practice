<template>
	<div></div>
</template>
<!--  

  동적 라우팅

    라우터에 라우트를 추가하는 것은 일반적으로 routes 옵션을 통해 이루어지지만, 애플리케이션이 이미 실행 중인 상황에서 라우트를 추가하거나 제거할 수도 있다.
		Vue CLI UI와 같은 확장 가능한 인터페이스를 가진 애플리케이션은 이를 통해 애플리케이션을 성장시킬 수 있다.

	-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

	라우트 추가하기

		동적 라우팅은 주로 router.addRoute()와 router.removeRoute() 두 개의 함수로 구현된다.
		이것들은 새 라우트만 등록하므로, 새로 추가된 라우트가 현재 위치와 일치하는 경우, 새 라우트를 표시하려면 router.push() 또는 router.replace()를 사용하여 수동으로 탐색해야 한다.

		다음은 라우터에 하나의 단일 라우트만 있다고 가정해보자.

			const router = createRouter({
				history: createWebHistory(),
				routes: [{ path: '/:articleName', component: Article }],
			})

		모든 페이지 /about, /store, /3-tricks-to-improve-your-routing-code 가 결국 Article 컴포넌트를 렌더링한다. 우리가 /about에 있고 새 라우트를 추가하는 경우:

			router.addRoute({ path: '/about', component: About })

		하지만 페이지는 여전히 Article 컴포넌트가 표시된다. 수동으로 router.replace()를 호출하여(푸쉬를 사용하면 히스토리에 기록되므로), 현재 위치를 교체 변경해야 한다.

			router.addRoute({ path: '/about', component: About })
			// this.$route 또는 useRoute()를 사용할 수도 있다.
			router.replace(router.currentRoute.value.fullPath)

		새 라우트가 표시될 때까지 기다려야 하는 경우, await router.replace() 할 수 있음을 기억하자.

	-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

	네비게이션 가드 내부에서 라우트 추가하기

		네비게이션 가드 내부에서 라우트를 추가하거나 제거하기로 결정했다면, router.replace()를 호출하지 말고 새 위치를 반환하는 리디렉션을 트리거해야 한다.

			router.beforeEach(to => {
				if(!hasNecessaryRoute(to)) {
					router.addRoute(generateRoute(to))
					// 리디렉션 트리거
					return to.fullPath
				}
			})

		위의 예는 두 가지를 가정한다.
		첫 번째로 새로 추가된 라우트 레코드는 to 라우트와 일치하지만, 실제로 접근하려는 위치가 새로 추가된 라우트 위치와 다름.
		두 번째로 hasNecessaryRoute()는 무한 리디렉션을 방지하기 위해, 새 라우트 추가 후에는 false 반환.

		탐색을 대체하는 방식으로 리디렉션 하는 것으로, 앞서 설명한 router.replace() 예제와 같은 동작이다.
		실제 시나리오에서는 네비게이션 가드 외부에서 뷰 컴포넌트가 마운트되면, 이러한 동작을 처리해야 할 수도 있다.

	-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

	라우트 제거하기

		존재하는 라우트를 제거하는 몇 가지 방법이 있다.

			1.같은 name 가지는 라우트를 추가한다. name이 중복될 경우, 기존 라우트를 제거한 후에 라우트를 추가한다.

				router.addRoute({ path: '/about', name: 'about', component: About })
				// 이전에 추가된 라우트를 제거한다.
				// 이는 그들이 동일한 이름을 가지고 있으며, 모든 라우트에서 이름을 고유하기 때문이다.
				router.addRoute({ path: '/other', name: 'about', component: Other })

			2.router.addRoute()가 반환한 콜백을 호출.

				const removeRoute = router.addRoute(routeRecord)
				removeRoute() // 라우트가 존재하는 경우 제거함.

				name이 없는 라우트일 경우에 유용하다.

			3.router.removeRoute()에 name 문자열을 인자로 전달해 라우트를 제거한다.

				router.addRouter({ path: '/about', name: 'about', component: About })
				// 라우트를 제거함.
				router.removeRoute('about')

				라우트의 name에 Symbol을 사용하면, 라우트간 name의 충돌을 피할 수 있다.

			라우트가 제거되면 모든 별칭과 자식도 함께 제거된다.

	-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

	중첩 라우트 추가하기

		중첩된 라우트를 기존 라우트에 추가하려면, 라우트의 name을 첫 번째 파라미터로 router.addRoute()에 전달할 수 있다.
		그러면 라우트가 children을 통해 추가된 것처럼 간편하게 추가된다.

			router.addRoute({ name: 'admin', path: '/admin', component: Admin })
			router.addRoute(admin, { path: '/settings', component: AdminSettings })

		이것은 다음과 동일하다.

			router.addRouter({
				name: '/admin',
				path: '/admin',
				component: Admin,
				children: [{ path: 'settings', component: AdminSettings }],
			})

	-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

	존재하는 라우트 찾기

		Vue Router는 존재하는 라우트를 찾아볼 수 있도록, 두 개의 함수를 제공한다.

			1.router.hasRoute() : 라우트 이름을 인자로 전달하여 라우트가 존재하는지 확인.
			2.router.getRoutes(): 모든 라우트 레코드를 배열로 반환.
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
