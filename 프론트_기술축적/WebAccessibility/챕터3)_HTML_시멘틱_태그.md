# 태그를 의미 있게 사용해야 하는 이유

## 의미 있는 태그를 사용했을 때 장점

**검색엔진 최적화** 검색 엔진은 시맨틱 태그를 통해 콘텐츠의 구조와 중요도를 파악합니다. `<article>`, `<nav>`, `<main>` 등의 태그는 페이지의 핵심 콘텐츠와 부가 정보를 구분하는 데 도움을 주어 검색 순위에 긍정적인 영향을 미칩니다.

**웹접근성 높이기** 스크린 리더와 같은 보조 기술이 시맨틱 태그를 통해 페이지 구조를 이해하고 사용자에게 전달합니다. 예를 들어 `<nav>` 태그는 "탐색 영역"으로, `<main>` 태그는 "주요 콘텐츠"로 인식되어 시각 장애인도 웹사이트를 효율적으로 탐색할 수 있습니다.

**가독성, 유지보수성** 의미 있는 태그는 코드의 의도를 명확하게 전달합니다. `<div class="header">`보다 `<header>`가 더 직관적이며, 개발자 간 협업과 향후 코드 수정 시 구조 파악이 용이합니다.

## 의미 있는 태그들

#### 제목
**h1 ~ h6**
- 페이지의 계층 구조를 나타내며, h1은 페이지당 하나만 사용하는 것이 권장됨
- 순서대로 사용해야 함 (h1 → h2 → h3, h2를 건너뛰고 h3 사용 지양)
- SEO와 접근성에서 매우 중요한 역할

#### 레이아웃
**header**: 페이지나 섹션의 머리말 영역, 로고, 네비게이션, 검색 등 포함

**footer**: 페이지나 섹션의 바닥글, 저작권, 연락처, 사이트맵 등 포함

**main**: 문서의 주요 콘텐츠 영역, 페이지당 하나만 사용, 사이드바나 네비게이션 제외

**article**: 
- 독립적으로 분리 가능한 영역 (블로그 포스트, 뉴스 기사, 제품 카드)
- 중첩해서 사용 가능함

**section**: 
- 주제별로 그룹화된 콘텐츠 영역
- 일반적으로 제목을 포함하여 나열되는 경우
- 콘텐츠 내용이 문서의 개요에 포함되는 경우에 적합하다.

**aside**: 
- 주요 콘텐츠와 간접적으로 연관된 부가 정보 (사이드바, 광고, 관련 링크)
- 본문 콘텐츠에 영향을 미치지 않아야 한다.


##### 랜드마크 탐색
> 랜드마크 역할을 가진 태그 사이를 이동하는 탐색 기능
> 랜드마크 역할 가지는 태그들 : `header`, `nav`, `main`, `section`, `aside`, `footer`, `form`

- header : 다른 세션요소(`nav`, `article`, `section`, `aside`)의 자식요소로 사용되지 않을땐, banner 랜드마크 역할 수행
- nav : navigation 랜드마크 역할 수행
- main : main 랜드마크 역할 수행
- section : 레이블이 지정된 경우, region 랜드마크 역할 수행
- aside : complementary )보조적인 정보제공) 랜드마크 역할 수행
- footer : 다른 세션요소(`main`,`nav`, `article`, `section`, `aside`)의 자식요소로 사용되지 않을땐, contentinfo 랜드마크 역할 수행
- form : 레이블이 지정된 경우에만 form 랜드마크 역할 수행


#### 텍스트
**p, span**
- p: 단락을 나타내는 블록 레벨 요소
- span: 인라인 텍스트를 그룹화, 스타일링 목적으로 주로 사용

**strong, b**
- strong: 중요한 내용을 나타냄 (의미적 강조)
- b: 시각적으로만 굵게 표시 (의미 없음)

**em, i**
- em: 강조하고 싶은 내용, 이탤릭체로 표시 (의미적 강조)
- i: 강조하고 싶은 내용,  이탤릭체로 표시 (의미없음 : 기술 용어, 외국어 등)

**time**: 날짜와 시간 정보를 나타냄, datetime 속성으로 기계 판독 가능한 형식 제공

**br, hr**
- br: 줄바꿈 (시나 주소 등 필요한 경우만 사용)
- hr: 주제 구분선 (섹션 간 논리적 구분)

#### 목록
**ul, ol**
- ul: 순서가 없는 목록 (불릿 포인트)
- ol: 순서가 있는 목록 (번호, 알파벳 등)
	- type 속성 : 글머리기호 지정
	- start 속성 : 시작되는 첫 글머리 기호(번호)를 지정할 수 있음
	- reversed 속성 : 역순으로 정렬

**li**: 목록 항목, ul이나 ol의 직계 자식 요소로만 사용
**dl**: 정의 목록, key-value 형태의 항목
- **dt**: 정의할 용어 (Definition Term)
- **dd**: 용어에 대한 설명 (Definition Description)

#### 양식
**form**: 사용자 입력을 서버로 전송하는 양식 컨테이너
- name 속성
- method 속성
- action 속성 : 제출된 데이터를 처리할 URL지정
- target 속성 : 데이터 처리 후 응답을 표시할 위치를 지정
- autocomplete 속성

**input** (*활용도가 높음)
속성들:
- **type**
    - text: 일반 텍스트 입력
    - number: 숫자 입력, 증감 버튼 제공
    - tel: 전화번호 입력, 모바일에서 숫자 키패드
    - password: 비밀번호 입력, 문자 마스킹
    - email: 이메일 주소, 자동 유효성 검사
    - checkbox: 다중 선택 가능한 체크박스
    - radio: 단일 선택만 가능한 라디오 버튼
    - range: 슬라이더로 범위 내 값 선택
    - ~~submit: 폼 제출 버튼~~ (권장하지 않음)
    - color: 색상 선택기
    - date: 날짜 선택
    - datetime-local: 날짜와 시간 선택
    - month: 연월 선택
    - week: 주 선택
    - time: 시간 선택
    - image: 이미지 버튼 (제출 기능)
    - url: URL 입력, 자동 유효성 검사
    - file: 파일 업로드
    - search: 검색 입력 (X 버튼 제공)
    - reset: 폼 초기화 버튼
    - hidden: 사용자에게 보이지 않는 데이터 전송
- **value**: 입력 필드의 초기값 또는 현재값
- **autocomplete**
	자동완성은 디바이스, 브라우저 환경에 따라 authcomplete 속성과 무관하게 name 값에 의해 자동 완성이 되기도 한다. ***그러므로 name 과 autocomplete 속성을 동일하게 정해진 값으로 관리 제공하는 것이 좋다*.**
    - off: 자동완성 비활성화
    - on: 자동완성 활성화
    - name: 전체 이름
    - given-name: 이름
    - family-name: 성
    - nickname: 닉네임
    - email
    - username
    - current-passwort
    - one-time-code
    - country
    - bday
    - bday-day
    - bday-month
    - bday-year
    - tel
- **[[inputmode]]**: 모바일 키보드 타입 제어 (numeric, tel, email, url 등)
    
**label**
입력 필드에 대한 설명 텍스트, 클릭 시 연결된 입력 필드로 포커스 이동
구현방법:
***명시적 레이블링을 우선적으로 사용하되, 쿠드 구조상 암묵적 레이블링이 필요한 경우에는 id-for 연결을 활용해 접근성을 보완해야한다.***
- 암묵적: `<label>텍스트 <input></label>`
- 명시적: `<label for="id">텍스트</label> <input id="id">`
속성들:
- htmlFor(React) / for(HTML): 연결할 input의 id 지정
- id: label과 연결할 input의 고유 식별자

**select > option**
- select: 드롭다운 목록
- option: 선택 가능한 항목
속성들:
- name: 폼 제출 시 서버로 전송될 필드명
- required: 필수 입력 필드 지정
- multiple: 다중 선택 가능
- size: 한 번에 보여질 옵션 개수

**textarea**: 여러 줄 텍스트 입력 영역
속성들:
- rows: 보여질 행 수
- cols: 보여질 열 수
- minlength: 최소 글자 수
- maxlength: 최대 글자 수

**fieldset > legend**
폼 내부에서 영역을 구분해주는 역할
상황에 따라서 fieldset 을 중첩해서 사용할 수 있다.
- fieldset: 관련된 폼 요소들을 그룹화
- legend: fieldset의 제목/설명

#### 표
**table**: 표 컨테이너
**caption**: 표의 제목/설명
**thead**: 표의 헤더 영역
**tbody**: 표의 본문 영역
**tfoot**: 표의 바닥글 영역
**tr, th, td**
- tr: 표의 행
- th: 표의 헤더 셀
- td: 표의 데이터 셀
속성들:
- **scope**: th 요소가 어느 셀들의 헤더인지 지정
    - row: 같은 행의 셀들
    - col: 같은 열의 셀들
    - rowgroup: 행 그룹의 셀들
    - colgroup: 열 그룹의 셀들
- **rowspan**: 셀이 차지할 행의 개수
- **colspan**: 셀이 차지할 열의 개수
- **id, headers**: 복잡한 표에서 헤더와 데이터 셀 연결
**colgroup, col**: 열 단위 스타일 적용
속성들:
- span: 적용할 열의 개수

#### 대화형 요소
**a**: 하이퍼링크
- **href**: 링크 대상 지정
    - tel: 전화번호 링크 `href="tel:010-1234-5678"`
    - mailto: 이메일 링크 `href="mailto:example@email.com"`
    - download: 파일 다운로드 `href="file.pdf" download`
- **rel**: 현재 문서와 링크된 리소스의 관계
    - nofollow: 검색엔진이 따라가지 않음
    - noopener: 새 창에서 opener 참조 방지 (보안)
    - noreferrer: HTTP 리퍼러 전송 방지
- **target**: 링크 열기 방식
    - _self: 현재 창 (기본값)
    - _blank: 새 창/탭
    - _parent: 부모 프레임 (ifram이 될 수 있음)
    - _top: 최상위 창

**button**: 클릭 가능한 버튼
- **type**:
    - button: 일반 버튼 (JavaScript와 함께 사용)
    - submit: 폼 제출 버튼 (기본값)
    - reset: 폼 초기화 버튼


a 태그의 rel 속성에서 nofollow 속성값
- SEO 검색엔진이 크롤링 시 쓸데 없는 페이지를 크롤링 하여, 크롤링 버짓을 소모하지 않도록 명시해주는 것이 좋다

```html
<a href="..." rel=">이동</a>
```
https://codingeverybody.kr/html-rel-%EC%86%8D%EC%84%B1-%EB%A7%81%ED%81%AC-%EA%B4%80%EA%B3%84-%EB%82%98%ED%83%80%EB%82%B4%EB%8A%94-%EC%86%8D%EC%84%B1/
