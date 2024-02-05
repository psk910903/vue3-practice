<template>
	<div></div>
</template>
<!--  

  트렌지션 그룹

    <TransitionGroup> 은 목록에서 렌더링되는 엘리먼트 또는 컴포넌트의 삽입, 제거 순서 변경을 
    애니메이션으로 만들기 위해 설계된 빌트인 컴포넌트이다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    <Transition>과의 차이점

      <TransitionGroup>은 <Transition>과 동일한 props, CSS 트랜지션 클래스 및 JavaScript 훅 리스너를 지원하지만, 다음과 같은 차이점이 있다.

      - 기본적으로 래퍼 엘리먼트를 렌더링하지 않는다. 그러나 tag prop으로 렌더링할 엘리먼트를 지정할 수 있다.

      - 트랜지션 모드는 더 이상 상호 배타적인 엘리먼트를 사용하지 않기 때문에 사용할 수 없다.

      - 내부 엘리먼트는 고유한 key 속성을 필수로 가져야 한다.

      - CSS 트랜지션 클래스는 그룹/컨테이너 자체가 아닌 목록의 개별 엘리먼트에 적용된다.

      TIP
        in-DOM 템플릿에서 사용할 때 <Trasition-group>으로 참조해야 한다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    진입/진출 트랜지션

      다음은 <TransitionGroup>을 사용하여 v-for 목록에 진입/진출 트랜지션을 적용하는 예제이다.

        <TransitionGroup name="list" tag="ul">
            <li v-for="item in items" :key="item">
              {{ item }}
            </li>
        </TransitionGroup>

        .list-enter-active,
        .list-leave-active {
          transition: all 0.5s ease;
        }
        .list-enter-from,
        .list-leave-to {
          opacity: 0;
          transform: translateX(30px);
        }

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    이동 트랜지션

      위의 대모에는 몇 가지 명백한 결함이 있다. 항목을 삽입하거나 제거할 때 주변 항목이 부드럽게 움직이지 않고 제자리에서 즉시 "점프"한다.
      몇 가지 추가 CSS 규칙을 추가하여 이 문제를 해결할 수 있다.

        .list-move, /* 움직이는 엘리먼트에 트랜지션 적용 */
        .list-enter-active,
        .list-leave-active {
          transition: all 0.5s ease;
        }

        .list-enter-from,
        .list-leave-to {
          opacity: 0;
          transform: translateX(30px);
        }

        /* 이동 애니메이션을 올바르게 계산할 수 있도록
          레이아웃 흐름에서 나머지 항목을 꺼내기. */
        .list-leave-active {
          position: absolute;
        }

      이제 훨씬 좋아보인다. 전체 목록이 섞일 때 부드럽게 애니메이션이 적용된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    시차가 있는 목록 트랜지션

      데이터 속성을 통해 JavaScript 트랜지션과 통신함으로써 목록에서 트랜지션을 시차가 있게 할 수도 있다.
      먼저 목록의 인덱스를 DOM 엘리먼트의 data- 속성으로 렌더링한다.

        <TransitionGroup
          tag="ul"
          :css="false"
          @before-enter="onBeforeEnter"
          @enter="onEnter"
          @leave="onLeave"
        >
          <li
            v-for="(item, index) in computedList"
            :key="item.msg"
            :data-index="index" //이 부분
          >
            {{ item.msg }}
          </li>
        </TransitionGroup>

      그런 다음 JavaScript 훅에서 data- 속성을 기반으로 딜레이를 사용하여 엘리먼트에 애니메이션을 적용한다.
      다음 예제는 애니메이션을 위해 GreenSock 라이브러리를 사용한다.

        function onEnter(el, done) {
          gsap.to(el, {
            opacity: 1,
            height: '1.6em',
            delay: el.dataset.index * 0.15, //이 부분
            onComplete: done
          })
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
