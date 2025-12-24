# 현상

기존 PC web 서비스를 Mobile web 도 대응하게 되면서, 기존에 [(keycloak)] 기반의 인증화면에서 이슈가 발생했다

아래와 같이 OTA 인증을 하는 화면이 있는데, 모바일에서 해당 Input 을 클릭하면 '텍스트' 키보드가 먼저 뜬다는 게 보고 내용이다.

![[auth_page_image.png]]

# 나의 잘못

처음에는 input 에 type 으로 text 가 들어가있는 것을 보고 이를 number 로 주면 될 것이라고 생각했다.

# 실제 해결

[(웹접근성_지침)] 에서 공부하면ㅅ [(inputmode)] 속성을 습득한적이 있었는데.. 그때는 그렇구나 넘겼던것 같다. 하지만 모바일은 작은 화면에서 사용자 경험을 극대화 해야하다보니 이런 하나하나가 매우 엄격한 것 같다.

여기서 inputmode 를 numberic 을 주면 숫자 키보드 패드가 뜬다. 그 외에 none, text, decimal, tel, search 등등 다양한 값들을 선택해서, 목적에 맞는 키보드가 뜨게끔 할 수 있다. [참고자료](https://mong-blog.tistory.com/entry/html-%EB%8D%94-%EB%82%98%EC%9D%80-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%9E%85%EB%A0%A5%EC%9D%84-%EC%9C%84%ED%95%9C-inputmode-%EC%86%8D%EC%84%B1#google_vignette)

이 참에 모바일에 자주 활용되는 웹접근성 속성들을 정리해보면 좋을 것 같다.

---

# 모바일 웹 접근성 주요 속성 정리

## 1. inputmode
가상 키보드의 종류를 지정하여 사용자 입력 경험을 최적화

```html
<input type="text" inputmode="numeric" /> <!-- 숫자 키패드 -->
<input type="text" inputmode="decimal" /> <!-- 소수점 포함 숫자 -->
<input type="text" inputmode="tel" />     <!-- 전화번호 키패드 -->
<input type="text" inputmode="email" />   <!-- 이메일 키보드 -->
<input type="text" inputmode="url" />     <!-- URL 키보드 -->
<input type="text" inputmode="search" />  <!-- 검색 최적화 키보드 -->
```

**주요 값:**
- `none`: 키보드를 띄우지 않음 (사용자 정의 키보드 사용 시)
- `text`: 일반 텍스트 키보드
- `decimal`: 소수점이 포함된 숫자 키패드
- `numeric`: 숫자 키패드
- `tel`: 전화번호 입력용 키패드
- `search`: 검색에 최적화된 키보드
- `email`: 이메일 주소 입력용 키보드 (@, . 포함)
- `url`: URL 입력용 키보드 (/, . 포함)

## 2. enterkeyhint
가상 키보드의 Enter 키 레이블을 제어

```html
<input type="text" enterkeyhint="search" />  <!-- 검색 -->
<input type="text" enterkeyhint="done" />    <!-- 완료 -->
<input type="text" enterkeyhint="go" />      <!-- 이동 -->
<input type="text" enterkeyhint="next" />    <!-- 다음 -->
<input type="text" enterkeyhint="send" />    <!-- 전송 -->
```

**사용 예시:**
- 검색창: `enterkeyhint="search"`
- 로그인 폼 마지막 필드: `enterkeyhint="done"`
- 다단계 폼: `enterkeyhint="next"`
- 메시지 전송: `enterkeyhint="send"`

## 3. autocomplete
자동완성 기능 제어 및 브라우저에 입력값 유형 힌트 제공

```html
<input type="text" autocomplete="name" />
<input type="email" autocomplete="email" />
<input type="tel" autocomplete="tel" />
<input type="text" autocomplete="cc-number" /> <!-- 신용카드 번호 -->
<input type="text" autocomplete="off" /> <!-- 자동완성 비활성화 -->
```

**주요 값:**
- `off`: 자동완성 비활성화
- `on`: 자동완성 활성화
- `name`, `email`, `tel`: 개인정보 필드
- `address-line1`, `postal-code`: 주소 관련
- `cc-number`, `cc-exp`: 신용카드 정보

## 4. autocapitalize (iOS)
iOS에서 자동 대문자 변환 제어

```html
<input type="text" autocapitalize="off" />        <!-- 대문자 변환 안 함 -->
<input type="text" autocapitalize="sentences" />  <!-- 문장 첫 글자 -->
<input type="text" autocapitalize="words" />      <!-- 각 단어 첫 글자 -->
<input type="text" autocapitalize="characters" /> <!-- 모든 글자 -->
```

**사용 예시:**
- 이메일, 아이디 입력: `autocapitalize="off"`
- 일반 텍스트: `autocapitalize="sentences"`
- 이름 입력: `autocapitalize="words"`

## 5. autocorrect (iOS)
iOS에서 자동 맞춤법 교정 제어

```html
<input type="text" autocorrect="off" /> <!-- 자동 교정 비활성화 -->
<input type="text" autocorrect="on" />  <!-- 자동 교정 활성화 -->
```

**사용 예시:**
- 코드, 아이디, 이메일 입력: `autocorrect="off"`
- 일반 텍스트 입력: `autocorrect="on"`

## 6. pattern
입력값 유효성 검사를 위한 정규표현식 패턴

```html
<!-- 전화번호 (숫자만) -->
<input type="tel" pattern="[0-9]{10,11}" inputmode="tel" />

<!-- 우편번호 -->
<input type="text" pattern="[0-9]{5}" inputmode="numeric" />

<!-- 영문자만 -->
<input type="text" pattern="[A-Za-z]+" />
```

**장점:**
- HTML5 기본 유효성 검사 활용
- 모바일에서 적절한 키보드와 함께 사용하면 UX 향상

## 7. maxlength / minlength
입력 가능한 문자 수 제한

```html
<input type="text" maxlength="11" /> <!-- 최대 11자 -->
<input type="text" minlength="2" />  <!-- 최소 2자 -->
```

**모바일 활용:**
- 전화번호, 인증번호 등 고정 길이 입력 시 유용
- 사용자가 불필요한 입력을 시도하지 않도록 방지

## 8. readonly vs disabled
입력 필드의 상태 제어

```html
<input type="text" readonly value="수정 불가" />  <!-- 읽기 전용 -->
<input type="text" disabled value="비활성화" />   <!-- 비활성화 -->
```

**차이점:**
- `readonly`: 포커스 가능, 폼 제출 시 값 포함, 스크린 리더 읽음
- `disabled`: 포커스 불가, 폼 제출 시 값 제외, 시각적으로 비활성화

**모바일 활용:**
- 확인용 필드는 `readonly` 사용 (터치 시 키보드 안 뜸)
- 조건부 활성화 필드는 `disabled` 사용

## 9. aria 속성들
스크린 리더 및 보조 기술 지원

```html
<!-- aria-label: 요소에 레이블 제공 -->
<button aria-label="메뉴 닫기">✕</button>

<!-- aria-labelledby: 다른 요소를 레이블로 참조 -->
<input type="text" aria-labelledby="username-label" />
<label id="username-label">사용자명</label>

<!-- aria-describedby: 추가 설명 제공 -->
<input type="password" aria-describedby="password-hint" />
<span id="password-hint">8자 이상, 영문+숫자 포함</span>

<!-- aria-required: 필수 입력 표시 -->
<input type="text" aria-required="true" />

<!-- aria-invalid: 유효성 검사 실패 표시 -->
<input type="email" aria-invalid="true" />

<!-- aria-live: 동적 콘텐츠 변경 알림 -->
<div aria-live="polite">검색 결과 10개</div>
```

## 10. role 속성
요소의 역할을 명시적으로 정의

```html
<div role="button" tabindex="0">클릭 가능한 div</div>
<nav role="navigation">네비게이션</nav>
<div role="alert">오류 메시지</div>
<div role="status">로딩 중...</div>
```

**모바일 활용:**
- 커스텀 UI 컴포넌트에 적절한 역할 부여
- 스크린 리더 사용자에게 요소의 목적 명확히 전달

## 11. tabindex
키보드 포커스 순서 제어

```html
<div tabindex="0">포커스 가능</div>  <!-- 자연스러운 순서로 포커스 -->
<div tabindex="-1">프로그램적으로만 포커스</div> <!-- 탭 키로 불가 -->
<button tabindex="1">첫 번째</button> <!-- 권장하지 않음 -->
```

**모바일 활용:**
- 커스텀 인터랙티브 요소에 키보드 접근성 제공
- 모달, 드로어 등에서 포커스 트랩 구현

## 12. 터치 타겟 크기
모바일 터치 인터페이스를 위한 최소 크기 권장사항

```css
/* 최소 터치 영역: 44x44px (iOS), 48x48px (Android) */
button {
  min-width: 48px;
  min-height: 48px;
  padding: 12px;
}

/* 작은 버튼은 패딩으로 터치 영역 확보 */
.icon-button {
  padding: 12px;
  /* 아이콘 24x24px + 패딩 12px = 터치 영역 48x48px */
}
```

## 13. 실전 조합 예시

### 전화번호 입력
```html
<input
  type="tel"
  inputmode="tel"
  autocomplete="tel"
  pattern="[0-9]{10,11}"
  maxlength="11"
  placeholder="01012345678"
  aria-label="전화번호"
  required
/>
```

### 이메일 입력
```html
<input
  type="email"
  inputmode="email"
  autocomplete="email"
  autocapitalize="off"
  autocorrect="off"
  placeholder="example@email.com"
  aria-label="이메일 주소"
  required
/>
```

### OTP 인증번호 입력
```html
<input
  type="text"
  inputmode="numeric"
  autocomplete="one-time-code"
  pattern="[0-9]{6}"
  maxlength="6"
  placeholder="000000"
  aria-label="인증번호 6자리"
  required
/>
```

### 검색창
```html
<input
  type="search"
  inputmode="search"
  autocomplete="off"
  enterkeyhint="search"
  placeholder="검색어를 입력하세요"
  aria-label="검색"
/>
```

### 가격 입력
```html
<input
  type="text"
  inputmode="decimal"
  pattern="[0-9]+(\.[0-9]{1,2})?"
  placeholder="0.00"
  aria-label="가격"
/>
```

## 14. 주의사항

1. **type vs inputmode**
   - `type="number"`는 스피너(증가/감소 버튼)가 표시되어 모바일에서 불편할 수 있음
   - 순수 숫자 입력은 `type="text" + inputmode="numeric"` 조합 권장

2. **autocomplete 보안**
   - 민감한 정보(비밀번호, 카드번호)는 `autocomplete="off"` 고려
   - 단, UX와 보안의 균형을 고려해야 함

3. **iOS vs Android**
   - `autocapitalize`, `autocorrect`는 iOS 전용
   - 크로스 플랫폼 대응 시 feature detection 고려

4. **접근성 테스트**
   - 실제 모바일 기기에서 테스트
   - VoiceOver (iOS), TalkBack (Android) 스크린 리더로 검증
   - 키보드만으로 모든 기능 사용 가능한지 확인

## 참고 자료
- [MDN - inputmode](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/inputmode)
- [MDN - enterkeyhint](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/enterkeyhint)
- [MDN - autocomplete](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete)
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [Apple Human Interface Guidelines - Touch Targets](https://developer.apple.com/design/human-interface-guidelines/inputs/touchscreen-gestures)
