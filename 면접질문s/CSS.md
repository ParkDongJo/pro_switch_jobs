


## CSS-in-CSS vs. CSS-in-JS
-----
먼저 기존에 단순히 CSS 만을 사용했을 시, 당면하는 불편한점들이 있습니다.

- global에 선언되고, 중복된 class 이름을 피해야합니다.
- 프로그래밍 커 질 수록 css 를 관리하기가 힘들어 집니다.
- JS 코드와 상태값을 주고 받을 수 없습니다.
- 역할 분리 또는 재사용이 힘듭니다.

그렇다보니, 이런 불편점을 해결하기 위한 방법들이 있습니다. 크게는 아래 세가지로 나뉠 수 있습니다.
- css-in-css
- css-hybrid
- css-in-js

##### Css-in-Css
여기서 css-in-css 는 아래와 같이 정의해볼 수 있다.

- css 의 불편점을 개선한 기술동향
- css-in-js 외에 css에 대한 개선방법들

이렇게 정의를 하는 이유는 css-in-css 에대한 정확한 정의가 없다. 다만 아래와 같은 기술동향들이 있다. 
- CSS 전처리기 (pre processors)
- CSS BEM
- CSS Framework

먼저 CSS 전처리기는 sass(2006), less 등등이 있다. 기존 css 를 Nested 한 문법으로 작성해주면, 이를 변환시켜주는 변환기라고 보면 됩니다. css를 구조화했다는 것에 의의가 있다.

둘째 방법론에 대한 모색입니다. 그 중 BEM(2013)이 가장 대세가 되었다고 한다. selector를 class 로만 작성하고, 이름 규칙을 Block__Element--Modifier 로 하게끔 권장한다. 하나의 이름을 통해 구조를 표현하고 관리할 수 있게 하자는 것이다.

셋째 CSS Framework 는 대표적으로 TailwindCSS(2017) 가 있습니다. Utiliy-First 라는 방식으로 css의 class에 유틸리티한 이름을 붙이고 이들을 조합해서 개발하는 방식 입니다. class 이름도 기존과는 다른 파격적인 방식들을 제시하면서 인기를 끌었습니다.


##### Css-hybrid
이 영역은 사실 개인적으로 정의한 영역입니다. CSS Modules(2015) 엄밀히 말해서, css-in-css 라고 보기에도 힘들고 css-in-js 라고 보기에도 힘듭니다.

css 코드는 .css, .scss 같은 css 파일에 작성합니다. 하지만 이를 JS 파일로 가져와서 객체로 사용합니다. 이말은 JS 로직과 밀접하게 연결 된다.

덕분에 CSS의 Global namespace 로 인한 문제점이 해결됩니다. 컴포넌트와 함께 구조화 범위를 일치 시킬 수 있게 된다. 게다가 런타임 css-in-js 와 달리 빌드타임에 처리가 되기때문에, 런타임 성능이슈는 없다.


##### Css-in-Js
(2013년 이후) 웹 어플리케이션 영역의 성장으로 인해 React 와 같은 프론트 단에서의 프레임워크들이 나오면서, Css 에 대한 불편점은 더더욱 극대화 됐습니다.

기존 html 문서에서 컴포넌트 단위 개발방식으로 발전해가는데, CSS는 이에 따라오지 못하게 된다.

기술적인 발전 외에도 디자인 영역에서의 Tool의 발전도 한몫한 것 같다. Figma를 통해 CSS를 Tool이 생상해내는 시대가 왔고, 이제 더이상 CSS를 잘 짜는 것보다 컴포넌트의 구조를 잘 설계하는게 더 중요해진 시대가 온것이다.

CSS 의 문제 더 있는데 아래와 같다.

- 모든 스타일이 중복되지 않는 class 이름을 적용해야한다.
- class 이름 관리가 힘듦
- selector 로 인한 스타일 우선순위
- JS 코드와 상태값을 공유하기 힘듦
- 캡슐화가 불가능

등등이 있습니다. 그래서 JS를 통해 CSS를 만들고자 하는 움직임이 일어 났습니다. 가장 대표적인 css-in-js 가 바로 emotion, styled-components 입니다.

덕분에 컴포넌트 단위로 css를 만들고 의미를 부여할 수 있게 되었습니다. JS와의 긴밀한 데이터 공유도 가능해졌죠.

이런 방식을 덕분에 CSS의 Selector 를 더이상 활용하지 않아도, 컴포넌트 구조에 맞춰 css 코드를 작성할 수 있게 되었습니다.



## CSS-in-JS
-----
css-in-js 라이브러리 들은 2가지 큰 타입으로 나눌 수 있습니다.

- 런타임
- 빌드타임

##### 런타임
런타임 중에 스타일을 처리하고 DOM에 삽입합니다. 즉, 일반적으로 애플리케이션이 로드되고 사용자와 상호 작용할 때 스타일이 파싱되고 적용됩니다. 런타임 라이브러리는 뛰어난 유연성과 강력한 기능을 제공하여 컴포넌트 상태, 프롭 또는 자바스크립트 로직에 따라 동적으로 스타일을 지정할 수 있습니다.

- sytled component
- emotion

##### 빌드타임
빌드 시점에 스타일을 처리하므로 애플리케이션을 빌드할 때 스타일을 정적 CSS 파일로 컴파일합니다. 이 접근 방식은 브라우저가 런타임에 스타일을 파싱하고 적용할 필요가 없으므로 성능이 향상될 수 있습니다. 하지만 런타임 라이브러리에 비해 동적 스타일링에 대한 유연성이 떨어질 수 있습니다.

- linaria



CSS 분류
https://mine-it-record.tistory.com/656

CSS 역사 CSS-IN-JS 로 오기까지
https://yozm.wishket.com/magazine/detail/1319/
https://yozm.wishket.com/magazine/detail/1326/

css-variables 란
https://www.daleseo.com/css-variables/

https://velog.io/@hyejee0504/Tailwind-CSS-Bootstrap-Sass-styled-components%EC%9D%98-%ED%8A%B9%EC%A7%95

tailwindCSS 에 대한 상세글
https://leekihyun.tistory.com/entry/TailwindCSS-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0

linaria 에 대한 상세글
https://blog.logrocket.com/using-linaria-faster-css-in-js-react-apps/#what-linaria



## Emotion vs. Styled-component
----
일단 요약만 먼저하면

- 번들 크기 차이
	- styled 말고 core 만 사용한다면 번들크기가 상대적으로 작다
- 미묘한 성능차이가 있다.
	- emotion 이 미세하게 앞선다고 하는데, 런타임에서는 결국 도찐개찐 이라고 생각한다
- SRR 에서 별도의 설정없이 돌아간다
	- styled-components 는 SeverStyleSheet 를 설정해야한다고 한다.


https://velog.io/@bepyan/styled-components-%EA%B3%BC-emotion-%EB%8F%84%EB%8C%80%EC%B2%B4-%EC%B0%A8%EC%9D%B4%EA%B0%80-%EB%AD%94%EA%B0%80



## Css-in-Js 의 한계점
----
CSS-in-JS는 크게 분류가

- run-time
- zero-time

으로 나뉘는데, 특히 run-time CSS-in-JS 의 단점은
- 런타임에서의 스타일 직렬화로 인한 오버해드
- CSS-in-JS 는 번들 크기를 늘린다.
- 리액트 18v에서 동시성이 중요해지면서,
	- 이런 동시성에 대한 CSS 라이브러리들에 대해 가이드라인을 제시하고 있다. (https://github.com/reactwg/react-18/discussions/110)
	- 하지만 emotion 의 컨트리뷰터에 의하면 성능까지 챙기면서 동시성에 대응하고자 하는 것이 CSS-in-JS 에서 해결해볼 수 있는 일이 아니라고 결론 짓고 있다.
- 리액트 v18 서버컴포넌트에서는 사용이 불가능 하다.

결국 성능적인 측면에서 태생적인 한계를 가지고 있다.

이 컨트리뷰터는 결국 자신이 맡은 프로젝트에서 emotion 을 포기하고, Sass 와 tailwind 를 결합해서 사용하는 걸 선택하고 있다고 한다.

emotion 을 포기
https://junghan92.medium.com/%EB%B2%88%EC%97%AD-%EC%9A%B0%EB%A6%AC%EA%B0%80-css-in-js%EC%99%80-%ED%97%A4%EC%96%B4%EC%A7%80%EB%8A%94-%EC%9D%B4%EC%9C%A0-a2e726d6ace6


## CSS의 BOX MODEL
----
Box 모델이란 HTML 요소를 감싸는 네모난 상자 개념을 말한다. 안에서부터 contents > padding > border > margin 으로 구성됩니다.

이때 기본적으로 box-sizing 이라는 CSS 속성은 'content-box' 로 기본값을 가지고 있습니다. Box의 크기가 contents 기준으로 정해지는걸 의미한다. 이때의 width와 height는 contents 의 크기이다.

하지만 만약 box-sizing 을 'border-box' 로 바꾸면, width와 height 는  border, padding contents 를 포함한 크기이다.

![[css_box.png]]

- 블록 요소의 크기는 width, height + padding + border + margin 에 의해 계산된다.
- width 값이 없으면, 이때 다른 레이어 층이 같으면 부모의 width - padding - border - margin 에 의해 계산된다.
- height 값이 없으면, 자식의 height + padding + border + margin 에 의해 계산된다.


##### Box 로 된 요소를 배치
위와 같은 Box 모델로 구성된 요소를 어떻게 배치하고 보여줄지 결정하는 속성이다. display 속성에는 아래와 같이 여러가지가 있지만, 가장 대표적인 값들만 다뤄봅시다.

- block
- inline
- inline-block
- flex
- grid
- none

하나씩 살펴봅시다.

```css
display: block;
/*부모 영역에 한 줄로 꽉 차는 속성*/

display: inline;
/*안에 들어있는 컨텐츠의 크기 만큼만 차지, width, height 조절 불가능*/

display: inline-block;
/*inline의 속성을 가지고있으면서 박스의 속성도 가지고 있어서 width, height 조절이 가능*/

display: flex;
/*요소들을 한 방향으로 정렬할 때 편리*/

display: grid;
/*요소들을 격자(행열)로 정렬할 때 편리*/
```

https://velog.io/@asas33/css-box-model-flex-grid


## flexbox
------


https://d2.naver.com/helloworld/8540176




## Media Query
-----
다양한 디바이스 화면 크기를 대응하기 위해 사용한다

모바일에 특화된 미디어 쿼리는 min-width로 320px 부터 정의하는게 일반적이지만, 디바이스 크기가 점점 커지고 있는 현재로써는 480px 대응 부터 하는 것도 괜찮다고 생각한다.

- 768px 이하는 모바일
- 768 ~ 992 는 패드
- 1200px 이상은 PC

로 간주한다. 아래는 모바일 위주의 미디어 쿼리 정이라고 한다면,

```css
/* All Device */

/* Custom, iPhone Retina : 320px ~ */
@media only screen and (min-width : 320px) { … }

/* Extra Small Devices, Phones : 480px ~ */
@media only screen and (min-width : 480px) { … }

/* Small Devices, Tablets : 768px ~ */
@media only screen and (min-width : 768px) { … }

/* Medium Devices, Desktops : 992px ~ */
@media only screen and (min-width : 992px) { … }

/* Large Devices, Wide Screens : 1200px ~ */
@media only screen and (min-width : 1200px) { … }
```


PC위주의 서비스 일때는 아래와 같이 max-width 로 순차적으로 정의해주는 것이 좋다.
  
```css
/* All Device */

/* Large Devices, Wide Screens : ~ 1200px */
@media only screen and (max-width : 1200px) { … }

/* Medium Devices, Desktops : ~ 992px */
@media only screen and (max-width : 992px) { … }

/* Small Devices, Tablets : ~ 768px */
@media only screen and (max-width : 768px) { … }

/* Extra Small Devices, Phones : ~ 480px */
@media only screen and (max-width : 480px) { … }

/* Custom, iPhone Retina : ~ 320px */
@media only screen and (max-width : 320px) { … }
```



https://uxkm.io/publishing/css/03-cssMiddleclass/09-css_media_part2#gsc.tab=0



## CSS Transform




## Render blocking CSS
----
기본적으로 CSS는 렌더링 차단 리소스로 취급된다. 즉, CSSOM이 생성될 때까지 브라우저가 처리된 콘텐츠를 렌더링하지 않는다.

최초 렌더링 시간을 최적화 하기 위해

CSS 에서 해볼 수 있는 방법들로는
미디어 쿼리를 활용하여, CSS 파싱 시점을 필요할 때 하게끔 하는 것이다.

```css
<link href="style.css" rel="stylesheet" />
<link href="style.css" rel="stylesheet" media="all" />
<link href="portrait.css" rel="stylesheet" media="orientation:portrait" />
<link href="print.css" rel="stylesheet" media="print" />
<link href="other.css" rel="stylesheet" media="(min-width: 40em)" />
```

media 속성을 통해

- 화면이 가로로 됐을 시
- print 시점
- 최소 화면이 40em 일때

등등을 통해서 조건에 맞춰 css 를 로드 하는 것이다.

https://web.dev/articles/critical-rendering-path/render-blocking-css?hl=ko




## CSS 단위 px, em, rem, vw, vh
----
CSS 에서 단위를 분류하자면, 아래와 같이 분류가 된다. 우리는 웹상에서 다루는 단위만 취급해보자.

#### 절대적
- px : 1px == 1인치의 1/96 (웹에서는 px 만 주로 사용)
- pt : 1pt == 1인치의 1/72 
- pc : 1pc == 12pt

#### 상대적
- % - 부모 요소 기준으로 % 처리
- em - 글꼴 크기를 부모 요소 기준으로 배수
```css
.parent {
    font-size: 20px;

}

.child {
	/* 부모 기준 2rem 은 40px 이다 */
    font-size: 2em;
}
```
- rem - 글꼴 크기를 html(최상위 요소) 기준으로 배수
```css
html {
    font-size: 50px;
}

.parent {
    font-size: 20px;

}

.child {
	/* html 기준 2rem 은 100px 이다 */
    font-size: 2rem;
}
```
- vw - Viewport weight 의 축약어, 1vw 당 뷰표트 너비의 1% 만큼 계산됨
- vh - Viewport height 의 축약어, 1vh 당 뷰표트 높이의 1% 만큼 계산됨


#### 반응형 웹에서는 rem을 사용
각 디바이스 크기를 대응하기 위해, 효율적인 코드는

- 공통적인 부분
- 개별적인 부분

을 분리 시켜놓는 것이다. 여기서 각 디바이스당 개별젹인 스타일이 문제인데 아래와 같은 기법으로 나눌수 있다.

- 개별 스타일 - media-query
- 개별 폰트 - rem

으로 각각 대응한다.


#### webview 에서 겪는 vh 이슈


https://velog.io/@edie_ko/Tip-%EB%AA%A8%EB%B0%94%EC%9D%BC-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%97%90%EC%84%9C-100vh-%EC%A0%81%EC%9A%A9-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0-iosandroid



## Tailwind vs. StyleX
----
#### StyleX
styleX 는 메타에서 3년간 내부 주요 프로젝트들에서 사용되어 왔고, 최근 오픈소스로 풀렸다. 이 덕분에 안정성은 메타 내부에서 이미 어느정도 검증됐다고 볼 수 있다.

또한 CSS 분류중에서도 CSS-in-JS에 속하지만, 그 비교 대상은 Atomic CSS 분류에 속하는 tailwind 가 주요 비교 대상이다.

그 이유는 바로 styleX 는 컴파일러의 면모를 가지고 있는데, 빌드 단계에서 StyleX 코드를 컴파일하여 Atomic 한 CSS 모듈 단위들로 변환한다.

이는 기존 사용법은 CSS-in-JS 에 유사하면서도, 빌드 결과물은 Atomic CSS 와 유사하다. 물론 변환된 CSS는 런타임에 페인팅 단계에서 적용되는데, 이 또한 zero-time CSS 과 비교해서도 동적인 CSS적용에서 유리한 측명을 가지게 된다. 결국 StyleX는

- runtime CSS-in-JS 보다 빠르고
- Atomic CSS 보다 사용성이 좋고
- zero-time CSS-in-JS 보다 동적 CSS 적용에 유리하다.

위와 같은 장점들을 가진다.

기능적으로도 유용한 면들이 많은데,

- 조건에 따른 CSS를 구현하기 편하고
- 타입 안정성이 보장된다.
- create, props, defineVars 등과 같은 간단한 API를 가지고 css 의 역할 설계를 가져갈수 있다.

다만 아쉬운 점은
- 아직 초창기라 레퍼런스가 많이 없다


### tailwind
tailwind 는 Atomic CSS 분류에 대표적인 라이브러리 이다. 정해진 여러 class 들을 조합하여 스타일을 구축해가는 방법이다. 이렇게 되면 class 들이 많아졌을 시 코드 가독성 문제를 들 수 있지만 이 부분은 IDE 의 plugin 으로 어느정도 보완할 수 있다.

덕분에
- 각 class 들은 재사용성이 높고
- css 기반이기 때문에 runtime CSS-in-JS 가 겪는 성능적인 이슈도 없다.
- 규격화된 class 들 덕분에 일관된 스타일링이 가능해지고
- Purge CSS 같은 몇가지 도구를 활용하면, css의 class 도 최적화 할 수 있다.

다만 아쉬운 점은
- 여전히.. 가독성이 떨어진다.
- 러닝 커브가 높고
- props 나 변수를 통한 동적 속성을 하기가 힘들다.
	- inline-style 로 해결할 수 밖에 없다.

특히 위의 단점들 때문에,
일부 조직들에서는 tailwind 와 다른 라이브러리 들을 결합해서 사용하는 듯 보인다.


styleX
https://www.google.com/search?q=stylex+vs+tailwind&oq=stylex+vs+tailwind&sourceid=chrome&ie=UTF-8#fpstate=ive&vld=cid:9e385e36,vid:K10yeryaN58,st:0

Tailwind
https://lokendrakushwah.medium.com/why-tailwind-css-is-still-better-than-stylex-and-other-css-libraries-8a8efd5b45a2

둘을 비교
https://arc.net/l/quote/xjddzzke




## 현재 CSS 의 분류
----

- SEMANTIC CSS
	- css
	- sass
- CSS Modules
- CSS-IN-JS
	- Run time
		- emotion
		- styled-components
	- Zero time
		- linaria
- Atomic CSS
	- tailwind
	- styleX



https://velog.io/@sun1301/CSS-in-JSvsAtomicCSS


## SSR 에서의 Styled-components
-----


https://nextjs.org/docs/app/building-your-application/styling/css-in-js


https://handhand.tistory.com/291

https://velog.io/@shagrat/SSR-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C%EC%9D%98-CSS-in-JS