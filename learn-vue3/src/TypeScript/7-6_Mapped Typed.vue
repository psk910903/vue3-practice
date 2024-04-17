<template>
	<div></div>
</template>
<!--  

  Mapped Types

    타입스크립트에서 매핑된 타입(Mapped Types)은 기존의 타입을 기반으로 새로운 타입을 생성할 수 있게 해주는 강력한 기능이다.
    이를 통해 특정 규칙에 따라 타입의 프로퍼티들을 변환하거나, 새로운 프로퍼티를 추가하거나, 기존 프로퍼티를 제거하는 등의 작업을 수행할 수 있다.


    매핑된 타입의 기본 구조

      매핑된 타입은 아래와 같은 형태를 가지며, KeyType을 이용하여 Type의 프로퍼티를 순회하고 변형한다.

        type MappedType<T> = {
          [P in keyof T]: Type
        };

      여기서
      - T는 타입 파라미터로, 객체의 타입을 나타낸다.
      - keyof T는 T의 모든 프로퍼티 키를 나타내는 유니온 타입니다.
      - P in keyof T는 타입 T의 각 프로퍼티 P를 순회한다.
      - Type은 각 프로퍼티 P의 결과 타입이다.


    예시: 모든 프로퍼티를 선택적으로 만들기

      type Partial<T> = {
        [P in keyof T]?: T[P];
      };

    이 매핑된 타입은 타입 T의 모든ㅍ 프로퍼티를 선택적으로 만든다. Partial은 타입스크립트에서 제공하는 유틸리티 타입 중 하나이다.


    예시: 모든 프로퍼티를 읽기 전용으로 만들기

      type Readonly<T> = {
        readonly [P in keyof T]: T[P];
      };

    이 매핑된 타입은 타입 T의 모든 프로퍼티를 읽기 전용(readonly)으로 만든다.
    이 역시 타입스크립트의 기본 유틸리티 타입이다.


    예시: 모든 프로퍼티 값을 문자열로 변환하기

      type ToStringProperties<T> = {
        [P in keyof T]: string;
      };

    이 매핑된 타입은 타입 T의 모든 프로퍼티 값을 string 타입으로 변환한다.


    예시: 특정 조건에 따라 타입 매핑하기

      조건부 타입과 함께 매핑된 타입을 사용하여 더 복잡한 변형을 수행할 수 있다.

        type ConditionalProperties<T> = {
          [P in keyof T]: T[P] extends Functions ? never : T[P]
        }

      이 타입은 T의 모든 프로퍼티를 순회하면서 프로퍼티의 값이 함수타입이면 해당 프로퍼티를 제거하고, 그렇지 않으면 그대로 유지한다.

    중복을 피하기 위해서 다른 타입을 바탕으로 새로운 타입을 생성할 수 있다.
    매핑된 타입은 이전에 선언하지 않았던 프로퍼티의 타입을 선언할 수 있는 인덱스 시그니처 문법으로 구성된다.

      type OnlyBoolsAndHorses = {
        [key: string]: boolean | Horse;
      };

      const conforms: OnlyBoolsAndHorses = {
        del: true,
        rodney: false,
      }

    매핑된 타입은 PropertyKey(keyof를 통해서 자주 생성되는)의 조합을 사용하여 키를 통해 타입을 반복적으로 생성하는 제네릭 타입이다.

      type OptionsFlags<Type> = {
        [Property in keyof Type]: boolean;
      };

    다음 예제에서, OptionsFlags는 Type 타입의 모든 프로퍼티를 가져와서 해당 값을 불린으로 변경한다.

      type FeatureFlags = {
        darkMode: () => void;
        newUserProfile: () => void;
      }

      type FeatureOptions = OptionsFlags<FeatureFlags>;

        type FeatureOptions = {
          darkMode: boolean;
          newUserProfile: boolean;
        }


    Mapplin Modifiers

      매핑중에는 추가할 수 있는 수정자로 readonly와 ?가 있다. 각각 가변성과 선택성에 영향을 미친다.
      - 또는 +를 접두사로 붙여서 이런 수정자를 추가하거나 제거할 수 있다. 접두사를 추가하지 않으면 +로 간주한다.

        // 타입의 프로퍼티에서 readonly 속성을 제거한다.
        type CreateMutable<Type> = {
          -readonly [Property in keyof Type]: Type[Property];
        };

        type LockedAccount = {
          readonly id: string;
          readonly name: string;
        }

        type UnlockedAccount = CreateMutable<LockedAccount>;

          type UnLockedAccount = {
            id: stirng;
            name: stirng;
          }


        // 타입의 프로퍼티에서 optional 속성을 제거한다.
        type Concrete<Type> = {
          [Property in keyof Type]-?: Type[Property];
        }

        type MaybeUser = {
          id: string;
          name?: string;
          age?: number;
        };

        type User = Concrete<MaybeUser>;

          type User = {
            id: string;
            name: string;
            age: number;
          }


  Key Remapping via as

    TypeScript 4.1 이상에서는 매핑된 타입에 as 절을 사용해서 매핑된 타입의 키를 다시 매핑할 수 있다.

      type MappedTypeWithNewProperties<Type> = {
        [Propertise in keyof Type as NewKeyType]: Type[Preperties]
      }

    템플릿 리터럴 타입과 같은 기능을 활용해서 이전 프로퍼티에서 새로운 프로퍼티 이름을 만들 수 있다.

      type Getters<Type> = {
        [Property in keyof Type as `get${Capitalize<string & Property>}`]: () => Type[Property]
      };

      interface Person {
        name: string;
        age: number;
        location: string;
      }

      type LazyPerson = Getters<Person>;

        type LazyPerson = {
          getName: () => string;
          getAge: () => number;
          getLocation: () => string;
        }

    조건부 타입을 통해 never를 생성해서 키를 필터링할 수 있다.

      // kind 프로퍼티를 제거한다.
      type RemoveKindField<Type> = {
        [Property in keyof Type as Exclude<Property, "kind">]: Type[Property]
      };

      interface Circle {
        kind: "circle";
        radius: number;
      }

      type KindelssCircle = RemoveKindField<Circle>;

        type KindlessCircle = {
          radius: number;
        }

    string | number | symbol 의 조합뿐만 아니라 모든 타입의 조합을 임의로 매핑할 수 있다.

      type EventConfig<Events exteds { kind: string }> = {
        [E in Events as E["kind"]]: (event: E) => void;
      }

      type SquareEvent = { kind: "square", x: number, y: number };
      type CircleEvent = { kind: "circle", radius: number };

      type Config = EventConfig<SquareEvent | CircleEvent>

        type Config = {
          square: (event: SquareEvent) => void;
          circle: (event: CircleEvent) => void;
        }


    Futher Exploration

      매핑된 타입은 타입 조각 세션의 다른 기능들과 잘 동작한다.
      예를 들어 객체의 pii 프로퍼티가 true로 설정되어 있는지에 따라 true 혹은 false를 반환하는 조건부 타입을 사용한 매핑된 타입이 있다.

        type ExtractPII<Type> = {
          [Property in keyof Type]: Type[Property] extends { pii: true } ? true : false;
        };

        type DBFields = {
          id: { format: "incrementing "};
          name: { type: string; pii: true };
        }

        type ObjectsNeedingGDPRDeletion = ExtractPII<DBFields>;

          type ObjectNeedingGDPRDeletion = {
            id: false;
            name: true;
          }

  매핑된 타입을 사용하면 기존의 타입을 유연하게 변형시켜 새로운 타입을 생성할 수 있다.
  이는 코드의 재사용성을 높이고, 복잡한 타입 관계를 간결하고 명확하게 표현할 수 있게 도와준다. 
  타입스크립트에서 제공하는 여러 기본 유틸리티 타입들(Partial, Readonly 등)도 매핑된 타입을 사용하여 구현되어 있으며, 개발자는 이를 활용하여 더 효과적인 타입 설계를 할 수 있다.
-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
