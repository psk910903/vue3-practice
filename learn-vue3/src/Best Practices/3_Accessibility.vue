<template>
	<div></div>
</template>
<!--  

    접근성

    웹 접근성(a11y라고도 함)은 장애가 있는 사람, 네트워크 속도가 느린 사람, 오래되거나 손상된 하드웨어 또는 단순히 낙후된 환경에 있는 사람 등
    누구나 사용할 수 있는 웹사이트를 만드는 것이다. 예를 들어, 비디오에 자막을 추가하면 청각 장애인, 난청 및 시끄러운 환경에서 소리를 들을 수 없는 사용자 모두에게 도움이 된다.
    마찬가지로 텍스트의 대비가 너무 낮지 않은지 확인하면, 시력이 약하거나 밝은 햇빛 아래에서 휴대전화를 사용하는 사용자 모두에게 도움이 된다.

    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

        건너뛰기 링크

            사용자가 여러 웹 페이지에서 반복되는 컨텐츠를 건너뛸 수 있도록 각 페이지 상단에 기본 컨텐츠 역역으로 직접 연결되는 링크를 추가해야 한다.

            일반적으로 이것은 모든 페이지의 첫 번째 포커스 가능한 엘리먼트가 되기 때문에 App.vue 상단에서 수행된다.

                <ul class="skip-links">
                    <li>
                        <a href="#main" ref="skipLink">주요 컨텐츠로 건너뛰기</a>
                    </li>
                </ul>

            아래 스타일을 추가해 포커스가 되지 않은 링크를 숨길 수 있다.

                .skipLink {
                white-space: nowrap;
                margin: 1em auto;
                top: 0;
                position: fixed;
                left: 50%;
                margin-left: -72px;
                opacity: 0;
                }
                .skipLink:focus {
                opacity: 1;
                background-color: white;
                padding: 0.5em;
                border: 1px solid black;
                }

            사용자가 경로를 변경하면 건너뛰기 링크로 포커스를 다시 가져온다.
            이것은 템플릿 ref의 건너뛰기 링크에 포커스를 호출하여 구현할 수 있다(vue-router 사용 가정)

                <script setup>
                import { ref, watch } from 'vue'
                import { useRoute } from 'vue-router'

                const route = useRoute()
                const skipLink = ref()

                watch(
                    () => route.path,
                    () => {
                        skipLink.value.focus()
                    }
                )
                </script>

    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

        컨텐츠 구조

            접근성의 가장 중요한 부분 중 하나는 디자인이 접근성 구현을 지원할 수 있는지 확인하는 것이다.
            디자인은 색상 대비, 글꼴 선택, 텍스트 크기 및 언어뿐만 아니라 앱에서 컨텐츠가 구성되는 방식도 고려해야 한다.

            제목

                사용자는 제목을 통해 앱을 탐색할 수 있다. 앱의 모든 섹션에 제목이 있으면, 사용자가 각 섹션의 내용을 더 쉽게 예측할 수 있다.
                제목과 관련하여 몇 가지 권장되는 접근성이 있다.

                1. 순위에 따라 제목 삽입: <h1> - <h2>
                2. 섹션 내에서 제목을 건너뛰지 않기
                3. 제목의 시각적 모양을 제공하기 위해 텍스트 스타일 지정 대신 실제 제목 태그를 사용

                    <main role="main" aria-labelledby="main-title">
                        <h1 id="main-title">메인 제목</h1>
                        <section aria-labelledby="section-title">
                            <h2 id="section-title"> 섹션 제목 </h2>
                            <h3>섹션 부 제목</h3>
                            //컨텐츠
                        </section>
                        <section aria-labelledby="section-title">
                            <h2 id="section-title"> 섹션 제목 </h2>
                            <h3>섹션 부 제목</h3>
                            //컨텐츠
                            <h3>섹션 부 제목</h3>
                            //컨텐츠
                        </section>
                    </main>

            랜드마크

                랜드마크는 앱 내의 섹션에 대한 프로그래밍 방식 접근을 제공한다.
                보조 기술에 의존하는 사용자는 앱의 각 섹션으로 이동하여 컨텐츠를 건너뛸 수 있다.
                ARIA roles를 사용하여 이를 달성할 수 있다.

                HTML    ARIA Role               랜드마크 용도
                header  role="banner"           주요 제목: 페이지 제목
                nav     rele="navigation"       문서 또는 관련 문서를 탑색할 때 사용하기에 적합한 링크 모음
                main    rele="main"             문서의 주요 또는 중심 컨텐츠
                footer  rele="contentinfo"      상위 문서에 대한 정보: 각주/저작권/개인정보 취급방침 링크
                aside   rele="conplementary"    메인 컨텐츠를 보조하지만, 분리되어 있고, 그 내용 자체로 독립된 의미가 있음
                search  rele="search"           이 섹션은 애플리케이션의 검색 기능을 포함하고 있다.
                form    rele="form"             양식에 관련된 엘리먼트를 감싸고 있음
                section rele="region"           관련성이 있고 사용자가 탐색하고 싶어할 것 같은 컨텐츠. 이것을 사용한 엘리먼트에는 반드시 aria-label 제공 필요

                TIP
                    HTML5 시멘틱 엘리먼트를 지원하지 않는 오래된 브라우저와 최대한 호환되도록, 랜드마크 HTML 엘리먼트에 (의미적으로 중복되더라도) 랜드마크 role 속성을 사용하는 것이 좋다.

    
    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

        의미있는 양식(Semantic form)

            양식(form)을 만들 때 <form>, <label>, <input>, <textarea>, <button> 엘리먼트를 사용할 수 있다.
            
            레이블은 일반적으로 양식 필드의 상단이나 왼쪽에 배치된다.

                <form action="/dataCollectionLocation" method="post" autocomplete="on">
                    <div v-for="item in formItems" :key="item.id" class="form-item">
                        <label :for="item.id">{{ item.label }}: </label>
                        <input
                        :type="item.type"
                        :id="item.id"
                        :name="item.id"
                        v-model="item.value"
                        />
                    </div>
                    <button type="submit">제출하기</button>
                </form>

            form 엘리먼트에 autocomplate='on'을 포함하면 form의 모든 input에 적용된다.
            각 input에 대해 다른 자동 완성 속성 값을 설정할 수도 있다.

            레이블

                모든 양식의 용도를 설명하는 레이블을 제공한다. for와 id속성을 사용하여 연결하여

                    <label for="name">이름:</label>
                    <input type="text" name="name" id="name" v-model="name" />

                크롬 개발자 도구에서 이 엘리먼트를 검사하고, 엘리먼트 탭에서 접근성 탭을 열면, <input>이 <label>에서 이름을 가져오는 방법을 볼 수 있다.

            
            경고
                다음과 같이 입력 필드를 래핑하는 레이블을 보았을 수도 있다.

                    <label>
                        이름:
                        <input type="text" name="name" id="name" v-model="name" />
                    </label>

                일치하는 ID로 레이블을 명시적으로 설정하는 것이 보조 기술에서 더 잘 지원된다.


            aira-label

                aira-label을 사용하여 input에 접근 가능한 이름을 지정할 수도 있다.

                <label for="name">이름:</label>
                <input
                    type="text"
                    name="name"
                    id="name"
                    v-model="name"
                    :aria-label="nameLabel"
                />

            aira-labelledby

                aira-labelledby를 사용하는 것은 화면에 레이블 텍스트가 표시되는 경우를 제외하고는 aria-label과 유사하다.
                이는 다른 엘리먼트들과 id로 쌍을 이루며, 여러 개의 id를 연결할 수 있다.

                    <form
                        class="demo"
                        action="/dataCollectionLocation"
                        method="post"
                        autocomplete="on"
                    >
                        <h1 id="billing">계산서</h1>
                        <div class="form-item">
                            <label for="name">이름:</label>
                            <input
                                type="text"
                                name="name"
                                id="name"
                                v-model="name"
                                aria-labelledby="billing name"
                            />
                        </div>
                        <button type="submit">제출하기</button>
                    </form>

            aira-describedby
            
                aira-describedby는 사용자에게 필요할 수 있는 설명에 대한 추가 정보를 제공한다는 점을 제외하고는 aira-labelledby와 같은 방식으로 사용된다.
                이것은 input 특징을 설명하는 데 사용할 수 있다.

                    <form
                        class="demo"
                        action="/dataCollectionLocation"
                        method="post"
                        autocomplete="on"
                    >
                        <h1 id="billing">계산서</h1>
                        <div class="form-item">
                            <label for="name">이름:</label>
                            <input
                                type="text"
                                name="name"
                                id="name"
                                v-model="name"
                                aria-labelledby="billing name"
                                aria-describedby="nameDescription"
                            />
                            <p id="nameDescription">이름과 성을 입력하십시오.</p>
                        </div>
                        <button type="submit">제출하기</button>
                    </form>

            플레이스 홀더(Placeholder)

                플레이스홀더는 많은 사용자를 혼란스럽게 할 수 있으므로 사용하지 않아야 한다.

                플레이스홀더의 문제 중 하나는 기본적으로 색상 대비 기준을 충족하지 않는다는 것이다. 
                색상 대비를 수정하면 플레이스홀더가 입력 필드에 미리 채워진 데이터처럼 보인다.
                
                    <form
                        class="demo"
                        action="/dataCollectionLocation"
                        method="post"
                        autocomplete="on"
                    >
                        <div v-for="item in formItems" :key="item.id" class="form-item">
                            <label :for="item.id">{{ item.label }}: </label>
                            <input
                                type="text"
                                :id="item.id"
                                :name="item.id"
                                v-model="item.value"
                                :placeholder="item.placeholder"
                            />
                        </div>
                        <button type="submit">제출하기</button>
                    </form>

                    /* https://www.w3schools.com/howto/howto_css_placeholder.asp */

                    #lastName::placeholder {
                        /* Chrome, Firefox, Opera, Safari 10.1+ */
                        color: black;
                        opacity: 1; /* Firefox */
                    }

                    #lastName:-ms-input-placeholder {
                        /* Internet Explorer 10-11 */
                        color: black;
                    }

                    #lastName::-ms-input-placeholder {
                        /* Microsoft Edge */
                        color: black;
                    }

                사용자가 양식을 작성하는 데 필요한 모든 정보를 input 외부에 제공하는 것이 가장 좋다.

            지침

                입력 필드에 대한 지침을 추가할 때 input에 올바르게 연결해야 한다. 추가 지침을 제공하고 aira-labelledby 내부에 여러 ID를 바인딩할 수 있다.
                이를 통해 보다 유연한 설계가 가능하다.

                    <fieldset>
                        <legend>aria-labelledby 사용</legend>
                        <label id="date-label" for="date">현재 날짜:</label>
                        <input
                            type="date"
                            name="date"
                            id="date"
                            aria-labelledby="date-label date-instructions"
                        />
                        <p id="date-instructions">월/일/년</p>
                    </fieldset>

                또는 aria-describedby를 사용하여 입력에 지침을 첨부할 수 있다.

                    <fieldset>
                        <legend>aria-describedby 사용</legend>
                        <label id="dob" for="dob">생일:</label>
                        <input type="date" name="dob" id="dob" aria-describedby="dob-instructions" />
                        <p id="dob-instructions">월/일/년</p>
                    </fieldset>

            컨텐츠 숨기기

                일반적으로 input에 접근 가능한 이름이 있더라도 레이블을 시각적으로 숨기는 것은 권장되지 않는다.
                그러나 input의 역할을 주변 컨텐츠로 이해할 수 있다면 레이블을 시각적으로 숨길 수 있다.

                이 검색 필드를 살펴보자

                    <form role="search">
                        <label for="search" class="hidden-visually">검색: </label>
                        <input type="text" name="search" id="search" v-model="search" />
                        <button type="submit">검색</button>
                    </form>

                검색 버튼이 시각적으로 사용자가 입력 필드의 목적을 식별하는 데 도움이 된다.

                CSS를 사용하여 시각적으로 엘리먼트를 숨길 수 있지만, 보조 기술에 계속 사용할 수 있도록 유지할 수 있다.

                .hidden-visually {
                    position: absolute;
                    overflow: hidden;
                    white-space: nowrap;
                    margin: 0;
                    padding: 0;
                    height: 1px;
                    width: 1px;
                    clip: rect(0 0 0 0);
                    clip-path: inset(100%);
                }

            aria-hidden-"true"

                aria-hidden-"true"를 추가하면 보조 기술에서 엘리먼트가 숨겨지지만, 다른 사용자는 시각적으로 사용할 수 있다.
                초점을 맞추는 엘리먼트, 오직 꾸미지 위한 용도, 복제 또는 화면에 나오지 않는 컨텐츠에 사용하면 안된다.

                    <p>이것은 스크린 리더에서 숨겨지지 않습니다.</p>
                    <p aria-hidden="true">이것은 스크린 리더에서 숨겨져 있습니다.</p>

            버튼

                폼 내에서 버튼을 사용할 때 폼이 제출되지 않도록 타입을 설정해야 한다.
                input을 사용하여 버튼을 만들 수도 있다.

                <form action="/dataCollectionLocation" method="post" autocomplete="on">
                    //버튼
                    <button type="button">취소</button>
                    <button type="submit">제출하기</button>

                    //input 버튼
                    <input type="button" value="취소" />
                    <input type="submit" value="제출하기" />
                </form>

            기능적 이미지

                이 기술을 사용하여 기능적 이미지를 만들 수 있다.

                입력 필드

                    이 이미지는 양식에서 submit 타입 버튼으로 작동한다.

                        <form role="search">
                            <label for="search" class="hidden-visually">검색: </label>
                            <input type="text" name="search" id="search" v-model="search" />
                            <input
                                type="image"
                                class="btnImg"
                                src="https://img.icons8.com/search"
                                alt="검색"
                            />
                        </form>
                아이콘

                    <form role="search">
                        <label for="searchIcon" class="hidden-visually">검색: </label>
                        <input type="text" name="searchIcon" id="searchIcon" v-model="searchIcon" />
                        <button type="submit">
                            <i class="fas fa-search" aria-hidden="true"></i>
                            <span class="hidden-visually">검색</span>
                        </button>
                    </form>

    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

        표준

            W3C(World Wide Web Consortium) WAI(Web Accessibility Initiative)는 다양한 컴포넌트에 대한 웹 접근성 표준을 개발한다.

            1. 유저 에이전트 접근성 지침(UAAG): User Agent Accessibility Guidelines)
            
                보조 기술의 일부 측면을 포함한 웹 브라우저 및 미디어 플레이어

            2. 저작 도구 접근성 지침(ATAG: Authoring Tool Accessibility Guidelines)
            
                저작 도구

            3. 웹 컨텐츠 접근성 지침(WCAG: Web Content Accessibility Guidelines)

                웹 컨텐츠 - 개발자, 저작 도구 및 접근성 평가 도구에서 사용

        웹 컨텐츠 접근성 지침(WCAG: Web Content Accessibility Guidelines)

            WCAG 2.1은 WCAG 2.0을 확장하고 웹의 변경 사항을 처리하여 새로운 기술을 구현할 수 있다.
            W3C는 웹 접근성 정책을 개발하거나 업데이트할 때 최신 버전의 WCAG를 사용하도록 권장한다.

            WCAG 2.1dml 4가지 주요 기본 원칙(약칭:POUR)

                1. Perceivable(인지가능)
                
                    - 사용자는 제공되는 정보를 인지할 수 있어야 한다.

                2. Operable(작동가능)

                    - 인터페이스 양식, 컨트롤 및 탐색이 작동 가능하다.

                3. Understandable(이해가능)

                    - 사용자 인터페이스의 정보 및 작동은 모든 사용자가 이해할 수 있어야 한다.

                4. Robust

                    - 기술이 발전함에 따라 사용자가 컨텐츠에 접근할 수 있어야 한다.





-->
<script>
export default {
	setup() {
		return {};
	},
};
</script>

<style lang="scss" scoped></style>
