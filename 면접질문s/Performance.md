**목차**
- [리액트 라이프사이클에 대해](#리액트-라이프사이클에-대해)
- [useMemo 는 어떤 hooks이고, 언제 사용하나요?](#useMemo는-어떤-hooks이고,-언제-사용하나요?)

<br/>
<br/>

## 웹페이지 성능 최적화를 한다면 어떻게 할 건가요
-----
일단 프론트에서 웹서비스에서 성능 측정의 구분을 크게 2개로 봐야합니다.

- 로딩 성능
	- 서버에 있는 웹 리소스를 다운로드 할 때의 성능을 말합니다.
		- 리로스 크기, 개수 최적화
		- 코드 분할
		- 리소스 우선순위
- 랜더링 성능
	- 다운로드한 리소스를 가지고 화면을 그릴 때의 성능을 말합니다.
		- 브라우저 동작원리
		- 프레임워크 라이프 사이클

만약 웹서비스 전체로 파악한다면 더 다양한 영역들이 있습니다. 너무 깊게 가진않고 나열만 해볼게요.

<br/>
<br/>

#### TTFB, FCP, SI, LCP,  FMP, TTI, TBT, CLS는 무엇인가요
------
여러 요소로 웹페이지 성능 측정을 하는데요. 각 위치마다 최적화를 위한 방법들이 다릅니다. 하지만 모든걸 할 수 없다면 전체 Perfomance에서 미치는 각각의 가중치들이 있습니다. 이를 우선순위로 삼아서 접근하는 것도 방법일 것 같습니다.

가중치가 가장 높은 3가지는 LCP, TBT, CLS 입니다.

![](./resource/web_page_performance.png)

TTFB
- 서버에서 바이트가 도착하는 시점을 나타낸다.
- 서버 성능과 관련있다.
<br/>

FCP
- 페이지가 로드 될때 브라우저가 DOM 콘텐츠의 첫 번째 부분을 렌더링 하는데 걸리는 <U>시간</U>
- 10% 가중치
- 최적화 방법
	- 렌더링을 block 하는 리소스 제거
	- CSS minify
	- 사용하지 않는 CSS 제거
	- 요청할 origin에 preconnect : `<link rel="preconnect">`
	- 서버 응답 시간 (TTFB) 줄이기
	- 여러 페이지 리다이렉트 줄이기
	- 키 요청 preload : `<link rel="preload">`
	- 지나치게 큰 네트워크 payload를 피하기
	- 효율적인 캐시 정책으로 정적 자산 제공
	- 과도한 DOM size를 피하기
	- 중요한 요청의 depth를 최소화
	- 웹 폰트 로드 중에 텍스트가 계속 보이도록 함
	- 요청 수를 줄이고 전송 크기를 작게 하기
<br/>

SI
- 페이지 로드 중 콘텐츠가 시각적으로 표시되는 **<u>속도</U>**
- 10% 가중치
<br/>

FMP
- 페이지의 주요 컨텐츠들을 화면에 렌더링하기 시작하는 <U>순간</U>
<br/>

⭐️⭐️ LCP
- 페이지가 로드될 때 화면내에 가장 큰 이미지나 텍스트 요소가 렌더링 되기까지 걸리는 <U>시간</U>
- 25% 가중치
- 최적화 방법
	- 느린 서버 응답 시간 해결
	    - 서버 최적화
	    - 가까운 CDN으로 라우팅
	    - asset 캐싱
	    - cache-first HTML 페이지 서빙
	    - 서드파티 origin 리소스 preconnect
	- 렌더링을 block 하는 JS 및 CSS 해결
	    - CSS block 시간 단축
	    - CSS minify
	    - 중요하지 않은 CSS는 defer로 load
	    - 중요 CSS는 inline 으로 load
	- Javascript block 시간 단축
	    - JS 파일 minify 및 압축
	- 느린 리소스 (img, svg, video, ...) 로드 시간 해결
	    - 이미지 최적화 및 압축
	    - 중요한 리소스 preload
	    - 텍스트 파일 압축
	    - 적응형 리소스 서빙
	        - 예: 네트워크가 느린 경우 video 대신 image 서빙
	    - 서비스 워커를 통한 자산 캐싱
	- 클라이언트 사이드 렌더링의 경우
	    - 중요한 자바스크립트 최소화
	    - 자바스크립트 minify
	    - 사용하지 않는 자바스크립트 defer로 load
	    - 사용하지 않는 polyfill 최소화
	- 서버사이드 렌더링 사용
	        - `TTFB` 증가, `TTI` 증가를 고려해야 함
	- pre-rendering 사용
	        - `TTI`는 증가할 수 있으나 `TTFB`는 `SSR`보다 나음

TTI
- 사용자가 페이지와 상호작용이 가능한 시점까지 걸리는 <U>시간</U>
- 10% 가중치
- 최적화 방법
	- JS minify
	- 요청할 origin에 preconnect : `<link rel="preconnect">`
	- 키 요청 preload : `<link rel="preload">`
	- 타사 코드의 영향 범위 감소
	- 중요한 요청의 depth를 최소화
	- JS 실행시간 단축
	- 메인 스레드 작업 최소화
	- 요청 수를 줄이고 전송 크기를 작게 유지

⭐️⭐️ TBT
- FCP - TTI 시간동안, 페이지가 클릭, 키보드 입력 등의 사용자 입력에 응답하지 않도록 차단된 총 <U>시간</U>
- 30% 가중치
- 최적화 방법
	- 타사 코드의 영향 범위 감소
	- JS 실행시간 단축
	- 메인 스레드 작업 최소화
	- 요청 수를 줄이고 전송 크기를 작게 유지
<br/>

⭐️⭐️ CLS
- 페이지가 로드과정에서 발생하는 예기치 못한 모든 <U>레이아웃 이동</U>의 누적점수를 측정
- 레이아웃 이동 == 화면상에서 요소의 위치나 크기가 순간적으로 변하는 현상
- 15% 가중치
- 최적화 방법
	- JS minify
	- 요청할 origin에 preconnect : `<link rel="preconnect">`
	- 키 요청 preload : `<link rel="preload">`
	- 타사 코드의 영향 범위 감소
	- 중요한 요청의 depth를 최소화
	- JS 실행시간 단축
	- 메인 스레드 작업 최소화
	- 요청 수를 줄이고 전송 크기를 작게 유지
<br/>

⭐️⭐️ FID
- 사용자가 페이지와 처음 상호작용한 시간부터 브라우저가 실제로 이벤트 핸들러 처리를 시작할 수있는 시간까지의 <U>시간</U>
- 최적화 방법
	- 긴 Tasks 분할하기
	    - 메인 스레드를 50ms 이상 차단하는 긴 Task를 분할하기 (`TBT` 개선)
	    - 코드 스플리팅
	- 상호작용 준비를 위한 페이지 최적화
	    - JS 코드 및 기능을 점진적으로 load
	    - SSR의 경우 논리를 서버측으로 이동하고 코드 스플리팅을 고려해야 함
	    - cascading data fetch를 최소화
	    - 서드파티 코드 로딩시간 고려해야 함
	- web worker 사용하기
	    - 백그라운드 스레드에서 자바스크립트를 실행할 수 있음
	- JS 실행시간 단축
	    - 사용하지 않는 자바스크립트 defer로 load
	        - 특별한 이유가 없는 한 서드파티 스크립트는 defer 또는 async로 로드되어야 함
	    - 사용하지 않는 polyfill 최소화
	        - module / nomodule 패턴을 활용하여 개별 번들 제공

<details>
<summary><b>참고자료</b></summary>
	<a href="https://velog.io/@yrnana/%EC%9B%B9%EC%82%AC%EC%9D%B4%ED%8A%B8-%EC%84%B1%EB%8A%A5-%EB%A9%94%ED%8A%B8%EB%A6%AD#tbt-total-blocking-time">
		# 웹성능별 영역 그리고 각각의 최적화 방안
	</a>
</details>



## 브라우저
우선 브라우저 구조는 아래와 같다.
![[browser_structure.png]]

- User Interface
==========
- Browser Engine  <-->  Data Persistence
==========
- Redering Engine
	- Webkit (크롬(iOS), 사파리)
		- 기반 - Blink (크롬, 오페라)
	- Gecko (파이어폭스)
	- Trident (IE)
==========
	- JavaScript Interperter
	- Networking
	- UI Beckend


https://another-light.tistory.com/41


## 브라우저 Rendering Engine
------
렌더링 엔진이 하는 일은 '화면에 요청된 콘텐츠를 표시하는 것' 이다. 이때 화면을 그리기 위해 아래와 같은 과정을 거친다.

> Parsing -> Render tree -> Layout -> Painting

사실 대표적인 엔진인 Webkit 과 Gecko 의 렌더링 흐름은 더 많은 세부과정이 있고, 각 엔진마다 약간의 차이가 있지만 큰틀에서는 위 4단계를 거친다고 해도 틀린 말이 아니다.
![[Pasted image 20231225211405.png]]
![[Pasted image 20231225211418.png]]


먼저
#### 1. Parsing
'문서를 파싱한다'는 것은 코드를 사용할 수 있는 구조로 변환하는 것을 의미한다.

파싱
- 어휘(Lexixal) 분석 - 입력을 유효한 토큰으로 분할하는 과정
	- CSS, JavaScript 등의 코드가 분석되어 토큰으로 변환된다.
- 구문(Syntax) 분석 - 문법 규칙을 적용하는 과정

이 파싱이라는 단계는 2가지 작업이 이뤄진다.
- HTML 파싱
- CSS 파싱

###### HTML 파싱---------
HTML파서는 HTML 마크업을 파싱 트리로 변환한다. 이는 일반적인 파싱 기법을 사용할 수 없다. 다 알 필요는 없을 ![[Pasted image 20231225211126.png]]

- 토큰화
	- HTML 마크업 코드를 특정 알고리즘에 의해 토큰화
- 트리 생성
	- Document, 요소 node 객체 생성
	- 각 요소들의 관계를 Tree로 구축

두 단계로 나뉜다.

이때, HTML 문서는 원활한 파싱 작업을 위해 잘못된 HTML 에 대해서도 오류를 특정 범위를 허용한다.


###### CSS 파싱---------
HTML과 달리 CSS는 일반적인 파싱 기법으로 파싱이 가능하다. 더 정확히는 '문맥 자유 언어' 와 같은 개념이 있지만, 더 파고들진 않는다.

CSS 파일을 StyleSheet 객체로 취급되어 파싱된다. 각 StyleSheet 객체들은 CSS 규칙에 의해 나뉩니다.
![[Pasted image 20231225211030.png]]


#### 2. Render tree
앞서 살펴본 Parsing 단계에서 생성된 DOM 트리와 StyleSheet 객체의 (CSSOM) 트리를 합치고 렌더(Render) 트리를 구축하는 작업을 진행합니다.

이 과정의 대표적인 작업들은

- 렌더기 생성
- 스타일 속성 계산
- 스타일 데이터 공유
- 규칙 적용

이 있다.


##### 렌더기 생성---------
RenderObject 라는 렌더기가 생성된다. CSS2 기준으로 렌더기의 모양은 아래와 같다고 한다.
이후 '레이아웃', '페인팅' 과정에서 이 RenderObject 를 통해서 작업을 수행한다.
```javascript
class RenderObject {  
	virtual void layout();  
	virtual void paint(PaintInfo);  
	virtual void rect repaintRect();  
	Node* node;  //the DOM node  
	RenderStyle* style;  // the computed style  
	RenderLayer* containgLayer; //the containing z-index layer
}
```


###### 스타일 속성 계산---------
렌더링 트리를 빌드하려면 각 렌더링 객체의 시각적 속성을 계산해야 한다. 스타일 계산은 매우 무거운 작업이다.

스타일 계산은 아래와 같은 단계를 거친다.

1. **스타일 시트 적용**: 브라우저는 모든 CSS 스타일 시트를 읽는다.
2. **선택자 일치**: 각 HTML 요소에 대해, 브라우저는 해당 요소에 적용할 수 있는 모든 CSS 규칙을 찾는다. 이 과정에서 CSS 선택자가 사용된다.
3. **스타일 규칙의 적용**: 일치하는 모든 스타일 규칙을 찾은 후, 브라우저는 이러한 규칙을 합쳐서 최종 스타일을 결정된다.
	1. **종속 스타일 계산**: 특정 스타일은 다른 요소의 스타일에 종속될 수 있다. 이러한 종속 스타일은 실제 값으로 계산되어야 합니다.
4. **최종 스타일 결정**: 모든 규칙과 계산이 완료되면, 각 HTML 요소에 최종 스타일이 적용된다.

###### 스타일 데이터 공유---------
- 스타일 규칙, 속성의 상하관계를 구조화 하여, 스타일 객체 재사용
- 캐싱 매커니즘 활용

###### 규칙 적용---------
- 규칙을 조작하여 좀 더 쉽게 일치를 시킴
- 규칙 적용에 대한 우선순위 재정렬

###### 이때 개발자가 해야할 최적화 작업 
그래서, 내가 이 단계의 최적화를 위해 고려해야할 점들은?

- 클래스 기반 스타일 정의 (재사용 증가)
- CSS 전처리기 사용권장 (재사용 증가)
- 복잡한 선택자 피하기
- 선택자 중첩 피하기
- 빌드 도구를 활용하여 CSS 파일 크기 최소화

를 해야한다.

#### Layout
레이아웃 단계에서는 각 요소의 정확한 크기와 위치를 계산한다. 이러한 값을 계산하는 것을 레이아웃 또는 리플로우라고 한다.

레이아웃 계산은 2가지로 나뉠수 있다.
- 전역적 레이아웃
	- 화면 전체의 레이아웃 계산
	- 폰트, 뷰포트 사이즈
- 증분적 레이아웃
	- dirty-bit 를 활용
	- 레이아웃 변경이 발생해야하는 요소들을 비동기로 일괄작업(batch)


배치 과정은 아래와 같다.
- 상위 레이아웃에서 자신의 너비 계산
- 하위 요소의 x, y 좌표를 구해서 위치 계산. 하위 요소가 dirty-bit(레이아웃 작업이 필요하다는 상태값) 이라면, 하위 요소의 layout() 메서드가 호출된다.
- 상위 레이아웃은 하위 요소의 높이 + 자신의 패딩, 마진등을 총 계산하여 높이를 구함
- 상위 레이아웃의 dirty-bit 제거 (작업이 끝났음을 의미)

시스템 상으로는 각 요소들의 RenderObject 안에서 layout() 메서드가 호출된다.

만약 요소의 크기와 위치가 바뀌면, 해당 요소와 함께 하위 요소들 까지 다시 레이아웃 단계를 거친다.



#### Painting
배치 작업이 끝나면, 페인팅 단계가 수행된다. Redner Tree 를 순회하면서 각각의 요소의 RenderObject 의 paint()가 호출 되어 화면에 콘텐츠를 표시한다.

페인팅의 순서는 정해져 있는데, 하나의 요소를 그린다고 했을 때,
1. background color
2. 배경 이미지
3. border
4. children
5. outline


솔직히... 자료 내용이 정리가 제대로 안되어있고, 글 내용만 더럽게 많다. 다 이해하고 알려고 하기보다 시간 내서 다시한번 내게 필요한 내용으로 간추리는게! 내게 도움이 될 것 같다.
https://another-light.tistory.com/46?category=844048
https://web.dev/articles/howbrowserswork?hl=ko#Layout

이 자료가 쉽게 정리가 잘 되어 있는데, 이걸 보고 다시 정리를 하면 좋을것 같다.
https://yozm.wishket.com/magazine/detail/1338/


## 렌더 트리와 레이아웃 단계가 나뉜 이유
--------
왜 브라우저는 'render tree'단계에서 요소의 크기와 위치를 계산하지 않고, 'layout' 단계에서 크기나 위치 계싼을 하는거야? 'render tree' 단계에서도 스타일링 계산을 한다며? 그때 같이 하면 안되는거야?

GPT의 훌륭한 대답
![[Pasted image 20231225230524.png]]





## Layout / paint 줄이기
-----
Gecko 브라우저 에서는 Reflow, Repaint 라고 표현한다.



https://boxfoxs.tistory.com/408
