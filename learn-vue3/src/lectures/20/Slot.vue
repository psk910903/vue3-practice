<template>
	<div></div>
	<!--  
    Solt이란?
      부모 컴포넌트에서 제공하는 콘텐츠를 나타내는 슬롯 콘텐츠이다.
      텍스트뿐만 아니라 HTML요소, 컴포넌트 등 다양한 모든 콘텐츠가 될 수 있다.

    Fallback Content
      상위 컴포넌트에서 슬롯 콘텐츠가 제공되지 않을 때 슬롯에 대한 폴백(기본 콘텐츠)을 지정할 수 있다.
        <template>
          <button class="btn">
            <slot>Default Click!!</slot>
          </button>
        </template>
    
    Named Solts
      <slot>요소에 이름을 부여하여 여러개의 <solt>을 정의할 수 있다.
        BaseCard.vue
          <template>
            <article>
              <div>
                <slot name="header"></slot>
              </div>
              <div>
                <slot></slot>
              </div>
              <div">
                <slot name="footer"></slot>
              </div>
            </article>
          </template>
      - <solt>에 name속성을 부여하여 특정 슬롯 콘텐츠가 렌더링 되어야 할 위치를 설정할 수 있다.
      - name이 없는 <solt>의 이름은 암시적으로 default이다.
        
      부모 컴포넌트 사용 예시
        <template>
          <BaseCard>
            <template v-slot:header>제목</template>
            <template v-slot:default>안녕하세요</template>
            <template v-slot:footer>푸터</template>
          </BaseCard>
        </template>
      위 예시처럼 name이 부여된 <slot>에 콘텐츠를 전달하려면 v-solt 디렉티브를 사용하여 전달할 수 있다.
      v-solt:전달인자 를 사용하여 지정한 슬롯 콘텐츠에 전달할 수 있다.

      v-solt은 #으로 단축 표현할 수 있다.
        부모 컴포넌트 사용 예시
          <template>
            <BaseCard>
              <template #header>제목</template>
              <template #default>안녕하세요</template>
              <template #footer>푸터</template>
            </BaseCard>
          </template>
          
      default 슬롯은 암시적으로 처리할 수 있다.
        부모 컴포넌트 사용 예시
        <template>
          <BaseCard>
            <template #header>제목</template>
              안녕하세요 - 암시적으로 default slot
            <template #footer>푸터</template>
          </BaseCard>
        </template>

    Dynamic Slot Named
      v-slot 디렉티브 전달인자에 데이터를 바인딩하여 동적으로 변경할 수 있다.
        <BaseCard>
          <template v-slot:[dynamicSlotName]>
            ...
          </template>

          with shorthand
          <template #[dynamicSlotName]>
            ...
          </template>
        </BaseCard>
      
    Render Scope
      슬롯 콘텐츠는 상위 컴포넌트에 정의되어 있으므로 상위 컴포넌트의 데이터 영역에 접근은 가능하지만
      하위 컴포넌트의 영역에는 접근할 수 없다.

    Scoped Slots
      Render Scope에서 언급했던 것처럼 슬롯 콘텐츠는 자식 컴포넌트의 데이터에 접근할 수 없다.
      하지만 슬롯 콘텐츠에서 상위 컴포넌트와 하위 컴포넌트 데이터를 모두 사용할 수 있다면 우리는 개발할 때 매우 유용할 것이다.
      이러한 방법으로 우리는 자식 컴포넌트에서 <slot> 요소를 사용할 때 props를 전달하는 것처럼 속성을 슬록 콘텐츠에 전달할 수 있다.
        <template>
          <div>
            <slot :text="greetingMessage" :count="count"></slot>
          </div>
        </template>
        <script>
        import { ref } from 'vue';

        export default {
          setup() {
            const greetingMessage = ref('Hello World!');
            const count = ref(1);
            return {
              greetingMessage,
              count,
            };
          },
        };
        </script>

      default <slot>이 하나 밖에 없는 경우에는 v-slot 디렉티브를 사용하여 props를 전달 받을 수 있다.
        <MyComponent v-slot="slotProps">
          {{ slotProps.text }} {{ slotProps.count }}
        </MyComponent>
      
      구조분해할당 문법으로 더 사용하기 편리하게 받을 수 있다.
        <MyComponent v-slot="{ text, count }">
          {{ text }} {{ count }}
        </MyComponent>

    Named Scoped Slots
      이름이 부여된 슬롯도 유사하게 동작한다. v-slot:name="slotProps"
        <MyComponent>
          <template #header="headerProps">
            {{ headerProps }}
          </template>

          <template #default="defaultProps">
            {{ defaultProps }}
          </template>

          <template #footer="footerProps">
            {{ footerProps }}
          </template>
        </MyComponent>

  -->
</template>

<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
