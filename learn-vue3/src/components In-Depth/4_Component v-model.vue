<template>
	<div></div>
</template>
<!--  

  Component v-model

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    기본 사용법

      v-model을 컴포넌트에서 사용하여 양방향 바인딩을 구현할 수 있다.

      Vue3.4 부터는 defineModel() 매크로를 사용하는 것이 권장되는 접근 방식이다.

        //Child.vue
        <script setup>
        const model = defineModel()

        function update() {
          model.value++
        }
        </script>

        <template>
          <div>부모 바인딩 v-model은: {{ model }}</div>
        </template>

      부모는 v-model을 사용하여 값을 바인딩할 수 있다.

        //Parent.vue
        <Child v-model="count" />

      defineModel()에 의해 반환되는 값은 ref이다. 
      다른 ref처럼 접근하고 변경할 수 있지만, 부모 값과 로컬 값 사이의 양방향 바인딩으로 작동한다.

        - .value는 부모 v-model에 의해 바인딩된 값고 동기화된다.
        - 자식에 의해 변경되면 부모 바인딩 값도 업데이트된다.

      따라서 이 ref를 네이티브 입력 엘리먼트의 v-model에 바인딩할 수도 있으며,
      네이티브 입력 엘리먼트를 래핑하면서 동일한 v-model 사용을 제공하는 것이 간단해진다.

        <script setup>
        const model = defineModel()
        </script>

        <template>
          <input v-model="model" />
        </template>


      내부 구조

        defineModel 은 편의성을 위한 매크로이다. 컴파일러는 다음과 같이 확장한다.

        - 로컬 ref의 값과 동기화되는 modelValue 라는 이름의 prop;
        - 로컬 ref의 값이 변경될 때 발생하는 update:modelValue 라는 이벤트.

      3.4 이전에 위와 같은 자식 컴포넌트를 구현하는 방법은 다음과 같았다.

        <script setup>
        const props = defineProps(['modelValue'])
        const emit = defineEmits(['update:modelValue'])
        </script>

        <template>
          <input
            :value="props.modelValue"
            @input="emit('update:modelValue', $event.target.value)"
          />
        </template>

      보시다시피, 이것은 훨씬 더 장황하다. 하지만 내부에서 무슨 일이 일어나는지 이해하는 것이 도움이 된다.

      defineModel은 prop을 선언하므로, defineModel에 전달함으로써 기본 prop의 옵션을 선언할 수 있다.

        //v-model을 필수로 만들기
        const model = definModel({ required: true })

        //기본값 제공
        const model = defineModel({ default: 0 })

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    v-model 인수

      v-model은 컴포넌트에서 인수를 받을 수도 있다.

        <MyComponent v-model:title="bookTitle" />

      자식 컴포넌트에서는 defineModel()의 첫 번째 인수로 문자열을 전달하여 해당 인수를 지원할 수 있다.

        //MyComponent.vue
        <script setup>
        const title = defineModel('title')
        </script>

        <template>
          <input type="text" v-model="title" />
        </template>

      prop 옵션이 필요한 경우, 모델 이름 뒤에 전달해야 한다.

        const title = defineModel('title', { required: true })

      Pre 3.4 Usage

        //MyComponent.vue
        <script setup>
        defineProps(['title'])
        defineEmits(['update:title'])
        </script>

        <template>
          <input
            type="text"
            :value="title"
            @input="$emit('update:title', $event.target.value)"
          />
        </template>

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    멀티플 v-model 바인딩

      앞서 배운 것처럼 특정 prop과 이벤트를 타깃팅하는 기능을 v-model 인자로 활용하면 
      이제 단일 컴포넌트 인스턴스에 여러 개의 v-model 바인딩을 생성할 수 있다.

      각 v-model은 컴포넌트에 추가 옵션 없이도 다른 prop에 동기화된다.

        <UserName
          v-model:first-name="first"
          v-model:last-name="last"
        >

        <script setup>
        const firstName = defineModel('firstName')
        const lastName = defineModel('lastName')
        </script>

        <template>
          <input type="text" v-model="firstName" />
          <input type="text" v-model="lastName" />
        </template>

      Pre 3.4 Usage

        <script setup>
        defineProps({
          firstName: String,
          lastName: String
        })

        defineEmits(['update:firstName', 'update:lastName'])
        </script>

        <template>
          <input
            type="text"
            :value="firstName"
            @input="$emit('update:firstName', $event.target.value)"
          />
          <input
            type="text"
            :value="lastName"
            @input="$emit('update:lastName', $event.target.value)"
          />
        </template>

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    v-model 수정자 처리하기

      Form 양식 입력 바인딩에 대해 배울 때 v-model에 .trim, .number 및 .lazy와 같은 내장 수정자가 있따는 것을 알았다.
      경우에 따라 사용자 정의 입력 컴포넌트에서 v-model이 사용자 정의 수정자를 지원하도록 할 수 있다.

      v-model 바인딩에서 제공하는 문자열의 첫 글자를 대문자로 표시하는 사용자 지정 수정자 예제인 capitalize를 만들어 보겠다.

        <MyComponent v-model.capitalize="myText" />

      컴포넌트 v-model에 추가된 수정자(modifiers)는 자식 컴포넌틍서 defineModel() 반환값을 구조 분해하여 다음과 같이 접근할 수 있다.

        <script setup>
        const [model, modifiers] = defineModel()

        console.log(modifiers) // { capitalize: true }
        </script>

        <template>
          <input type="text" v-model="model" />
        </template>

      수정자에 기반하여 값을 어떻게 읽거나 쓸지 조건적으로 조정하려면,
      defineModel()에 get과 set 옵션을 전달할 수 있다.
      이 두 옵션은 모델 ref와 get/set에서 값을 받아 변환된 값을 반환해야 한다.
      다음은 set 옵션을 사용하여 capitalize 수정자를 구현하는 방법이다.

        <script setup>
        const [model, modifiers] = defineModel({
          set(value) {
            if (modifiers.capitalize) {
              return value.charAt(0).toUpperCase() + value.slice(1)
            }
            return value
          }
        })
        </script>

        <template>
          <input type="text" v-model="model" />
        </template>

      Pre 3.4 Usage

        <script setup>
        const props = defineProps({
          modelValue: String,
          modelModifiers: { default: () => ({}) }
        })

        const emit = defineEmits(['update:modelValue'])

        function emitValue(e) {
          let value = e.target.value
          if (props.modelModifiers.capitalize) {
            value = value.charAt(0).toUpperCase() + value.slice(1)
          }
          emit('update:modelValue', value)
        }
        </script>

        <template>
          <input type="text" :value="modelValue" @input="emitValue" />
        </template>

      
      인자와 수정자가 있는 v-model에 대한 수정자들

        다음은 다른 인자를 가진 여러 v-model에 수정자를 사용한 예시이다.

          <UserName
            v-model:first-name.capitalize="first"
            v-model:last-name.uppercase="last"
          />

          <script setup>
          const [firstName, firstNameModifiers] = defineModel('firstName')
          const [lastName, lastNameModifiers] = defineModel('lastName')

          console.log(firstNameModifiers) // { capitalize: true }
          console.log(lastNameModifiers) // { uppercase: true}
          </script>

        Pre 3.4 Usage

          <script setup>
          const props = defineProps({
            firstName: String,
            lastName: String,
            firstNameModifiers: { default: () => ({}) },
            lastNameModifiers: { default: () => ({}) }
          })
          defineEmits(['update:firstName', 'update:lastName'])

          console.log(props.firstNameModifiers) // { capitalize: true }
          console.log(props.lastNameModifiers) // { uppercase: true}
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
