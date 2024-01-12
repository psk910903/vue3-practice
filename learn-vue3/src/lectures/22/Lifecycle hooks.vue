<template>
	<div></div>
	<!--  
		생명주기 훅
			각 Vue 컴포넌트 인스턴스는 생성될 때 일련의 초기화 단계를 거친다.
			예를 들어, 데이터 감시를 설정하고, 템플릿을 컴파일하고, 
			인스턴스를 DOM에 마운트하고, 데이터가 변경되면 DOM을 업데이트해야 한다.
			그 과정에서 생명 주기 훅(lifecycle hooke)이라 불리는 함수도 실행하여,
			특정 단계에서 개발자가 의도하는 로직이 실행될 수 있도록 해야한다.
---------------------------------------------------------------------------------------------------------------------------
		생명주기 훅 등록하기
			예를 들어, onMounted 훅은 컴포넌트가 초기 렌더링 및 DOM 노드 생성이 완료된 후 코드를 실행하는 데 사용할 수 있다.
				<script setup>
				import { onMounted } from 'vue'

				onMounted(() => {
					console.log(`컴포넌트가 마운트 됐습니다.`)
				})
				</script>
			인스턴스 생명 주기의 여러 단계에서 호출되는 다른 훅도 있으며,
			가장 일반적으로 사용되는 onMounted, onUpdated, onUnmounted가 있다.

			onMounted를 호출하면, Vue는 등록된 콜백 함수를 현재 활성 컴포넌트 인스턴스와 자동으로 연결한다.
			이를 위해서는 컴포넌트 설정 중에 이러한 훅은 동기적으로 등록해야 한다.
			예를 들어 다음과 같이 하면 안된다.
				setTimeout(() => {
					onMounted(() => {
						// 작동하지 않습니다.
					})
				}, 100)
			이 훅이 setup() 또는 <script setup> 내에 배치된 코드 순서에 영향을 받는다는 것을 의미하는 것은 아니다.
			onMounted() 는 호출 스택이 동기식이며, setup()내에서 시작되는 경우에만 외부 함수를 실행하는 방식으로 사용할 수 있다.

			import { onMounted } from 'vue'
			export default {
				setup() {
					// onMounted() 훅은 setup 함수 내에서 사용하는 경우에만
					// 마치 외부의 함수를 호출한 것처럼 작성할 수 있습니다.
					//
					// 이렇게 작성된 onMounted() 훅은
					// 컴포넌트가 마운트 된 이후 콜백 함수를 실행하지만
					// `this`를 통해 컴포넌트 인스턴스에 접근할 수 없습니다.
					onMounted(function() {
						// `this`를 통해 컴포넌트 인스턴스에 접근할 수 없습니다.
						console.log('onMounted가 호출 되었습니다:', this)
					})
				},

				// 컴포넌트가 마운트 된 후,
				// 옵션 API 방식의 mounted() 훅을 실행.
				mounted() {
					// `this`를 통해 컴포넌트 인스턴스에 접근할 수 있습니다.
					console.log('mounted()가 호출 되었습니다:', this)
				}
			}
---------------------------------------------------------------------------------------------------------------------------
		컴포지션 API 생명주기 훅
			아래 나열된 모든 API는 컴포넌트 setup() 단계에서 동기적으로 호출되어야 한다.
		
			onMounted()
				컴포넌트가 마운트된 후 호출될 콜백을 등록한다.

				타입(ts)
					function onMounted(callback: () => void): void

				세부사항
					컴포넌트가 마운트 되었다고 간주하는 조건은 다음과 같다.
						- 동기식 자식 컴포넌트가 모두 마운트됨(<suspense> 트리 내부의 비동기 컴포넌트 또는 컴포넌트는 포함하지 않음)
						- 자체 DOM 트리가 생성되어 상위 컨테이너에 삽입됨. 앱의 루트 컨테이너가 Document 내에 있는 경우에만 컴포넌트의 DOM 트리가 문서 내에 있음을 보장함
					일반적으로 이 훅은 컴포넌트의 렌더링된 DOM에 접근해야 하는 사이드 이펙트를 실행하거나, 
					서버에서 렌더링 된 앱에서 DOM과 관련 코드를 클라이언트에서만 실행하도록 제한하는데 사용한다.

				* 이 훅은 서버 사이드 렌더링 중에 호출되지 않는다.

				예제
					템플릿 ref를 통해 엘리먼트에 접근
						<script setup>
						import { ref, onMounted } from 'vue'

						const el = ref()

						onMounted(() => {
							el.value // <div>
						})
						</script>

						<template>
							<div ref="el"></div>
						</template>

			onUnmounted()
				컴포넌트가 마운트 해제된 후 호출될 콜백을 등록한다.

				타입(ts)
					function onUnmounted(callback: () => void): void

				세부 사항
					컴포넌트가 마운트 해제되었다고 간주하는 조건은 다음과 같다.
						- 모든 자식 컴포넌트가 마운트 해제됨.
						- 관련된 모든 반응형 이펙트( setup()에서 생성된 렌더 이펙트, 계산된 속성, 감시자)가 중지됨.
					이 훅을 사용하여 타이머, DOM 이벤트 리스너 또는 서버 연결처럼 수동으로 생성된 사이드 이펙트를 정리한다.

					* 이 훅은 서버 사이드 렌더링 중에 호출되지 않는다.

					예제
						<script setup>
						import { onMounted, onUnmounted } from 'vue'

						let intervalId
						onMounted(() => {
							intervalId = setInterval(() => {
								// ...
							})
						})

						onUnmounted(() => clearInterval(intervalId))
						</script>
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
