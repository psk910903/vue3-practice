<template>
	<div></div>
</template>

<script>
import { ref, watch, reactive } from 'vue';
export default {
	setup() {
		//Watch Source Type
		//watch(/* Source Type */, (newValue, oldValue) => {})
		//watch의 첫 번째 매개변수는 다양한 타입이 될 수 있다. ref, reactive, computed, getter 함수, array 타입이 될 수 있다.
		const x = ref(0);
		const y = ref(0);

		//single ref
		watch(x, newX => {
			console.log(`x is ${newX}`);
		});

		//getter
		watch(
			() => x.value + y.value,
			sum => {
				console.log(`sum of x + y is : ${sum}`);
			},
		);

		//array of mulitple sources
		watch([x, () => y.value], ([newX, newY]) => {
			console.log(`x is ${newX} and y is ${newY}`);
		});

		//다음과 같이 반응형 객체의 속성은 볼 수 없다.
		const obj = reactive({ count: 0 });
		//숫자 (number)를 전달하기 때문에 작동하지 않는다.
		watch(obj.count, newValue => {
			console.log('newValue: ', newValue);
		});
		//대신 getter를 사용
		watch(
			() => obj.count,
			newValue => {
				console.log('newValue: ', newValue);
			},
		);

		//depp option
		//반응형 객체를 직접 watch()하면 암시적으로 깊은 감시자가 생성된다.
		//즉, 속성 뿐만 아니라 모든 중첩된 속성에도 트리거 된다.
		const person = reactive({
			name: '홍길동',
			age: 30,
			hobby: '운동',
			obj: {
				count: 0,
			},
		});

		watch(person, newValue => {
			console.log('newValue: ', newValue);
		});
		//getter function으로 객체를 넘길 경우에는 객체의 값이 바뀔 경우에만 트리거 된다.
		//즉, 중첩된 속성은 트리거가되지 않는다.
		watch(
			() => person.obj,
			() => {
				//객체의 값이 바뀔 경우에만 트리거 된다.
			},
		);

		//deep 옵션을 사용하면 깊은 감시자로 강제할 수 있다.
		watch(
			() => person.obj,
			newValue => {
				console.log('newValue: ', newValue);
			},
			{ deep: true },
		);
		//deep 옵션은 큰 데이터 구조에서 사용할 때 비용이 많이 들 수 있다.
		//필요한 경우에만 사용하고 성능 영향에 주의가 필요하다.

		return {};
	},
};
</script>

<style lang="scss" scoped></style>
