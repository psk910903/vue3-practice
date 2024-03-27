<template>
	<div></div>
</template>
<!--  

	Actions

		액션은 컴포넌트의 메서드와 동일하다. 이들은 defineStore()에서 actions 속성으로 정의할 수 있으며, 처리해야 할 작업의 로직을 정의하는 데 완벽하다.

			export const useCounterStore = defineStore('counter', {
				state: () => ({
					count: 0,
				}),
				actions: {
					// this에 의존하므로, 화살표 함수를 사용할 수 없음.
					increment() {
						this.count++
					},
					randomizeCounter() {
						this.count = Math.round(100 * Math.random())
					},
				},
			})

		게터와 마찬가지로 액션은 전체 입력(및 자동완성)지원과 함께 this를 통해 전체 스토어 인스턴스에 접근할 수 있다.
		게터와 달리 actions는 비동기식일 수 있으며, 액션 내에서 API 호출이나 다른 액션을 await할 수 있다.
		여기에 Mande를 사용한 예제가 있다. Promise를 얻기 위해 어떤 라이브러리를 사용하는지는 중요하지 않다.
		네이티브 fetch 함수(브라우저만 해당)를 사용할 수도 있다.

			import { mande } from 'mande'

			const api = mande('/api/users')

			export const useUsers = defineStore('users', {
				state: () => ({
					userData: null,
					// ...
				}),

				actions: {
					async registerUser(login, passward) {
						try {
							this.userData = await api.post({ login, passward })
							showTooltip('다시 오신 것을 환영합니다, ${this.userData.name}!')
						} catch (error) {
							showTooltip(error)
							// 폼(form) 컴포넌트가 오류를 표시하도록 함.
							return error
						}
					},
				},
			})

		또한 원하는 인자를 자유롭게 설정하고, 무엇이든 반환할 수 있다. 액션을 호출하면 모든 것이 자동으로 추론된다.

		액션은 일반 함수 및 메서드처럼 호출된다.

			<script setup>
			const store = useCounterStore()
			// 스토어의 액션을 메서드처럼 호출
			store.randomizeCounter()
			</script>

			<template>
				// 템플릿에서
				<button @click="store.randomizeCounter()">버튼</button>
			</template>

	-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

	다른 스토어 액션에 접근

		액션 내부에서 직접 다른 스토어를 사용할 수 있다.

			import { useAuthStore } from './auth-store'

			export const useSettingStore = defineStore('setting', {
				state: () => ({
					preferences: null,
					// ...
				}),
				actions: {
					async fetchUserPreferences() {
						const auth = useAuthStore()
						if (auth.isAuthenticated) {
							this.preferences = await fetchPreferences()
						} else {
							throw new Error('인증이 필요합니다.')
						}
					},
				},
			})

	-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

	옵션 API에서 사용

		다음 예제는 아래와 같은 저장소가 생성되었다고 가정한다.

			// 예제 파일 경로:
			// ./src/stores/counter.js

			import { defineStore } from 'pinia'

			export const useCounterStore = defineStore('counter', {
				state: () => ({
					count: 0
				}),
				actions: {
					increment() {
						this.count++
					}
				}
			})

		setup 에서

			컴포지션 API가 모든 사람을 위한 것은 아니지만 setup() 훅을 사용하면 옵션 API에서 피니아를 더 쉽게 사용할 수 있다.
			추가 맵 헬퍼 함수가 필요하지 않다.

			<script>
			import { useCounterStore } from '../stores/counter'

			export default defineComponent({
				setup() {
					const counterStore = useCounterStore()

					return { counterStore }
				},
				methods: {
					incrementAndPrint() {
						this.counterStore.increment()
						console.log('숫자 세기:', this.counterStore.count)
					},
				},
			})
			</script>

		setup() 없이

			컴포지션 API를 전혀 사용하지 않으려면, mapActions() 헬퍼를 사용하여 컴포넌트의 메서드에 액션 속성을 매핑할 수 있다.

				import { mapActions } from 'pinia'
				import { useCounterStore } from '../stores/counter'

				export default {
					methods: {
						// 컴포넌트 내부에서 this.increment()로 접근할 수 있게 함.
						// store.incretment() 처럼 호출하는 것과 동일.
						...mapActions(useCounterStore, ['increment']),
						// 위와 같지만 this.myOwnName()으로 등록.
						...mapActions(useCounterStore, { myOwnName: 'increment '}),
					},
				}

	-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

	액션 구독하기

		store.$onAction()에 콜백을 전달해 액션과 그 결과를 감시할 수 있으며, 액션보다 먼저 실행된다.
		after는 프라미스(Promise)를 처리하고, 액션이 해결(resolve)된 후, 함수를 실행할 수 있도록 한다.
		비슷한 방식으로 onError를 사용하면, 작업이 실패(throw)되거나 거부(reject)되는 경우, 함수를 실행할 수 있다.
		이는 Vue 문서에서 언급하는 팁과 유사하게 런타임에 오류를 추적하는 데 유용하다.

		다음은 액션을 실행하기 전과 해결/거부 이후를 콘솔에 기록하는 예제이다.

			const unsubscribe = someStore.$onAction(
				({
					name, // 액션의 이름.
					store, // 스토어 인스턴스, someStore와 같음.
					args, // 액션으로 전달된 인자로 이루어진 배열.
					after, // 액션에서 반환 또는 해결 이후의 훅.
					onError, // 액션에서 실패 또는 거부될 경우의 훅.
				}) => {
					// 이 특정 액션 호출에 대한 공유 변수.
					// 역자설명: 이 액션의 훅에서 참조하게 되는 클로저 변수 개념.
					const startTime = Date.now()

					// 이곳은 store의 액션이 실행되기 전에 트리거됨.
					console.log(`"${name}"가 [${args.join(', ')}] 인자를 전달받아 시작됩니다.`)

					// 액션이 성공하고 완전히 실행된 후에 트리거됨.
					// 프라미스 반환을 대기
					after((result) => {
						console.log(`"${name}"rk ${Date.now() - startTime}ms 후 종료되었습니다. \n결과: ${result}.`)
					})

					// 액션이 실패하거나 프라미스가 거부되면 트리거 됨.
					onError((error) => {
						console.warn(`"${name}가 ${Date.now() - startTime}ms 후 실패했습니다.\n애러: ${error}.")
					})
				}
			)

			// 리스너를 수동으로 제거.
			unsubscribe()

		기본적으로 액션 구독은 컴포넌트에 추가된(스토어가 컴포넌트의 setup() 내부에 있는) 경우에 바인딩된다.
		따라서 컴포넌트가 마운트 해제되면 자동으로 제거된다. 컴포넌트가 마운트 해제된 후에도 이를 유지하려면, 두 번째 인수로 현재 컴포넌트에서 액션 구독을 분리하는 true를 전달한다.

			<script setup>
			const someStore = useSomeStore()

			// 이 구독은 컴포넌트가 마운트 해제된 후에도 유지됨.
			someStore.$onAction(callback, true)
			</script>

		
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
