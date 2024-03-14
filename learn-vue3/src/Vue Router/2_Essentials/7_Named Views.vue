<template>
	<div></div>
</template>
<!--  
  
  이름이 있는 뷰

    때로는 여러 뷰(view)를 중첩하지 않고, 동시에 표시해야 한다.(예: 사이드바와 메인 뷰가 있는 레이아웃).
    이럴 때는 이름이 있는 뷰를 사용하면 유용하다.
    "결과를 노출할 수단"(이하 아울렛)으로 하나의 뷰를 사용하는 것보다. 여러 개의 아울렛 뷰 각각에 이름을 지정해 사용하는 것이다.
    이름이 지정되지 않은 <router-view>는 기본 값으로 default라는 이름을 가진다.

      <router-view class="view left-sidebar" name="LeftSidebar"></router-view>
      <router-view class="view main-content"></router-view>
      <router-view class="view right-sidebar" name="RightSidebar"></router-view>

    뷰는 사용할 컴포넌트로 렌더링되므로, 한 라우트에 여러 뷰를 사용하는 경우에는 여러 컴포넌트가 필요하다.
    따라서 이러한 라우트에는 components 옵션을 사용해야 한다.

      const router = createRouter({
        history: createWebHasHistory(),
        routes: [
          {
            path: '/',
            components: {
              default: Home,
              // 아래 두 컴포넌트는 <router-view>의 name 속성에 매치됨.
              // LeftSidebar: LeftSidebar 문법과 같은 코드.
              LeftSidebar,
              RightSidebar,
            }
          }
        ]
      })

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
  중첩된 이름이 있는 뷰

    중첩된 뷰가 명명된 뷰를 사용하여 복잡한 레이아웃을 만들 수 있다. 이 때 중첩된 router-view에 이름도 지정해야 한다.
    유저 세팅 패널을 예로 들어보자.

      /settings/emails                                /settings/profile
      +-----------------------------------+           +------------------------------+
      | UserSettings                      |           | UserSettings                 |
      | +-----+-------------------------+ |           | +-----+--------------------+ |
      | | Nav | UserEmailsSubscriptions | |  +---- >  | | Nav | UserProfile        | |
      | |     +-------------------------+ |           | |     +--------------------+ |
      | |     |                         | |           | |     | UserProfilePreview | |
      | +-----+-------------------------+ |           | +-----+--------------------+ |
      +-----------------------------------+           +------------------------------+

      - UserSettings는 부모 뷰로 렌더링된 컴포넌트.
      - Nav는 일반 텀포넌트.
      - UserEmailsSubscriptions, UserProfile, UserProfilePreview는 UserSettings 내에 중첩된 뷰 컴포넌트.

    위 레이아웃의 <UserSettings /> 컴포넌트 템플릿은 다음과 같다.

      // UserSettings.vue
      <div>
        <h1>사용자 설정</h1>
        <NavBar />
        <router-view />
        <router-view name="subView" />
      </div>

    이제 아래와 같이 라우트를 구성하여 위 레이아웃을 구현할 수 있다.

      {
        path: '/settings',
        // 상단(이곳)에 이름이 있는 view를 가질 수도 있음.
        component: UserSettings,
        children: [{
          path: 'emails',
          component: UserEmailsSubscriptions
        },
        {
          path: 'profile',
          components :{
            default: UserProfile,
            subView: UserProfilePreview
          }
        }]
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
