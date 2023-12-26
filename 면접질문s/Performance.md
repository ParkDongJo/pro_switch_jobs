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

> Parsing -> Render tree -> Layout -> Painting -> Composite

사실 대표적인 엔진인 Webkit 과 Gecko 의 렌더링 흐름은 더 많은 세부과정이 있고, 각 엔진마다 약간의 차이가 있지만 큰틀에서는 위 4단계를 거친다고 해도 틀린 말이 아니다.
![[browser_redering_engine_flow_webkit.png]]
![[browser_redering_engine_flow_gecko.png]]


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
![[browser_cssom.png]]


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
레이아웃 단계에서는 브라우저의 뷰포트 내에서 보여지는 요소들의 정확한 크기와 위치를 계산한다. 이러한 값을 계산하는 것을 레이아웃 또는 리플로우라고 한다.
Layout 단계를 통해 %, vh, vw와 같이 상대적인 위치, 크기 속성은 실제 화면에 그려지는 pixel단위로 변환된다.

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


#### Composite
페인트 시 효율을 위해 여러 레이어로 나눠서 페인트 작업을 한다. 그리고 이렇게 작업한 여러 레이어를 합치는 단계가 있는데, 그게 바로 컴포지트 단계 이다.



솔직히... 자료 내용이 정리가 제대로 안되어있고, 글 내용만 더럽게 많다. 다 이해하고 알려고 하기보다 시간 내서 다시한번 내게 필요한 내용으로 간추리는게! 내게 도움이 될 것 같다.
https://another-light.tistory.com/46?category=844048
https://web.dev/articles/howbrowserswork?hl=ko#Layout

이 자료가 쉽게 정리가 잘 되어 있는데, 이걸 보고 다시 정리를 하면 좋을것 같다.
https://yozm.wishket.com/magazine/detail/1338/


## 렌더 트리와 레이아웃 단계가 나뉜 이유
--------
왜 브라우저는 'render tree'단계에서 요소의 크기와 위치를 계산하지 않고, 'layout' 단계에서 크기나 위치 계싼을 하는거야? 'render tree' 단계에서도 스타일링 계산을 한다며? 그때 같이 하면 안되는거야?

GPT의 훌륭한 대답
![[browser_layout.png]]





## Reflow / RePaint 줄이기
-----
브라우저 마다 용어가 약간의 차이가 있지만, 우선은 용어는 웹킷 브라우저 기준으로 한다. 위의 브라우저 동작을 통해 페이지가 최종적으로 그려졌다고 해서, 모든 과정이 끝난 것이 아니다.

- Reflow
	- 일부 요소들이 Layout 까지의 과정을 다시 거치는 현상
	- 모든 과정을 모드 거친다.
- RePaint
	- 일부 요소들이 Paint 까지의 과정을 다시 거치는 현상
	- 레이아웃 단계는 건너뛴다.

가 일어나는 경우가 있다. 이때 다시 과정을 수행하는 만큼 추가적인 연산이 들게 된다.

#### Reflow 가 발생하는 원인
- 윈도우 리사이징 시 (= viewport 크기 변경)
- 노드 추가 또는 제거
- 요소의
	- 위치 변경
	- 크기 변경
- 폰트 변경
- 이미지 크기 변경

#### Repaint 가 발생하는 원인
- Reflow 가 발생 했을 시
- Reflow 가 발생 하지 않았을 시
	- background-color, visibility 등등의 레이아웃에 영향을 주지 않는 속성이 변경 되었을 시


#### Reflow / Repaint 를 발생시키는 속성
- Reflow
	- position(+ top, left, right, bottom), display, float, width, height, padding, margin, min-✲, max-✲,  font-family, font-size, font-weight, line-height 등등
- Repint
	- background, background-image, background-position



#### Reflow 최적화
Reflow 의 원인을 없애거나, 그렇지 못하다면 더 빠르게 연산되는 reflow 를 

- 하드웨어 가속을 활용한다.
	- 요소를 별도의 레이어로 분리하여 GPU에게 작업을 위임해야한다.
	- transform 속성과 opacity 속성을 사용한다.
	- 이러면 레이아웃, 페인트 단계를 건너뛸수 있다.
- 가능 하다면 가장 하위 노드의 스타일을 변경한다.
- 인라인 스타일은 추가 reflow를 발생 시킨다.
- 애니메이션이 있는 노드는 position: fixed 또는 abolute 로 지정하여, 부모 노드에서 분리시킨다.
- table 태그 사용을 피한다.
- display 를 invisible 로 하기 보다는 none 으로 한다.
- 스타일 변경 시, 변경하고자 하는 스타일의 class 를 주입하는 것이 더 낫다.

https://beomy.github.io/tech/browser/reflow-repaint/
https://boxfoxs.tistory.com/408



## 하드웨어 가속 (GPU 활용)
----
하드웨어 가속은 CPU에서 처리해야할 작업을 GPU에 위임하여 더 효율적으로 처리하는 방법을 말한다. 이때 특정 요소에 하드웨어 가속을 하려면 요소를 별도의 레이어로 분리하여 GPU로 보내야한다.

이때 CSS 속성을 활용해서 처리하는 방법은 아래 2개 속성을 활용하는 것이다.
- transform
- opacity

이 2 속성을 활용해서 CSS 변화를 시키면, reflow, repiant 가 일어나지 않는다.


#### 암묵적인 컴포지팅
GPU가 맡는다고 무조건 장점만 있는 것은 아니다. 하드웨어 가속화가 되는 과정은 요소를 별도의 레이어로 분리해서 GPU 로 보내는 것이라고 했다. 좀더 상세한 과정은 아래와 같다

- 각각의 컴포짓 레이어를 개별적인 이미지로 페인트한다.
- 레이어 데이터(크기, 오프셋, 투명도 등등)를 준비한다.
- (적용할 수 있으면) 애니메이션에 적용할 쉐이더를 준비한다.
- GPU로 데이터를 보낸다.

이 과정은 꽤 무거운 작업이다.
문제는 이 작업이 암묵적으로 꽤 빈번히 일어 난다는 것이다. 특히 아래와 같은 경우이다.

- 하위에 있는 요소들에게 애니메이션 같은 변화가 주어졌을 시,
	- 상위에 있는 요소들도 별도의 레이어로 승격시킨다.
	- 하위에 있을 수록 이 작업은 많아진다.

우리는 이 작업을 최소화 하기 위해 아래와 같은 사항을 신경써야한다.

1. **하위 요소에 애니메이션 적용 피하기**: 
	- 가능하면 하위 요소에 대한 복잡한 애니메이션을 피하고, 대신 간단하거나 필수적인 애니메이션만 사용한다.
2. **상위 요소에서 애니메이션으로 해결**: 
	- 하위 요소 대신 상위 요소에 애니메이션을 적용하여, 필요한 시각적 효과를 달성하는 방법을 찾는다.
3. **will-change CSS 속성 사용**: 
	- 특정 요소가 애니메이션 또는 변화가 있을 것임을 브라우저에 미리 알림으로써, 브라우저가 렌더링 최적화를 더 효과적으로 수행할 수 있도록 한다.

![[implicit_compositing.png]]

#### Opacity 도 무조건 최적화가 되는건 아니다.
opacity: 1 로 설정해두고 0.x 소수점으로 변경하면, reflow 가 일어난다. 이때
opacity: 0.99 로 설정해두고 0.x 소수점으로 변경하면, reflow 가 발생하지 않는다.

이 글에서 자세히 설명되어 있다.
https://arc.net/l/quote/rjacwyns



Gecko 브라우저 에서는 Reflow, Repaint 라고 표현한다.



## 웹 성능 최적화
----
프론트단에서 해볼 수 있는 최적화의 종류 목록을 나열해보자면 아래와 같다.


- 최적화
	- 이미지 최적화
	- 폰트 최적화
		- 텍스트 압축
	- 캐시 최적화
	- 애니메이션 최적화
- 코드 분할
- 사전로딩
	- 컴포넌트 사전로딩
	- 이미지 사전로딩
- 지연로딩
	- 컴포넌트 지연로딩
	- 이미지 지연로딩
- 병목 코드 찾아서 최소화
	- 크롬에서 제공해주는 Performance 패널 활용
- 불필요한 CSS


### 최적화
##### 이미지 최적화
- 이미지 사이즈는 보여지는 최대 크기의 2배가 적당하다.
- 이미지 CDN 을 활용하여, 이미지 크기를 조절하고 다운로드 시간을 단축시킨다. (in 서버)
- 이미지 포멧 WebP 사용
	- 장단점
		- 사이즈 : PNG > JPG > WebP
		- 화질 : PNG = WebP > JPG
		- 호환성 : PNG = JPG > WebP
	- <picture></picture>`<picture></picture>` 태그를 활용한다
		- media 크게 에 따른 지정 가능
		- 호환성에 따른 적용

##### 폰트 최적화
- 폰트 적용시점 제어
	- css의 font-display 속성을 활용
	- X초 이후 불러오지 못하더라도 넘아가고, 일단 캐시 (속성값에 따라 조금씩 다름)
	- fontfaceobserver 라이브러리 활용해서, 더 자연스러운 적용
- 폰트 사이즈 줄이기
	- 폰트 포멧 TTF 폰트를 압축하여 웹에서 더욱 빠르게 로드할 수 있도록 한 WOFF 사용
	- 호환성 대응
		- @font-face 의 src 에 대응 순서대로 포멧 나열
- 폰트 로드 시간 줄이기
	- Data URI 적용
- 콘텐츠에 텍스트 압축 방식 지정 및 적용 (in 서버)
	- 압축 방식 gzip 또는 Deflate

##### 캐시 최적화
응답 헤더 Cache-Control 적용
- HTML 파일에는 no-cache 설정 (항상 최신버전 제공을 위함)
- JS, CSS 는 public값 적요용, max-age 약 1년 설정

##### 애니메이션 최적화
- 브라우저 렌더링 엔진 동작 원리 이해
- 리플로우, 리페인팅 현상을 최소화
	- transform, opacity 속성 활용
	- 암묵적 컴포지팅 유념


### 코드분할
코드 번들을 확인하고, 특정 시점에 불필요하게 다운받고 있는 지점을 파악함

- 동적 import 사용
```javascript
import('xxx').the((module) => {
	const { xxx } = module;
	xxx.run();
})
```
- React의 lazy() 함수와 Suspense 활용
```jsx
import { lazy, Suspense } from 'react'

const Comp = lazy(() => import('./Comp'));

function MyComp() {
	return (
		<Suspense fallback={<div>Loading...</div>}>
			<Comp />
		</Suspense>
	)
}

```

이때, 분할 단위에 따라 전략이 2개로 나뉨
- 페이지별로 분할
	- 페이지별로 번들 chunk가 분할되어서 다운로드 됨
- 컴포넌트별로 분할


### 지연로딩
##### 컴포넌트 지연로딩
코드를 분할하고 분할된 코드를 필요한 시점에 로드 되도록 하는 기법이다.

- React 의 lazy(), Suspense 를 활용하여 분할해둔다.
- state 를 통해서, 특정 이벤트로 인해 state 조건이 만족됐을 시 보여지도록 한다. (p.91)

##### 이미지 지연로딩
이미지가 화면에 보이는 그 순간 또는 그 직전에 이미지를 로드하는 기법이다. 덕분에 초기 로드 시간이 길어지는 것을 해결할 수 있다.

- 브라우저에서 제공하는 API인 Intersection Observer 를 활용한다.
- 요소가 화면(뷰포트)에 들어왔을때만, callback 함수 호출
```javascript
const observer = new IntersectionObserver((entries, observer) => {
	// callback
}, {
	root: null,
	rootMargin: '0px',
	threshold: 1.0,
} // options)

observer.observe(document.querySelector('#target-el-1'));
observer.observe(document.querySelector('#target-el-2'));

```

이 때 이미지가 이미 출력된 이후로는 observer 를 해제하는 함수도 넣어주는 것이 좋다.

```jsx

useEffect(() => {
	const options = {};
	const callback = (entries, observer) => {};
const observer = new IntersectionObserver(callback, options);
	observer.observe(imgRef.current);

	return () => observer.disconnect()
}, [])

```


### 사전로딩
##### 컴포넌트 사전로딩
컴포넌트의  지연로딩의 단점을 커버할 수 있는 기법이다. 특정 이벤트를 발생하기 이전에 미리 컴포넌트를 로드해 두는 기법이다.

로드 타이밍
- 사용자 특정 영여겡 마우스를 올려놨을 시
```jsx
const handleMouseEnter = () => {
	const component = import('./components/ImageModal')
}

```
- 최초 페이지가 로드되고 마운트가 끝났을 시
```jsx
useEffect(() => {
	const component = import('./components/ImageModal')
}, [])

```

##### 이미지 사전로딩
기존에는 화면에 그려지는 시점에 로드 되게끔 했다면, JS 로 직접 로드하는 방법을 활용한다.

```javascript
const image = new Image()
image.src = '{이미지_주소}'
```
```jsx
useEffect(() => {
	const component = import('./components/ImageModal'); 
	const img = new Image(); img.src = 'https://stillmed.olympic.org/...'; 
	}, []);
```
하지만 위 예제는 뭔가 하다 만 느낌이다. (p.101) 좀더 알아볼 필요가 있다.