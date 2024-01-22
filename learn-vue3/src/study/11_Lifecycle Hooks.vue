<template>
	<div></div>
</template>
<!--  

  생명 주기 훅

    각 Vue 컴포넌트 인스턴스는 생성될 때 일련의 초기화 단계를 거친다.
    예를 들어, 데이터 감시를 설정하고, 템플릿을 컴파일하고, 인스턴스를 DOM에 마운트하고, 데이터가 변경되면 DOM을 업데이트해야 한다.
    그 과정에서 생명 주기 훅(lifecycle hooks)이라 불리는 함수도 실행하여,
    특정 단계에서 개발자가 의도하는 로직이 실행될 수 있도록 한다.

  ----------------------------------------------------------------------------------------------------------------------------

    생명 주기 훅 등록하기

      예를 들어 onMounted 훅은 컴포넌트가 초기 렌더링 및 DOM 노드 생성이 완료된 후 코드를 실행하는 데 사용할 수 있다.

        <script setup>
        import { onMounted } from 'vue'

        onMounted(() => {
          console.log(`컴포넌트가 마운트 됐습니다.`)
        })
        </script>

      인스턴스 생명 주기의 여러 단계에서 호출되는 다른 훅도 있으며, 가장 일반적으로 사용되는 것은
      onMounted, onUpdated, onUnmounted 가 있다.

      onMounted를 호출하면, Vue는 등록된 콜백 함수를 현재 활성 컴포넌트 인스턴스와 자동으로 연결한다.
      이를 위해서는 컴포넌트 설정 중에 이러한 훅은 동기적으로 등록해야 한다.
      예를 들어 다음과 같이 하면 안된다.

        setTimeout(() => {
          onMounted(() => {
            // 작동하지 않는다.
          })
        }, 100)

      이 훅이 setup() 또는 <script setup> 내에 배치된 코드 순서에 영향을 받는다는 것을 의미하는 것은 아니다.
      onMounted()는 호출 스택이 동기식이며, setup()내에서 시작되는 경우에만 외부 함수를 실행하는 방식으로 사용할 수 있다.

        import { onMounted } from 'vue'

        export default {
          setup() {
            onMounted() 훅은 setup 함수 내에서 사용하는 경우에만
            마치 외부의 함수를 호출한 것처럼 작성할 수 있다.

            이렇게 작성된 onMounted() 훅은
            컴포넌트가 마운트 된 이후 콜백 함수를 실행하지만
            this를 통해 컴포넌트 인스턴스에 접근할 수 없다.
            onMounted(function() {
              this를 통해 컴포넌트 인스턴스에 접근할 수 없다.
              console.log('onMounted가 호출 되었습니다:', this)
            })
          },

          컴포넌트가 마운트 된 후,
          옵션 API 방식의 mounted() 훅을 실행
          mounted() {
            this를 통해 컴포넌트 인스턴스에 접근할 수 있다.
            console.log('mounted()가 호출 되었습니다:', this)
          }
        }

  ----------------------------------------------------------------------------------------------------------------------------

  정리

    Vue.js에서는 라이프사이클 훅(lifecycle hook)을 통해 컴포넌트의 생명주기 동안 특정 시점에 코드를 실행할 수 있다.
    라이프사이클 훅은 컴포넌트의 생성, 갱신, 소멸과 같은 여러 단계에서 호출되며, 각 단계에서 필요한 로직을 추가할 수 있다.
    Vue.js 3에서는 Composition API가 도입되면서 라이프사이클 훅이 기존의 Oprions API보다 조금 다르게 사용된다.

    Vue 3 Composition API의 라이프사이클 훅

      1. onBeforeMout
        - 컴포넌트가 마운트 되기 전에 실행된다.
        - 주로 초기 데이터 로딩이나 외부 자원에 대한 요청을 처리할 때 사용

      2. onMounted
        - 컴포넌트가 마운트 된 직후에 실행된다.
        - DOM 조작이나 외부 라이브러리 초기화 등에 사용된다.

      3. onBeforeUpdate
        - 컴포넌트가 업데이트 되기 전에 실행된다.
        - 데이터가 변경되기 전에 추가적인 로직을 수행할 수 있다.

      4. onUpdated
        - 컴포넌트가 업데이트 된 직후 실행된다.
        - 업데이트 이후의 DOM 조작이나 애니메이션 등을 처리할 수 있다.

      5. onBeforeUnmounted
        - 컴포넌트가 소멸되기 전에 실행된다.
        - 주로 이벤트 리스너를 제거하거나 클린업 작업을 수행할 때 사용된다.

      6. onUnmounted
        - 컴포넌트가 소멸된 직후에 실행된다.
        - 자원을 해제하거나 정리 작업을 수행할 때 사용된다.


    사용예시

      import { ref, onMounted, onBeforeUnmount } from 'vue'

      export default {
        setup() {
          const data = ref(null)

          //마운트 시 데이터 로딩
          onMounted(async () => {
            data.value = await = fetchData()
          })

          //언마운트 시 정리 작업
          onBeforeUnmount(() => {
            //이벤트 리스너 제거 등의 작업 수행
          })
        }
      }

      위의 예시에서는 onMounted 를 사용하여 컴포넌트가 마운트된 후에 데이터를 로딩하고 onBeforeUnmount를 사용하여 컴포넌트가 소멸되기 전에 정리 작업을 수행하고 있다.
      이와 같이 각 라이프사이클 훅을 적절히 활용하여 컴포넌트의 생명주기 동안 필요한 로직을 추가할 수 있다.

-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
