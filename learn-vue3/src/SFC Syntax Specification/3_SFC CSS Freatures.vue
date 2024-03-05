<template>
	<div></div>
</template>
<!--  

  SFC CSS 기능

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  범위가 지정된 CSS

    <style> 태그에 scoped 속성이 있으면, 해당 CSS는 현재 컴포넌트의 엘리먼트에만 적용된다.
    이것은 Shadow DOM에서 발견되는 스타일 캡슐화와 유사하다.
    몇 가지 주의 사항이 있지만, 풀리필이 필요하지 않다. PostCSS를 사용하여 다음을 변환함으로써 달성된다.

      <style scoped>
      .example {
        color: red;
      }
      </style>

      <template>
        <div class="example">안녕!</div>
      </template>

    다음으로

      <style>
      .example[data-v-f3f3eg9] {
        color: red;
      }
      </style>

      <template>
        <div class="example" data-v-f3f3eg9>안녕!</div>
      </template>

    자식 컴포넌트 루트 엘리먼트

      scoped를 사용하면 부모 컴포넌트의 스타일이 자식 컴포넌트로 누출되지 않는다. 그러나 자식 컴포넌트의 루트 노드는 부모의 범위가 지정된 CSS 모두의 영향을 받는다.
      이것은 부모가 레이아웃 목적으로 자식 루트 엘리먼트의 스타일을 지정할 수 있도록 의도적으로 설계된 것이다.

    깊은 셀렉터

      scoped 스타일의 셀렉터를 "깊게"(즉, 자식 컴포넌트에 영향을 미치게 하려면) :deep() 의사 클래스를 사용할 수 있다.

        <style scoped>
        .a :deep(.b) {
          /* ... */
        }
        </style>

      위의 내용은 다음과 같이 컴파일된다.

        .a[data-v-f3f3eg9] .b {
          /* ... */
        }

      TIP
        v-html로 만든 DOM 컨텐츠는 범위가 지정된 스타일의 영향을 받지 않지만, 깊은 셀렉터를 사용하여 스타일을 지정할 수 있다.

    슬롯형 셀렉터

      기본적으로 범위가 지정된 스타일은 <slot/>에 의해 렌더링된 컨텐츠에 영향을 미치지 않는다.
      스타일을 전달하는 부모 컴포넌트가 소유한 것으로 간주되기 때문이다. 슬롯 컨텐츠를 명시적으로 대상으로 지정하려면, :slotted 의사 클래스를 사용해야 한다.

        <style scoped>
        :slotted(div) {
          color: red;
        }
        </style>

    전역 셀렉터

      하나의 규칙만 전역적으로 적용하려면, 다른 <style>을 만드는 대신 :global 의사 클래스를 사용할 수 있다.(아래 참고)

        <style scoped>
        :global(.red) {
          color: red;
        }
        </style>

    로컬 및 전역 스타일 혼합

      동일한 컴포넌트에 범위가 지정된 스타일과 범위가 지정되지 않은 스타일을 모두 포함할 수도 있다.

        <style>
        /* 전역 스타일 */
        </style>

        <style scoped>
        /* 로컬 스타일 */
        </style>

    범위가 지정된 스타일 팁

      - 범위가 지정된 스타일은 클래스의 필요성을 제거하지 않는다.
        브라우저가 다양한 CSS 셀렉터를 렌더링하는 방식 때문에, p { color: red } 처럼 범위를 지정할 때(즉, 속성 셀렉터와 결합될 때) 속도가 몇 배 느려진다.
        .example { color: ref }와 같이 클래스나 ID를 사용하면, 성능 저하를 거의 제거할 수 있다.

      - 재귀적 컴포넌트의 자손 셀렉터에 주의해야 한다.
        셀렉터가 .a .b인 CSS 규칙의 경우, .a와 일치하는 엘리먼트가 재귀적인 자식 컴포넌트를 포함한다면, 해당 자식 컴포넌트의 모든 .b는 규칙과 일치하게 된다.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  CSS 모듈

    <style module> 태그는 CSS 모듈로 컴파일되고, 결과적으로 CSS 클래스를 $style 키(key) 내부에 객체로 컴포넌트에 노출한다.

      <template>
        <p :class="$style.red">이것은 빨간색이어야 합니다.</p>
      </template>

      <style module>
      .red {
        color: red;
      }
      </style>

    결과적인 클래스는 충돌을 피하기 위해 해시되어, CSS 범위를 현재 컴포넌트로만 지정하는 것과 동일한 효과를 얻는다.

    전역 예외, 구성 등의 자세한 사항은 CSS 모듈 스팩을 참고.

    커스텀 이름 삽입

      module 속성에 값을 지정하여, 주입된 클래스 객체의 속성 키를 커스텀할 수 있다.

        <template>
          <p :class="classes.red">red</p>
        </template>

        <style module="classes">
        .red {
          color: red;
        }
        </style>

    컴포지션 API와 함께 사용

      주입된 클래스는 useCssModule API를 통해 setup() 및 <script setup>에서 접근할 수 있다.
      커스텀 주입 이름이 있는 <style module> 블록의 경우 useCssModule은 일치하는 module 속성 값을 첫 번째 인자로 받는다.

        import { useCssModule } from 'vue'

        // setup() 내부에서...
        // 기본값은, <style module>의 클래스 반환
        useCssModule()

        // 이름을 지정한 경우, <style module="classes">의 클래스 반환
        useCssModule('classes')

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  CSS에서 v-bind()

    SFC <style> 태그는 v-bind CSS 함수를 사용하여, CSS 값을 동적 컴포넌트 상태에 연결하는 것을 지원한다.

      <template>
        <div class="text">안녕!</div>
      </template>

      <script>
      export default {
        data() {
          return {
            color: 'red'
          }
        }
      }
      </script>

      <style>
      .text {
        color: v-bind(color);
      }
      </style>

    <script setup> 에서도 문법은 적용하며, JavaScript 표현식을 지원한다.(따옴표로 묶어야 함)

      <script setup>
      const theme = {
        color: 'red'
      }
      </script>

      <template>
        <p>안녕!</p>
      </template>

      <style scoped>
      p {
        color: v-bind('theme.color');
      }
      </style>

    실제 값은 해시된 CSS 커스텀 속성으로 컴파일되므로, CSS는 여전히 정적이다. 커스텀 속성은 컴포넌트의 루트 엘리먼트에 인라인 스타일로 적용되고,
    소스 값이 변경되면(소스가 반응형일 경우) 반응적으로 업데이트된다.

    

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
