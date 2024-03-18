<template>
	<div></div>
</template>
<!--  

  네비게이션 가드

    네비게이션 가드는 주로 탐색을 리디렉션하거나 취소하여, 탐색을 막는데 사용된다.
    라우트 탐색 프로세스에 가드를 연결하는 방법으로는 "전역", "라우트별", "컴포넌트 내부"가 있다.

    * 가드란
      사용자의 요청이나 엑세스를 보호하고 관리하는 기능을 의미한다.
      주로 보안 및 권한 부여와 관련된 작업을 처리한다. 가드는 요청된 경로나 페이지에 대한 엑세스를 제어하고, 사용자가 해당 경로에 접근할 수 있는지 여부를 결정한다.

      가드는 일반적으로 두 가지 목적으로 사용된다.

      1.인증(Authentication)
        사용자가 특정 페이지나 경로에 엑세스하기 전에 사용자를 인증하는 데 사용된다.
        이는 일반적응로 로그인 또는 인증 토큰을 통해 이루어진다.
        사용자가 로그인되어 있는지 확인하고, 필요한 권한이 있는지 확인하여 요청을 허용하거나 거부할 수 있다.
      2.권한 부여(Authorization)
        사용자가 특정 펭이지나 리소스에 대한 엑세스 권한을 가지고 있는지 확인하는 데 사용된다.
        이는 사용자의 역할 또는 권한에 따라 요청을 승인 도는 거부할 수 있다.
        예를 들어, 관리자 권한이 있는 사용자만이 특정 페이지에 접근할 수 있도록 할 수 있다.

      이러한 가드는 뷰 라이터에서 미들웨어로 작동하며, 라우트에 적용될 수 있다.
      가드 요청을 처리할 때 사용자나 권한을 확인하고, 요청을 처기하거나 거부할 수 있다.
      이를 통해 애플리케이션의 보안 및 권한 관리를 효과적으로 구현할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  전역: 비포 가드

    라우터 인스턴스의 beforeEach() 메서드에 콜백 함수를 전달하여, "비포(탐색 전) 가드"를 전역으로 등록할 수 있다.

      const router createRouter({ ... })

      router.beforeEach((to, from) => {
        // ...
        // 탐색을 취소하려면 명시적으로 false를 반환해야 함.
      })

    탐색이 트리거 될 때마다 등록 순서대로 "전역 비포 가드"가 호출된다. 
    가드는 비동기식으로 해결될 수 있으며, 탐색은 모든 훅이 해결되기 전까지 대기 중(pending)으로 간주된다.

    모든 가드 함수는 두 개의 인자를 받는다.

    - to: 탐색 될 라우트 위치 객체
    - from: 탐색 전 현재 라우트 위치 객체
    
    그리고 선택적으로 다음 값 중 하나를 반환할 수 있다.

    - false: 현재 탐색을 취소한다. URL이 변경된 경우, from 라우트 URL이 재설정된다.

    - 라우트 위치 정보: router.push()를 사용할 때처럼 라우트 위치(문자열 또는 객체)를 전달한다.
      현재 탐색이 중단되고, 기존 from 위치에서 새로운 탐색 동작이 생성된다.

      router.beforeEach(async (to, from) => {
        if(
          // 유저 로그인 인증여부 확인
          !isAuthenticated &&
          // ! 무한 리디렉션 방지
          to.name !== 'Login'
        ) {
          // 유저를 로그인 페이지로 리디렉션
          return { name : 'Login'}
        }
      })

    예외 상황 시 Error를 던질 수도 있다. 이 경우에도 탐색은 취소되고 router.onError() 메서드에 전달돼 모든 콜백 함수를 호출한다.

    undefined, true 또는 아무것도 반환되지 않으면, 탐색이 유효하다고 판단하고 다음 탐색 가드가 호출된다.

    위의 모든 설명은 async 함수 및 Promise에 동일한 방식으로 작동한다.

      router.beforeEach(async (to, from) => {
        // canUserAccess()는 true 또는 false 중 하나를 리턴함
        const canAccese = await canUserAccess(to)
        if (!conAccess) return '/login'
      })

    선택적 세 번째 인자 next

      Vue Router의 이전 버전에서는 세 번째 인자 next를 사용할 수도 있었는데, 이는 일반적인 실수의 원인이었으며, RFC를 이유로 제거했다.
      그러나 여전히 지원되므로 탐색 가드에서 세 번재 인자를 전달받을 수 있다. 이 경우, 트리거 되는 탐색 가드별 정확히 한 번만 next를 호출해야 한다.
      그렇지 않으면 영원히 훅이 해결되지 않거나 오류가 생긴다.
      다음은 인증되지 않은 사용자를 /login으로 리디렉션하는 나쁜 예제이다.

        // BAD
        router.beforeEach(to, from, next) => {
          if(to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
          // 사용자가 인증되지 않은 경우, next가 두 번 호출됨
          next()
        }

      올바른 예제

        // GOOD
        router.beforeEach((to, from, next) => {
          if(to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
          else next()
        })

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  전역: 리졸브(resolve)가드

    router.beforeResolve로 전역 가드를 등록할 수 있다. 이것은 모든 탐색에서 트리거되기 때문에 router.beforeEach와 유사하지만, 
    컴포넌트 내 가드 및 비동기 라우트 컴포넌트가 모드 해결된 후, 최종적으로 탐색을 진행할 것인지 결정하기 위한 목적으로 호출한다.
    다음은 사용자 정의 메타를 정의한 속성 requiresCamera가 있는 라우트에 대해, 사용자가 카메라에 접근 권한을 부여했는지 확인하는 예시이다.

      router.beforeResolve(async to => {
        if (to.meta.requiresCamera) {
          try {
            await askForCameraPermission()
          } catch (error) {
            if (error instansof NotAllowedError) {
              // ... 오류를 처리한 다음 탐색을 취소한다.
              return false
            } else {
              // 예기치 않은 오류, 탐색을 취소하고 오류를 전역 핸들러에 저장
              throw error
            }
          }
        }
      })

    유저가 페이지에 접근할 수 없는 경우, router.beforeResolve는 이것을 처리하기 위해 데이터를 가져오거나 다른 작업을 실행하기에 이상적이다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  전역: afterEach 훅

    전역으로 afterEach 훅을 등록할 수도 있지만, 가드와 달리 next 함수를 전달받지 않으며 탐색에 영향을 줄 수 없다.

      router.afterEach((to, from) => {
        sendToAnalytics(to.fullPath)
      })

    애널리틱스, 페이지 <title> 변경, 페이지 정보를 알리는 접근성 기능 및 기타 여러 작업에 유용하다.
    
    또한 탐색 실패를 세 번째 인자로 전달한다.

      router.afterEach((to, from, failure) => {
        if(!failure) sendtoAnalytics(to.fullPath)
      })

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  가드에서 전역 인젝션(injection)

    Vue3.3 부터는 네비게이션 가드 내에서 inject()를 사용할 수 있다. 이는 pinia 스토어와 같은 전역 속성을 인젝션하는 데 유용하다.
    app.provide()와 함께 제공되는 모든 항목은 router.beforeEach(), router.beforeResolve(), router.afterEach() 내에서도 엑세스할 수 있다.

      // main.js
      const app = createApp(App)
      app.provide('global', '안녕 인젝션!')

      // router.ts 또는 main.ts
      router.beforeEach((to, from) => {
        const global = inject('global') // '안녕 인젝션!'
        // pinia 스토어
        const userStore = useAuthStore()
        // ...
      })

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  라우트 별 가드

    라우트를 구성하는 객체에서 직접 beforeEnter 가드를 정의할 수 있다.

      const routes = [
        {
          path: '/user/:id',
          component: UserDetails,
          beforeEnter: (to, from) => {
            // 라우트 진입 거부
            return false
          },
        },
      ]

    라우트 별 beforeEnter는 라우트에 진입할 때만 가드를 실행하며, params, query 또는 hash가 변경될 때 트리거하지 않는다.
    (예: /user/2에서 /user/3으로 이동 또는 /users/2#info에서 /users/2#projects로 이동.)
    다른 라우트에서 탐색된 경우에만 실행한다.

    함수로 이루어진 배열을 beforeEnter에 전달할 수도 있다. 이는 다른 라우트의 가드를 재사용할 때 유용하다.

      function removeQueryParams(to) {
        if(Object.keys(to.query).length)
          return { path: to.path, query: {}, hash: to.hash }
      }

      function removeHash(to) {
        if (to.hash) return { path: to.path, query: to.query, hash: '' }
      }

      const reoutes = [
        {
          path: '/user/:id',
          component: UserDetails,
          beforeEnter: [removeQueryParams, removeHash]
        },
        {
          path: '/about',
          component: UserDetails,
          beforeEnter: [removeQueryParams],
        }
      ]

    중첩 라우트를 사용할 때, 부모와 자식 라우트 모두 beforeEnter를 사용할 수 있다.
    부모 라우트에 배치된 경우, 같은 부모를 공유하는 자식들 간에 이동할 때는 트리거되지 않는다. 예를 들어:

      const routes = [
        {
          path: '/user',
          beforeEnter() {
            // ...
          },
          children: [
            { path: 'list', component: UserList },
            { path: 'details', component: UserDetails },
          ],
        },
      ]

    위 예제는 beforeEnter는 /user/list와 /user/details 간 이동할 때 호출되지 않는다.
    왜냐하면 그들은 같은 부모를 공유하기 때문이다. 대신 details 라우트에 직접 beforeEnter 가드를 두면, 그 두 라우트 간 이동할 때 호출될 것이다.

    TIP
      라우트 메타 필드와 전역 탐색 가드를 사용하여 라우트별 가드와 유사한 동작을 달성할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  컴포넌트 내부 가드

    마지막으로 라우트를 구성하는 객체에 전달되는 "라우트 컴포넌트" 내에서 라우트 탐색 가드를 직접 정의할 수 있다.

    옵션 API 사용

      라우트 컴포넌트에 다음과 같은 가드 옵션을 추가할 수 있다.

      - beforeRouteEnter
      - beforeRouteUpdate
      - beforeRouteLeave
      
        <script>
        export default {
          beforeRouteEnter(to, from) {
            // 이 컴포넌트를 렌더링하는 라우트가 결정되기 전에 호출됨.
            // 이 가드가 호출되는 시검에 컴포넌트 인스턴스는 아직 생성되지 않았으므로,
            // this로 컴포넌트 인스턴스에 접근할 수 없음.
          },
          beforeRouteUpdate(to, from) {
            // 이 컴포넌트를 렌더링하는 라우트의 세부 정보가 변경될 때 동일한 컴포넌트가 사용되는 경우에 호출됨.
            // 예를 들어 /user/:id 파라미터가 있는 라우트가 /user/1과 /user/2 사이를 탐색할 때,
            // Userdetails 컴포넌트 인스턴스가 유지되면 이 훅이 호출됨.
            // 이 시점에서 컴포넌트 인스턴스는 마운트된 상태이므로, 훅 내부에서 this로 컴포넌트 인스턴스에 접근할 수 있음.
          },
          beforeRouteLeave(to, from) {
            // 이 컴포넌트를 렌더링한 라우트에서 떠나려고 할 때, 호출됨.
            // beforeRouteUpdate 처럼 this로 컴포넌트 인스턴스에 접근할 수 있음.
          },
        }
        </script>

      beforeRouteEnter 가드는 탐색이 결정되기 전에 호출되므로, 진입할 새로운 컴폰넌트가 아직 생성되지 않아 this에 접근할 수 없다.

      그러나 next에 콜백을 전달하여 인스턴스에 접근할 수 있다. 
      탐색이 결정되면 콜백이 호출되고, 컴포넌트 인스턴스가 콜백의 인자로 전달된다.

        beforeRouteEnter(to, from, next) {
          next(vm => {
            // vm을 통해 컴포넌트 공개 인스턴스에 접근
          })
        }

      beforeRouteEnter는 훅의 콜백 함수에 next 인자가 전달되는 유일한 가드이다.
      beforeRouteUpdate 및 beforeRouteLeave는 this가 이미 사용 가능하므로, 콜백을 전달할 필요가 없으므로 지원되지 않는다.

        beforeRouteUpdate(to, from) {
          // 컴포넌트 인스턴스에 접근하기 위해 this를 사용.
          this.name = to.params.name
        }

      리브(leave) 가드는 일반적으로 유저가 작업한 무엇인가를 저장하지 않고, 실수로 라우트에서 떠나는 것을 방지하는 데 사용된다.
      false를 반환하여 탐색을 취소할 수 있다.

        beforeRouteLeave(to, from) {
          const answer = window.confirm('정말 떠나시겠습니까? 저장되지 않은 변경 사항이 있습니다.')
          if(!answer) return false
      }

    컴포지션 API 사용
    
      Composition API를 사용하여 컴포넌트를 작성하는 경우, onBeforeRouteUpdate와 onBeforeRouteLeave를 각각 사용하여 가드를 추가할 수 있다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  전체적인 탐색 흐름

    1.탐색이 트리거됨.
    2.비활성화된 컴포넌트에서 beforeRouteLeave 가드 호출.
    3.전역 beforeEach 가드 호출.
    4.재사용된 컴포넌트에서 beforeRouteUpdate 가드 호출.
    5.라우트를 구성하면서 beforeEnter 호출.
    6.비동기 라우트 컴포넌트를 해결(resolve).
    7.활성화된 컴포넌트에서 beforeRouteEnter 호출.
    8.전역 beforeResolve 가드 호출.
    9.탐색이 승인됨.
    10.전역 afterEach 훅 호출.
    11.DOM 업데이트가 트리거됨.
    12.인스턴스화 된 인스턴스 내부 beforeRouteEnter 가드에서 전달된 next 콜백 호출.
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
