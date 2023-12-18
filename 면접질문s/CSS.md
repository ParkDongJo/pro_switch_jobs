


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




emotion 을 포기
https://junghan92.medium.com/%EB%B2%88%EC%97%AD-%EC%9A%B0%EB%A6%AC%EA%B0%80-css-in-js%EC%99%80-%ED%97%A4%EC%96%B4%EC%A7%80%EB%8A%94-%EC%9D%B4%EC%9C%A0-a2e726d6ace6

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



## CSS의 BOX MODEL





## Media Query



