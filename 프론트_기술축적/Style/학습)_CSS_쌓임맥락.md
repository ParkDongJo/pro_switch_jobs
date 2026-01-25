# 개념 (stacking context)
**쌓임 맥락**(stacking context)은 가상의 Z축을 사용한 HTML 요소의 3차원 개념, 쉽게말해  z축으로 요소의 관계를 다룰 수 있도록 _쌓임 맥락_이라는 개념을 제공

==쌓임 맥락이 생기는 상황==
- 루트 요소인 `<html>`
- `position` 값이 `absolute` 또는 `relative`이고, `z-index` 값이 정의된 경우
- `position` 값이 `fixed` 또는 `sticky`
- `display` 값이 `flex` 또는 `grid`인 요소의 자식 요소이고, `z-index` 값이 정의된 경우
- `opacity`가 1보다 작은 경우 (자리를 차지하면서, 투명도)
- `will-change` 속성이 정의된 요소
- **다음 속성들 중 하나라도 값이** **none****이 아닌 요소**
    - transform
    - scale
    - rotate
    - translate
    - filter
    - backdrop-filter
    - perspective
    - clip-path
    - mask / mask-image / mask-border
- isolation 값이 isolate인 요소
- contain 값이 layout 또는 paint인 요소, 또는 이 둘을 포함하는 복합 값인 요소
- opacity 같은 **스태킹 컨텍스트 생성 속성**을 @keyframes로 애니메이션했고, animation-fill-mode: forwards가 설정된 요소

==결론==
쌓임 맥락의 우선순위를 결정 짓는 요소는
1. 부모가 누구냐?
2. 쌓임 맥락을 일으키는 요소가 있냐 (위에 나열한 요소들)
3. z-index 의 숫치는 몇이냐?

```
- 루트
    - DIV #1
    - DIV #2
    - DIV #3
        - DIV #4
        - DIV #5
        - DIV #6
```

쌓임맥락 적용후
```
- 루트
    - DIV #2: (z-index: 2)
    - DIV #3: (z-index: 4)
        - DIV #5: (z-index: 1) -> 렌더링 순서가 4.1이 된다.
        - DIV #6: (z-index: 3) -> 렌더링 순서가 4.3이 된다.
        - DIV #4: (z-index: 6) -> 렌더링 순서가 4.6이 된다.
    - DIV #1: (z-index: 5)
```


일반적인 순서를 정리해보자면

루트요소 (html) > positioned 된 음의 z-index > block-level > float 된 요소들 > inline-level > positioned z-index : 0 또는 auto > positioned z-index 0이상

![[260116011855.png]]


# 쌓임 우선순위 == 버저닝 규칙

`z-index` 값에 따른 레이어 배치를 판단할 때, 버전 규칙을 대입할 수 있다는 내용

> An easy way to figure out the rendering order of stacked elements along the Z axis is to think of it as a "version number" of sorts, where child elements are minor version numbers underneath their parent's major version numbers.

— [HTML 요소 간의 z-index 우선 순위를 판단하는 방법, MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context#the_example)

| z-index                       | 버전     |
| ----------------------------- | ------ |
| 요소 A<br>- `z-index: 1`        | 1.0.0  |
| A의 자식 요소 B<br>- `z-index: 10` | 1.10.0 |
| 요소 C<br>`z-index: 2`          | 2.0.0  |
| C의 자식 D <br>- `z-index: 1`    | 2.1.0  |

버전 간의 비교에서 메이저 버전 값이 낮은 경우 마이너 버전 값이 아무리 높아도 버전의 크기는 역전되지 않습니다. 마찬가지로 `z-index`에서도 동일한 기준이 적용되어 부모 요소의 값에 따라 결정되기 때문에 위의 테이블에서 B 요소는 D 요소보다 위에 배치될 수 없습니다.




# 브라우저 렌더링 Composite 와 연관성

1. **Layout**: 박스 위치/크기 계산
2. **Paint**: “무엇을 어떤 순서로 그릴지”를 비트맵/디스플레이 리스트로 기록
3. **Composite**: 여러 레이어(타일)를 GPU 등으로 **마지막에 합성(병합)**해서 화면에 표시

여기서 **stacking context는 주로 2) Paint의 “그리는 순서 규칙”**에 해당한다.

- 브라우저는 성능 때문에 어떤 요소들을 **별도의 컴포지팅 레이어**로 올려서(예: transform 애니메이션) 나중에 합성만 하기도 한다.
- - **스태킹 컨텍스트는 ‘그림의 순서(페인팅 순서)’**, 컴포지팅 레이어는 **‘어떻게 합성할지(레이어 분리/병합 전략)’**라서 목적이 다르다

==결론==
- transform
- filter
- will-change
같은 요소들은 엔진에서 쌓임맥락을 만듦과 동시에, Composit(합성) 레이어 생성의 “강한 후보/트리거”** 가 된다.

하지만, 쌓임맥락을 일으키는 모든 요소가, Composit(합성) 레이어 생성에 영향을 주진 않는다.




## 참고자료
---
https://velog.io/@ddosang/Front-end-9

https://csstriggers.com/

https://so-so.dev/web/browser-rendering-process/

https://coyo-hm.github.io/post/browser-rendering

https://developer.mozilla.org/ko/docs/Web/CSS/CSS_positioned_layout/Stacking_context#%EC%8C%93%EC%9E%84_%EB%A7%A5%EB%9D%BD

https://www.yongdam.sh/blog/css-stacking-context

https://binyard.me/javascript/basic/browser/js003

https://fantasmith.com/frontEnd/fundamental/css/z-index_and_stack_context/

