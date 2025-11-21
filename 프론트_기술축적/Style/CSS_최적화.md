> Reder Tree -> Layout -> Paint -> Composit
> --------------Reflow -------------------->
> ---------                         Repaint ---------->


렌더링 성능의 핵심은
- Reflow 를 줄이거나
- Repaint 를 줄이거나

Reflow, Repaint 를 줄이는 방법
- GPUC 가속화
- 인라인 CSS 줄이기
- contain 으로 영향 법위 제한
- 이미지의 크기를 명시 (width, height, aspect-ratio)
- table 태그는 레이아웃으로 사용하지 않기
- JS를 통한 변화는 한꺼번에 적용되도록
- CSS 의 하위 선택자를 최소화 하기
- 유틸화 하여, 유틸간 조합으로 CSS 재사용하기 


CSS 생산성을 높이는 방법
- 토큰화
	- 원시적 값으로 토큰화 하기
	- 의미나 역할로 토큰화 하기
	- 컴포넌트에 맞게 구체적으로 토큰화 하기
- @layer 를 활용하여 레이어 규칙화 하고, 영향범위를 관리하기

