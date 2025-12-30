# WAI-ARIA 완벽 가이드: 웹 접근성을 위한 필수 지식

웹 애플리케이션이 점점 복잡해지고 동적으로 변하면서 시맨틱 태그만으로는 스크린 리더 사용자가 해당 UI의 명확한 기능을 파악하기 어려워졌습니다. 이를 해결하기 위해 W3C에서 작성한 기술 문서가 바로 **WAI-ARIA(Web Accessibility Initiative - Accessible Rich Internet Applications)**입니다.

**[추가]** ARIA는 HTML 요소에 추가적인 의미(semantic)를 부여하여 보조 기술이 콘텐츠를 더 정확하게 해석할 수 있도록 돕습니다. 중요한 점은 ARIA가 스크린 리더를 위한 접근성 트리를 형성하는 데 도움을 줄 뿐, 실제 동작에는 영향을 주지 않는다는 것입니다. 따라서 **ARIA 역할에 상응하는 기능 구현은 반드시 함께 구현되어야 합니다.**

## W3C에서 권장하는 ARIA 사용 5대 규칙

1. **ARIA를 사용하기 전에 기본 HTML 요소를 우선적으로 고려한다**
    - **[추가]** 예: `<button>` 대신 `<div role="button">`을 사용하지 마세요
2. **꼭 필요한 경우가 아니라면 요소의 기본 의미를 변경하지 않는다**
    - **[추가]** 예: `<h2 role="tab">`보다는 `<div role="tab">`을 사용하세요
3. **상호작용이 가능한 ARIA 역할은 반드시 키보드 사용성을 보장해야 한다**
    - **[추가]** 모든 대화형 요소는 키보드로 접근 가능해야 하며, Enter/Space 키 이벤트를 처리해야 합니다
4. **초점 가능한 요소에 `role="presentation"` 또는 `aria-hidden="true"` 속성을 사용하지 않는다**
    - **[추가]** 이는 키보드 사용자와 스크린 리더 사용자 간의 경험 불일치를 야기합니다
5. **상호작용이 가능한 대화형 요소에는 반드시 접근성 API가 접근할 수 있는 이름이 있어야 한다**
    - **[추가]** `aria-label`, `aria-labelledby`, 또는 내부 텍스트 콘텐츠를 통해 제공되어야 합니다

## 자주 사용하는 ARIA 역할 (Roles)

### 1. aria-role="alert"

[MDN Reference](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/alert_role)

중요한 정보를 즉시 사용자에게 전달할 때 사용합니다. alert 역할 사용 시 초점의 위치와 무관하게 alert의 내부 콘텐츠를 스크린 리더가 읽어주므로 별도의 초점 이동이 필요 없습니다.

**주의사항:**

- 내부 콘텐츠로는 텍스트만 사용해야 합니다
- `role="alert"` 속성 자체가 동적으로 추가/수정되어서는 안 됩니다 (스크린 리더가 인지하지 못함)
- **[추가]** alert는 aria-live="assertive"와 aria-atomic="true"가 암시적으로 적용됩니다

**[추가] 사용 예시:**

```html
<div role="alert">
  <p>양식 제출이 완료되었습니다!</p>
</div>
```

### 2. aria-role="alertdialog"

[MDN Reference](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/alertdialog_role)

사용자에게 중요 정보를 알리면서 동시에 상호작용을 요구할 때 사용합니다.

**필수 요구사항:**

- Modal 형태로 사용되어야 합니다 (외부 콘텐츠 탐색 불가)
- 내부에서 초점이 순환되어야 합니다
- ESC 키로 닫을 수 없는 경우, 명확한 닫기 버튼이 필요합니다

**필수 속성:**

- `aria-labelledby`: 제목 요소의 ID와 연결
- `aria-describedby`: 설명 요소의 ID와 연결

**[추가] 사용 예시:**

```html
<div role="alertdialog" aria-labelledby="dialog-title" aria-describedby="dialog-desc">
  <h2 id="dialog-title">삭제 확인</h2>
  <p id="dialog-desc">정말로 이 항목을 삭제하시겠습니까? 이 작업은 되돌릴 수 없습니다.</p>
  <button>삭제</button>
  <button>취소</button>
</div>
```

### 3. aria-role="dialog"

[MDN Reference](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/dialog_role)

대화상자로 사용자가 정보를 입력하거나 응답할 수 있도록 대화형 콘텐츠를 포함합니다.

**필수 설정:**

- `aria-modal="true"`: 모달임을 명시
- 모달 외부의 모든 콘텐츠에 `aria-hidden="true"` 적용
- **[추가]** 초점 트래핑(focus trapping) 구현 필요

**주의사항:**

- 모달은 `aria-hidden="true"` 속성을 가지는 요소의 자식이 되어서는 안 됩니다
- **[추가]** 모달이 열릴 때 초점은 모달 내부의 첫 번째 대화형 요소나 모달 자체로 이동해야 합니다

### 4. aria-role="combobox"

현재 요소에서 값을 선택할 수 있도록 옵션 목록 팝업이 제공됨을 나타냅니다.

**필수 속성:**

- `aria-expanded`: 옵션 목록 노출 여부
- `aria-haspopup`: 노출되는 팝업 종류 (listbox, tree, grid, dialog)
- `aria-activedescendant`: 선택된 옵션의 ID (초점이 combobox에 남아있는 경우)

**구현 가이드:**

- 사용자 입력 불가능 + 단순 선택: `<button>` 태그 사용
- 사용자 입력 가능: `<input>` 태그 사용

**[추가] 사용 예시:**

```html
<input type="text" 
       role="combobox" 
       aria-expanded="false" 
       aria-haspopup="listbox"
       aria-activedescendant="option-1">
<ul role="listbox">
  <li role="option" id="option-1">옵션 1</li>
  <li role="option" id="option-2">옵션 2</li>
</ul>
```

### 5. aria-role="listbox"

여러 개의 정적인 항목을 선택할 수 있는 목록을 나타냅니다.

**레이블 설정:**

- ID와 `aria-labelledby` 연결 또는
- `aria-label` 직접 사용

**선택적 속성:**

- `aria-selected`: 선택 여부
- `aria-orientation`: 방향 (vertical/horizontal)
- `aria-required`: 필수 선택
- `aria-readonly`: 읽기 전용
- `aria-multiselectable`: 다중 선택 가능

**초점 처리:**

- `tabindex` 설정 필요
- `aria-activedescendant`와 ID 연결

### 6. aria-role="option"

listbox의 개별 옵션을 나타냅니다. HTML `<option>`과 달리 이미지 등 다양한 콘텐츠를 포함할 수 있습니다.

**필수 속성:**

- `aria-selected`: 선택 상태
- `aria-disabled`: 비활성화 상태

**[추가]** 부모 요소가 `aria-multiselectable="true"`인 경우, 여러 옵션이 동시에 `aria-selected="true"`를 가질 수 있습니다.

### 7. aria-role="menu" / "menubar"

**menu**: 여러 항목의 목록을 나타냄 (드롭다운 메뉴) **menubar**: 계속 보이는 형태의 메뉴 (상단 메뉴바)

**하위 역할:**

- `menuitem`: 기본 메뉴 항목
- `menuitemcheckbox`: 체크 가능한 항목
- `menuitemradio`: 라디오 선택 항목

**네비게이션 메뉴의 경우:**

- `role="navigation"` 사용
- 현재 페이지: `aria-current="page"`

**[추가] 키보드 네비게이션:**

- 화살표 키로 항목 간 이동
- Enter/Space로 선택
- ESC로 메뉴 닫기

### 8. aria-role="menuitem" / "menuitemcheckbox" / "menuitemradio"

**menuitem**: 기본 메뉴 항목

- 하위 메뉴가 있는 경우: `aria-haspopup="menu"` 또는 `"true"`

**menuitemcheckbox**: 체크 가능한 메뉴 항목

- `aria-checked` 속성 필수
- group 역할로 그룹화 가능

**menuitemradio**: 라디오 형태의 메뉴 항목

- `aria-checked` 속성 필수
- 한 그룹 내에서 하나만 선택 가능

**초점 처리:**

- 선택된 항목: `tabindex="0"`
- 나머지 항목: `tabindex="-1"`

### 9. aria-role="presentation"

요소의 의미를 제거하고 내용만 전달합니다. `aria-hidden`과 달리 내용은 스크린 리더를 통해 전달됩니다.

**주의사항:**

- 초점 가능한 요소(a, button, input 등)에는 적용되지 않습니다
- `tabindex` 속성이 있는 요소에도 무시됩니다

**[추가] 사용 예시:**

```html
<!-- 레이아웃용 테이블의 의미 제거 -->
<table role="presentation">
  <tr>
    <td>내용은 읽히지만 테이블 구조는 무시됩니다</td>
  </tr>
</table>
```

### 10. aria-role="region"

페이지 내 중요한 섹션 영역을 나타냅니다. HTML5의 `<section>`과 동일한 의미를 가집니다.

**[추가]** 반드시 `aria-label` 또는 `aria-labelledby`로 영역의 목적을 명시해야 합니다.

### 11. aria-role="slider"

`<input type="range">`와 유사한 UI를 제공합니다.

**필수 속성:**

- `aria-valuemin`: 최솟값
- `aria-valuemax`: 최댓값
- `aria-valuenow` 또는 `aria-valuetext`: 현재 값

**선택적 속성:**

- `aria-orientation`: 방향

**초점 처리:**

- `tabindex` 설정 필수

**[추가] 키보드 조작:**

- 화살표 키로 값 증감
- Home/End 키로 최소/최대값 이동

### 12. aria-role="spinbutton"

`<input type="number">`와 유사한 UI를 제공합니다.

**필수 속성:**

- `aria-valuemin`: 최솟값
- `aria-valuemax`: 최댓값
- `aria-valuenow` 또는 `aria-valuetext`: 현재 값

**구현 팁:**

- 키보드로 값 조절이 가능하므로 별도의 증감 버튼은 `tabindex="-1"` 설정
- 증감 버튼에는 `aria-hidden="true"` 추가 권장

**[추가] 사용 예시:**

```html
<div role="spinbutton" 
     aria-valuemin="0" 
     aria-valuemax="100" 
     aria-valuenow="50"
     tabindex="0">
  50
</div>
```

### 13. aria-role="switch"

**[추가]** 토글 스위치를 나타냅니다. checkbox와 유사하지만 on/off 상태를 더 명확하게 표현합니다.

**필수 속성:**

- `aria-checked`: "true" 또는 "false"

**사용 예시:**

```html
<button role="switch" aria-checked="false" aria-label="알림 설정">
  <span>OFF</span>
</button>
```

### 14. aria-role="tab" / "tablist" / "tabpanel"

**[추가]** 탭 인터페이스를 구성하는 역할들입니다.

**구조:**

```html
<div role="tablist" aria-label="상품 정보">
  <button role="tab" aria-selected="true" aria-controls="panel1">설명</button>
  <button role="tab" aria-selected="false" aria-controls="panel2">리뷰</button>
</div>
<div role="tabpanel" id="panel1" aria-labelledby="tab1">
  <!-- 탭 내용 -->
</div>
```

**필수 구현:**

- 선택된 탭: `aria-selected="true"`
- 탭과 패널 연결: `aria-controls`와 ID
- 키보드 네비게이션: 화살표 키로 탭 이동

### 15. aria-role="timer"

**[추가]** 시간이 지남에 따라 업데이트되는 숫자 카운터를 나타냅니다.

**사용 예시:**

```html
<div role="timer" aria-label="남은 시간" aria-live="polite">
  05:23
</div>
```

## 자주 사용하는 ARIA 상태/속성 (States & Properties)

### aria-activedescendant

활성화된 하위 항목을 스크린 리더에 전달합니다. 초점은 컨테이너에 유지하면서 시각적으로만 하위 항목을 강조할 때 사용합니다.

**[추가] 사용 조건:**

- 컨테이너가 초점을 받을 수 있어야 함
- 값은 하위 요소의 ID여야 함

### aria-atomic

`aria-live` 영역에서 변경 시 일부만 읽을지 전체를 읽을지 결정합니다.

- `true`: 전체 내용 읽기
- `false`: 변경된 부분만 읽기 (기본값)

### aria-autocomplete

자동완성 동작 방식을 나타냅니다.

- `none`: 자동완성 없음
- `inline`: 입력 필드 내 자동완성
- `list`: 목록으로 제안
- `both`: inline + list

### aria-checked

체크박스나 라디오 버튼의 선택 상태를 나타냅니다.

- `false`: 선택 안 됨
- `true`: 선택됨
- `mixed`: 부분 선택 (체크박스만)

### aria-controls

**[추가]** 현재 요소가 제어하는 다른 요소를 나타냅니다. 제어 대상 요소의 ID를 값으로 가집니다.

```html
<button aria-controls="panel1" aria-expanded="false">패널 열기</button>
<div id="panel1" hidden>패널 내용</div>
```

### aria-current

그룹 내에서 현재 항목을 나타냅니다.

- `page`: 현재 페이지
- `step`: 현재 단계
- `location`: 현재 위치
- `date`: 현재 날짜
- `time`: 현재 시간
- `true/false`: 범용

**[추가]** `aria-selected`보다 우선순위가 높으므로, 네비게이션에는 `aria-current`, 선택 목록에는 `aria-selected`를 사용합니다.

### aria-describedby / aria-labelledby

- **aria-labelledby**: 간결한 제목/레이블 연결
- **aria-describedby**: 상세한 설명 연결

**[추가] 차이점:**

```html
<input aria-labelledby="label1" aria-describedby="help1">
<label id="label1">이메일</label>
<span id="help1">example@domain.com 형식으로 입력하세요</span>
```

### aria-disabled

의미적으로 비활성화 상태를 나타냅니다. 실제 기능 비활성화는 HTML의 `disabled` 속성을 사용하세요.

**[추가]** `aria-disabled="true"`는 폼 제출 시 값이 전송되지만, `disabled` 속성은 전송되지 않습니다.

### aria-expanded

확장/축소 가능한 요소의 상태를 나타냅니다.

- 반드시 `aria-controls`와 함께 사용
- **[추가]** 토글 버튼에 주로 사용됨

### aria-haspopup

트리거 요소가 팝업을 열 수 있음을 나타냅니다.

- `menu`, `listbox`, `tree`, `grid`, `dialog`
- `true` = `menu`와 동일

### aria-hidden

보조 기술에서 요소를 숨깁니다.

- 하위에 초점 가능한 요소가 있으면 안 됨
- **[추가]** 장식적 아이콘이나 중복 콘텐츠에 사용

### aria-label

요소에 접근 가능한 이름을 직접 제공합니다.

- **[추가]** 시각적 레이블이 없는 경우 사용
- 시각적 레이블이 있다면 `aria-labelledby` 우선 사용

### aria-level

계층 구조에서의 수준을 나타냅니다.

- **[추가]** 주로 제목(heading)이나 트리 구조에서 사용
- 값은 1부터 시작하는 정수

### aria-live

동적 콘텐츠 변경을 알립니다.

- `polite`: 현재 읽기 완료 후 알림
- `assertive`: 즉시 중단하고 알림
- `off`: 알리지 않음 (기본값)

**[추가] 사용 시 주의:**

- 너무 자주 변경되는 영역에는 사용 자제
- 중요도에 따라 적절한 수준 선택

### aria-modal

모달 대화상자임을 나타냅니다.

- `true`: 모달 외부 상호작용 불가
- **[추가]** 반드시 초점 트래핑과 함께 구현

### aria-multiselectable

다중 선택 가능함을 나타냅니다.

- listbox, tablist 역할과 함께 사용
- **[추가]** 각 옵션은 `aria-selected` 상태 필요

### aria-orientation

요소의 방향을 나타냅니다.

- `horizontal`: 가로 방향
- `vertical`: 세로 방향 (기본값)

### aria-pressed

토글 버튼의 눌림 상태를 나타냅니다.

- `true`: 눌림
- `false`: 안 눌림
- `mixed`: 부분적 눌림

**[추가] 주의사항:**

- 텍스트 레이블을 동적으로 변경하지 마세요
- 상태는 `aria-pressed`로만 전달됩니다

### aria-readonly

읽기 전용 상태를 나타냅니다.

- **[추가]** `disabled`와 달리 초점은 받을 수 있음

### aria-selected

선택 가능한 요소의 선택 상태를 나타냅니다.

- option, tab 역할과 필수 사용
- **[추가]** row, gridcell 등과 선택적 사용

### aria-required

필수 입력 필드임을 나타냅니다.

- **[추가]** HTML5의 `required` 속성과 동일한 의미

### aria-valuemax / aria-valuemin / aria-valuenow / aria-valuetext

범위를 가진 요소의 값을 나타냅니다.

- `aria-valuemin`: 최솟값
- `aria-valuemax`: 최댓값
- `aria-valuenow`: 현재 숫자 값
- `aria-valuetext`: 현재 텍스트 값

**[추가] 사용 예시:**

```html
<div role="slider" 
     aria-valuemin="0" 
     aria-valuemax="100" 
     aria-valuenow="75"
     aria-valuetext="75%">
</div>
```

## [추가] 실전 활용 팁

### 1. 폼 검증 메시지

```html
<input type="email" aria-describedby="email-error" aria-invalid="true">
<span id="email-error" role="alert">올바른 이메일 형식이 아닙니다.</span>
```

### 2. 로딩 상태

```html
<button aria-busy="true" aria-label="저장 중...">
  <span aria-hidden="true">⏳</span> 저장 중...
</button>
```

### 3. 동적 콘텐츠 업데이트

```html
<div aria-live="polite" aria-atomic="true">
  <p>장바구니에 3개 항목이 있습니다.</p>
</div>
```

## [추가] 마무리

WAI-ARIA는 강력한 도구이지만, 잘못 사용하면 오히려 접근성을 해칠 수 있습니다. 항상 "No ARIA is better than bad ARIA"라는 원칙을 기억하고, 기본 HTML 요소를 우선적으로 사용하세요. ARIA는 정말 필요한 경우에만, 그리고 올바르게 사용해야 합니다.

접근성은 단순히 규정 준수가 아닌, 모든 사용자를 위한 더 나은 웹 경험을 만드는 것입니다. 이 가이드가 여러분의 웹 접근성 향상에 도움이 되기를 바랍니다.