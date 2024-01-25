<template>
	<div></div>
</template>
<!--  

	커스텀 티렉티브

	----------------------------------------------------------------------------------------------------------------------------

		소개

			코어에 포함된 기본 디렉티브 세트(예: v-model 또는 v-show)외에도 Vue를 사용하면 커스텀 디렉티브를 정의할 수 있다.

			우리는 Vue에서 컴포넌트 기초와 컴포저블이라는 두 가지 형태의 코드 재사용을 도입했다.
			컴포넌트는 주요 빌딩 블럭(building-block)이고, 컴포저블은 상태 저장 로직을 재사용하는데 대중점을 관리한다.
			반면에 커스텀 디렉티브는 주로 일반 엘리먼트에 대한 저수준(low-level) DOM접근과 관련된 로직을 재사용하기 위한 것이다.

			커스템 디렉티브는 컴포넌트의 생명주기 혹은 포함을 객체처럼 정의된다.
			훅은 디렉티브가 바인딩된 엘리먼트를 수신한다. 다음은 엘리먼트가 Vue에 의해 DOM에 삽입될 때 <input>에 포거스 되는 커스텀 디렉티브 구현의 예제이다.

				<script setup>
				// 템플릿에서 v-focus로 활성화 가능
				const vFocus = {
					mounted: (el) => el.focus()
				}
				</script>

				<template>
					<input v-focus />
				</template>

			페이지의 다른 곳을 클릭하지 않았다고 가정하면, 위의 인풋은 자동으로 포커스 되어야 한다.
			이 디렉티브는 페이지 로드 시 뿐만 아니라, Vue에서 동적으로 엘리먼트를 삽입할 때도 작동하기 때문에 autofocus 속성보다 더 유용하다.

			<script setup>에서 v 접두사로 시작하는 모든 camelCase 변수를 커스텀 디렉티브로 사용할 수 있다.
			위의 예에서 vFocus는 템플릿에서 v-focus로 사용할 수 있다.

			<script setup>을 사용하지 않는 경우, directives 옵션을 사용하여 커스텀 디렉티브를 등록할 수 있다.

				export default {
					setup() {
						/*...*/
					},
					directives: {
						// 템플릿에서 v-focus로 활성화 가능
						focus: {
							/* ... */
						}
					}
				}

			앱 수준에서 커스텀 디렉티브를 전역적으로 등록하는 것도 일반적이다.

				const app = createApp({})

				// 모든 컴포넌트에서 v-focus를 사용할 수 있도록 한다.
				app.directive('focus', {
					/* ... */
				})

			TIP
				커스텀 디렉티브는 원하는 기능을 직접 DOM 조작을 통해서만 달성할 수 있는 경우에만 사용해야 한다.
				v-bind와 같은 내장 디렉티브를 사용하여 선언적 템플릿을 사용하는 것이 더 효율적이고 서버 렌더링에 친숙하기 때문이다.

	----------------------------------------------------------------------------------------------------------------------------

		디렉티브 훅

			디렉티브를 정의하는 객체는 다음과 같은 여러 훅 기능을 제공할 수 있다.(모두 선택 사항)

				const myDirective = {
					//바인딩된 엘리먼트의 속성 또는 이벤트 리스너가 적용되기 전에 호출된다.
					created(el, binding, vnode, prevVnode){
						//인자에 대한 자세한 내용은 아래를 참고.
					},

					//엘리먼트가 DOM에 삽입되기 직전에 호출된다.
					beforeMount(el, binding, vnode, prevVnode) {},

					//바인딩된 엘리먼트의 부모 컴포넌트 및 모든 자식 컴포넌트의 mounted 이후에 호출된다.
					mounted(el, binding, vnode, prevVnode), {},

					//부모 컴포넌트의 updated 전에 호출된다.
					beforeUpdate(el, binding, vnode, prevVnode) {},

					//바인딩된 엘리먼트의 부모 컴포넌트 및 모든 자식 컴포넌트의 updated 이후에 호출된다.
					updated(el, binding, vnode, prevVnode) {},

					//부모 컴포넌트의 beforeUnmount 이후에 호출된다.
					beforeUnmount(el, binding, vnode, prevVnode) {},

					//부모 컴포넌트의 unmounted 전에 호출된다.
					unmounted(el, binding, vnode, prevVnode) {}
				}

			훅 인자

				디렉티브 훅에는 다음 인자가 전달된다.

					- el: 디렉티브가 바인딩된 엘리먼트이다. DOM을 직접 조작하는 데 사용할 수 있다.

					- binding: 다음 속성을 포함하는 객체이다.

						- value:디렉티브에 전달된 값이다. 예를 들어 v-my-directive="1 + 1"에서 value는 2이다.

						- oldValue: 이것은 beforeUpdate 및 updated 에서만 사용할 수 있다. 값이 변경되었는지 여부에 관계없이 사용 가능하다.

						- arg: 디렉티브에 전달된 인자(있는 경우). 예를 들어 v-my-directive:foo 에서 인자는 "foo" 이다.

						- modifiers: 수식어가 있는 경우 수식어를 포함하는 객체이다. 예를 들어 v-my-directive.foo.bar 에서 수식어 객체는 { foo: true, bar: true } 이다.

						- instance: 디렉티브가 사용되는 컴포넌트의 인스턴스이다.

						- dir: 디렉티브를 정의하는 객체

					- vnode 바인딩된 엘리먼트를 나타내는 기본 VNode.

					- prevNode: 이전 렌더링에서 바인딩된 엘리먼트를 나타내는 VNode이다. beforeUpdate 및 update 훅에서만 사용할 수 있다.

				다음과 같은 디렉티브를 사용한다고 가정한 예제를 살펴보자

					<div v-example:foo.bar="baz">

				binding 인자는 다음과 같은 형태의 객체이다.

					{
						arg: 'foo',
						modifiers: { bar: true },
						value: /* baz의 값 */,
						oldValue: /* 업데이트 전 baz의 값 */,
					}

				내장 디렉티브와 유사하게 커스텀 디렉트브 인자는 동적일 수 있다. 예를 들어

					<div v-example:[arg]="value"></div>

				여기서 디렉티브 인자는 컴포넌트 상태의 arg 속성을 기반으로 반응형으로 업데이트된다.

				참고
					el을 제외하고 이러한 인자들은 읽기 전용으로 처리하고
					절대 수정해서는 안된다.
					훅 간에 정보를 공유해야 하는 경우 엘리먼트의 dataset을 통해 공유하는 것이 좋다.

	----------------------------------------------------------------------------------------------------------------------------

		간단하게 함수로 사용하기

			커스템 디렉티브가 mounted 및 updated에 대해 동일한 동작을 갖는 것이 일반적이며, 다른 훅은 필요하지 않다.
			이러한 경우 디렉티브를 객체가 아닌 함수로 정의할 수 있다.

					<div v-color="color"></div>

					app.directive('color', (el, binding) => {
						// 이 함수가 호출되는 시점은 mounted와 updated 이다.
						el.style.color = binding.value
					})

	----------------------------------------------------------------------------------------------------------------------------

		객체를 값으로 전달하기

			디렉티브에 여러 값이 필요한 경우, JavaScript 객체 리터럴을 전달할 수도 있다.
			디렉티브는 모든 유효한 JavaScript 표현식을 사용할 수 있다.

				<div v-demo="{ color: 'white', text:'안녕' }"></div>

				app.directive('demo', (el, binding) => {
					console.log(binding.value.color) // => "white"
					console.log(binding.value.text) // => "안녕!"
				})

	----------------------------------------------------------------------------------------------------------------------------

		컴포넌트에서 사용

			컴포넌트에서 사용될 때 커스텀 디렉티브는 폴스루 속성과 유사하게 항상 컴포넌트의 루트 노드에 적용된다.

				//MyComponent 템플릿

				<div> //여기에 v-node 디렉티브가 적용한다.
					<span>컴포넌트 건텐츠..</span>
				</div>

			컴포넌트에는 잠재적으로 둘 이상의 루트 노드가 있을 수 있다. 다중 루트 컴포넌트에 적용하려고 하면 디렉티브가 무시되고 에러가 발생한다.
			속성과 달리 디렉티브는 v-bind="$attrs"를 사용하여 다른 엘리먼트에 전달할 수 없다. 일반적으로 컴포넌트에 커스템 디렉티브를 사용하는 것은 권장히지 않는다.

			
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
