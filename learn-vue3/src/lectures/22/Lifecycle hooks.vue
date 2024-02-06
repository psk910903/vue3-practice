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
					일반적으로 이 훅은 컴포넌트의 렌더링된 DOM에 접근해야 하는 사이드 이팩트를 실행하거나, 
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
						- 관련된 모든 반응형 이팩트( setup()에서 생성된 렌더 이팩트, 계산된 속성, 감시자)가 중지됨.
					이 훅을 사용하여 타이머, DOM 이벤트 리스너 또는 서버 연결처럼 수동으로 생성된 사이드 이팩트를 정리한다.

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

			onBeforeMount()
				컴포넌트가 마운트되기 직전에 호출될 훅을 등록한다.

				타입(ts)
					function onBeforeMount(callback: () => void): void
				
				세부 사항
					이 훅은 컴포넌트의 반응형 상태 설정이 완료된 후 호출되지만, 아직도 DOM 노드가 생성되지 않은 단계이다.
					첫 DOM 렌더 이팩트를 실행하려고 한다.

				* 이 훅은 서버 사이드 렌더링 중에 호출되지 않는다.

			onBeforeUpdate()
				반응형 상태 변경으로 컴포넌트의 DOM 트리를 업데이트하기 직전에 호출될 콜백을 등록한다.

				타입(ts)
					function onBeforeUpdate(callback: () => void): void

				세부 사항
					이 훅은 Vue가 DOM을 업데이트하기 전에 DOM 상태에 접근하는 데 사용할 수 있다.
					이 훅 내부에서 컴포넌트 상태를 수정하는 것도 안전하다.

				* 이 훅은 서버 사이드 렌더링 중에 호출되지 않는다.

			onBeforeUnmount()
				컴포넌트 인스턴스가 마운트 해제되기 직전에 호출될 콜백을 등록한다.

				타입(ts)
					function onBeforeUnmount(callback: () => void): void
				
				세부 사항
					이 훅이 호출될 때, 여전히 컴포넌트 인스턴스는 완전히 동작하는 상태이다.

				* 이 훅은 서버 사이드 렌더링 중에 호출되지 않는다.

			onErrorCaptured()
				자식 컴포넌트에서 전파된 에러가 캡쳐되었을 때 호출될 콜백을 등록한다.
				
				타입(ts)
					function onErrorCaptured(callback: ErrorCapturedHook): void

					type ErrorCapturedHook = (
						err: unknown,
						instance: ComponentPublicInstance | null,
						info: string
					) => boolean | void

				세부 사항
					다음과 같은 출처의 에러를 캡처할 수 있다.
					- 컴포넌트 렌더
					- 이벤트 핸들러
					- 생명주기 훅
					- setup() 함수
					- 감시자
					- 커스텀 디렉티브 훅
					- 트랜지션 훅

				훅은 '에러', '에러를 트리거한 컴포넌트 인스턴스', '에러 소스 유형을 지정하는 정보 문자열' 세 개의 인자를 받는다.
				
				errorCaptured 훅에서 컴포넌트 상태를 수정하여 사용자에게 에러 상태를 표시할 수 있다.
				그러나 에러가 난 컴포넌트에서 에러 상태를 렌더링해서는 안된다.
				그렇지 않으면 컴포넌트가 무한 렌더링 루프에 빠진다.

				훅은 false를 반환하여 에러가 더 이상 전파되지 않도록 할 수 있다. 아래의 에러 전파 세부 사항을 참조

				에러 전파 규칙
					- 기본적으로 모든 에러는 단계적으로 전파되며, app.config.errorHandler가 정의된 경우,
						최종적으로 이곳으로 전파되므로 한 곳에서 서비스 분석 및 보고 작업을 할 수 있다.

					- 컴포넌트의 상속 체인 또는 부모 체인에 여러 개의 errorCaptured 후크가 존재하는 경우,
						모든 후크는 동일한 오류에 대해 아래에서 위로 순서대로 호출된다.
						이는 네이티브 DOM 이벤트의 버블링 메커니즘과 유사하다.

					- errorCaptured 훅 자체에서 에러가 발생하면, 이 에러와 원래 캡처된 에러가 모두 app.config.errorHandler로 전송된다.

					- errorCaptured 훅에서 false를 반환하면 더 이상 에러가 전파되지 않는다.
						이것은 본질적으로 "이 에러는 처리되었으므로 무시되어야 한다."를 의미한다.
						따라서 이후 단계적으로 전파되어야 할 errorCaptured 훅 또는 app.config.errorHandler에 이 에러로 인한 호출 동작은 없다.

			onRenderTracked() -Dev only
				컴포넌트의 렌더 이팩트에 의해 반응형 의존성이 추적되었을 때, 호출될 디버그 콜백을 등록한다.

				* 이 훅은 개발 모드 전용이며, 서버 사이드 렌더링 중에 호출되지 않는다.

					타입(ts)
						function onRenderTracked(callback: DebuggerHook): void

						type DebuggerHook = (e: DebuggerEvent) => void

						type DebuggerEvent = {
							effect: ReactiveEffect
							target: object
							type: TrackOpTypes /* 'get' | 'has' | 'iterate' */
							key: any
						}

			onRenderTriggered() -Dev only
				컴포넌트의 렌더 이팩트가 반응형 의존성에 의해 다시 실행되도록 트리거된 경우, 호출될 디버그 콜백을 등록한다.

				* 이 훅은 개발 모드 전용이며, 서버 사이드 렌더링 중에 호출되지 않는다.

				타입(ts)
					function onRenderTriggered(callback: DebuggerHook): void

					type DebuggerHook = (e: DebuggerEvent) => void

					type DebuggerEvent = {
						effect: ReactiveEffect
						target: object
						type: TriggerOpTypes /* 'set' | 'add' | 'delete' | 'clear' */
						key: any
						newValue?: any
						oldValue?: any
						oldTarget?: Map<any, any> | Set<any>
					}

			onActivated()
				<KeepAlive>로 캐시된 컴포넌트 인스턴스가 DOM 트리의 일부로 삽입된 후 호출될 콜백을 등록한다.
				
				* 이 훅은 서버 사이드 렌더링 중에 호출되지 않는다.
				
				타입(ts)
					function onActivated(callback: () => void): void

			onServerPrefetch() -SSR 전용
				컴포넌트 인스턴스가 서버에서 렌더링 되기 전에 완료(resolve)되어야 하는 비동기 함수를 등록한다.
				
				타입(ts)
					function onServerPrefetch(callback: () => Promise<any>): void
				
				세부 사항
					- 콜백이 Promise를 반환하면, 서버 렌더러는 컴포넌트를 렌더링하기 전 Promise가 해결될 때까지 기다린다.
						이 훅은 SSR(서버 사이드 렌더링)중에만 호출되므로, 서버 전용 데이터 가져오기를 실행하는 데 사용할 수 있다.

				예제
					<script setup>
					import { ref, onServerPrefetch, onMounted } from 'vue'

					const data = ref(null)

					onServerPrefetch(async () => {
						// 서버에서 미리 데이터를 가져오는 것은
						// 클라이언트에서 데이터를 요청하는 것보다 빠름.
						// 최초 데이터 요청 결과로 컴포넌트의 일부가 렌더링 됨.
						data.value = await fetchOnServer(/* ... */)
					})

					onMounted(async () => {
						if (!data.value) {
							// 마운트 시 데이터가 null일 경우,
							// 컴포넌트가 클라이언트에서 동적으로 렌더링되도록
							// 클라이언트 측에서 가져오기를 실행해야 함.
							data.value = await fetchOnClient(/* ... */)
						}
					})
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
