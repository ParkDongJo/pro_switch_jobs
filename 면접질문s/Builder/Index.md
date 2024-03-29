


## bundler를 사용해야하는 이유
-----
웹에서 왜 번들러

1. 모듈 시스템에 대한 필요성
	- 서버사이드에서도 JavaScipt 를 활용하자
	- v8 의 등장
2. CommonJS와 AMD 등장
	- CommonJS 동기적으로 호출 방식 채택
	- AMD 비동기적 대응 방식 채택 (동기적으로도 가능)
3. UMD 등장
	- CommonJS와 AMD 방식을 모두 호환
4. ES6 Module 등장
	- JavaScript 언어 자체에서 모듈 시스템 지원 시작
	- 비동기, 동기 모두 지원
	- 간단한 문법
	- 단, 최신 브라우저에서만 지원됨
5. 트랜스 파일러 Babel 의 등장
	- 한번 컴파일 하면 구형 브라우저까지 대응해줌
	- 최신문법을 대부분의 브라우저에 맞춰서 컴파일 해줌
6. TypeScript, CoffeeScript 슈퍼셋 언어 등장

다시 프론트 입장에서
5. 테스크 러너 Grunt, Gulp 의 등장
	- 소스코드 축소
	- 소스코드 단일화
	- 소스코드 테스팅
	- 프론트단 테스크를 코드로 자동화
6. 번들러 webpack 의 등장
	- 번들링 영역에서 좀 더 전문적인 번들로 필요성
	- CSS, JS 전처리
	- 이미지 에셋 관리
	- 코드 분할
	- 편리한 개발 서버
	- 트리 쉐이킹

간단히, 시대에 흐름에 따른 프론트 단의 번들러 등장에 대해 이야기 해봤는데요. 그 필요성을 간추려 보자면 웹 기술의 발전에 따라 웹 어플리케이션의 규모가 커졌고 그만큼 서버에 요청하는 파일의 크기와 갯수가 점점 커졌습니다. 

또한 과거의 HTML의 문서기반의 서비스가 SPA 의 발전으로 인해 컴포넌트 단위로 파일 관리가 이뤄졌습니다.
이로 인해 모듈시스템과 관련된 생태계가 발전함과 동시에, 자연스럽게 여러개의 모듈을 하나의 단일 파일로 만드는 번들러의 필요성도 생긴겁니다.

그 이유는
- 중복된 이름으로 인한 JS 에러
- 여러 파일 요청으로 인한 응답지연

등등이 문제가 있었고, 이를 해결하기 위해 번들링이라는 작업이 필요했기 때문입니다. 지금까지도 webpack 이 많이 쓰이지만, 최근에는 좀 더 프로젝트의 성향에 맞게 선택사항이 많아 졌습니다.

- rollup
- parcel
- vite

등등의 다양한 모듈러들이 나왔습니다.


https://artjoker.net/blog/gulp-vs-grunt-vs-webpack-tools-and-task-runners-which-technology-is-better/

https://medium.com/naver-place-dev/javascript-%EB%B2%88%EB%93%A4%EB%9F%AC%EC%9D%98-%EC%9D%B4%ED%95%B4-4-webpack-%EB%B0%8F-%EB%8B%A4%EB%A5%B8-%EB%B2%88%EB%93%A4%EB%9F%AC%EB%93%A4-e5158e94ef60


번들러의 역사
https://yozm.wishket.com/magazine/detail/1261/



## Webpack
----
웹팩 [공식 Github](https://github.com/webpack/webpack) 에서는 웹팩을 **'****모듈(module)'**을 위한 **'번들러(bundler)'** 라고 소개한다.


모듈이란 재사용 가능한 코드 조각이다. 모듈은 자신만의 파일 스코프를 갖고 import, export 를 할 수 있다.

JavaScript 를 통해 만드는 프로젝트들 크기가 점점 커지면서, AMD, CommonJS, UMD 등등 모듈 시스템이 나왔다.

이때, 브라우저 환경에서는 서버와 통신 시 각 모듈별 통신을 하기 보다, 하나의 번들링된 파일로 통신하는 것이 비용을 낮출 수 있다. 이때문에 전문 번들러에 대한 니즈가 커진다.

### 왜 Webpack 인가
결국 번들러는 프론트단에서 찾게되는 라이브러리이다. 

웹팩은 아래와 같은 기능들이 가능하다.
- JavaScript, CSS, SVG 다양한 포맷 번들 가능
- 핫리로딩 지원
- 코드 스플리팅
- 동적 로딩
- 지연 로딩


### Entry 설정
webpack 은 번들링 과정에서 Dependency graph 를 그린다. 출발점에서 애플리케이션의 모든 모듈 그래프를 재귀적으로 완성해 간다.
그래프를 완성하고 나면 이 모든 모듈을 하나의(소수) 번들로 묶어서 브라우저에 로드 할 준비를 마친다.

이때 우리는 config 파일에서 출발점을 알려줘야한다.

```javascript
const config = { 
	entry: './src/js/index.js',
};
```

출발점은 여러개로 지정도 가능하다.
```javascript
module.exports = {
  entry: ['./src/file_1.js', './src/file_2.js'],
};
```


### Output 설정
하나의 번들로 묶고 나서 그 결과물을 어디로 내보낼지 지정해줘야한다. 기본값으로 **./dist** 에 전달된다.

- filename
- path
- clean
	- `사용하지 않는 파일을 알아서 정리해준다.`
- publicPath
	- `브라우저에서 접근할 수 있는 파일의 위치를 지정한다.
- library
	- `library를 만들 경우 이름, 타입, 코멘트 등등을 지정한다.`

등등의 설정을 지정해줄 수 있다. 그 외 많은 옵션들이 있다.


### Module 설정
Webpack 은 기본적으로 JS, JSON 포멧만 이해할 수 있다. 그 외에 CSS, HTML, Image 포멧, SVG 등등의 다른 포멧들을 번들링 하려면, 그에 맞는 loader 들을 설정해줘야 한다.

두가지 필수 속성이 있다.
- test
	- `언떤 파일을 변환할 지 지정하는 속성, 정규표현식으로 작성한다`
- use
	- `변환에 필요한 로더를 명시한다`

이렇게 설정해주면, 어떠한 포멧이든 모듈로 import 가 가능해진다.

설정 예시는 아래와 같다.
// webpack.config.js
```javascript
const config = {
	module: { 
		rules: [ 
			{ 
				test: /\.css$/, 
				use: [ 
					{ 
						loader: MiniCssExtractPlugin.loader 
					}, 
					{ 
						loader: 'css-loader', 
						options: { import: true }, // 기본값 true
					}, 
				], 
			}, 
		], 
	}, 
};
```

##### 바벨 설정
React 와 함께 설정을 하려면, jsx 포멧도 설정을 해둬야 한다. 설치해야할 플러그인은 

```shell
@babel/core @babel/preset-env @babel/preset-react babel-loader
```

use 안에 options 로 넣는 방법도 있지만, .babelrc 파일로 따로 preset 을 설정해야한다.
```json
{
	"presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

그리고 webpack module 의 rules 에 설정하면 된다.

##### CSS 설정
js 파일에서 css 파일을 불러오기 위해 사용하는 로더가 css-loader 이다.

```shell
css-loader
```

- css 파일을 모듈로 불러 올수 있다.
- css 파일을 string 으로서 읽어들인다.
- class 이름을 난수화 시키기 때문에, locally-scoped 가 가능하다
- 몇가지 옵션들이 있는데, 그때그때 찾는게 좋을 것 같다.
	- import
	- sourceMap
	- localsConvention


css-loader 를 통해서 읽어드린 스타일을 가져와 페이지의 `<head>`요소에 해당 스타일을 포함하는 `<style>` 태그를 생성한다.

```shell
style-loader
```

- 옵션 중 injectType 을 lazy(지연) tag 로도 설정이 가능하다.
	- 지연 로딩이 가능해진다.
	- 파일명을 *.lazy.css 로 갖게 한다.

이 모든 과정은 js 파일 내부에서 코드로 변환되며, bundle.js 파일에서 검색해보면 관련 코드를 찾을 수 있다.


##### 그 외 로더
- [Babel Loader](https://webpack.js.org/loaders/babel-loader/#root)
- [Sass Loader](https://webpack.js.org/loaders/sass-loader/#root)
- [File Loader](https://webpack.js.org/loaders/file-loader/#root)
- [Vue Loader](https://github.com/vuejs/vue-loader)
- [TS Loader](https://webpack.js.org/guides/typescript/#loader)



### Plugin
검색을 해보면, 2가지 플러그인은 꼭 추천이 되는걸로 보인다. 그 외에 다른 플러그인들은 그때마다 필요한걸 찾아서 설정하면 될 것 같다

- html-webpack-plugin
	-  dist의 index.js를 스크립트 파일로 포함하는 HTML 문서를 dist 디렉토리 내에 자동으로 생성해준다
	- 원본 index.html 을 template 속성에 넣어준다.
- mini-css-extract-plugin
	- 사용하면 스타일시트를 분리해서 dist 디렉토리 내에 CSS 파일을 따로 만들어준다.
	- 만약 style-loader 를 사용하면 index.js 내에서 createElement() 를 통해 JS에 주입시킨다.

```javascript
module: {
	rules: [
		{
			test: /\.css$/,
			use: [
				MiniCssExtractPlugin.loader,  // style-loader 대타로 대입
				{
					loader: 'css-loader',
					options: { import: true },
				},
			],
		},
	],
},
plugins: [
	new HtmlWebPackPlugin({
		template: './src/index.html'
	}),
	new MiniCssExtractPlugin({
		filename: "index.css"
	})
],
```

##### 그 외 자주 사용하는 plugin
- [split-chunks-plugin](https://webpack.js.org/plugins/split-chunks-plugin/)
- [clean-webpack-plugin](https://www.npmjs.com/package/clean-webpack-plugin)
- [image-webpack-loader](https://github.com/tcoopman/image-webpack-loader)
- [webpack-bundle-analyzer-plugin](https://github.com/webpack-contrib/webpack-bundle-analyzer)
- webpack.ProvidePlugin



https://joshua1988.github.io/webpack-guide/concepts/plugin.html#plugin
https://www.eastflag.co.kr/webpack/webpack-plugin-and-loader/
https://365kim.tistory.com/35
웹팩 설정에 대한 경험기
https://velog.io/@designc/Webpack5-%EC%84%A4%EC%A0%95-%EB%AC%B4%EC%9E%91%EC%A0%95-%ED%95%B4%EB%B3%B4%EA%B8%B0


## webpack vs. rollup vs.  ESBuild vs. vite
---
웹팩은 번들러의 춘추전국 시대에서 가장 오래되고 안전성을 가진 번들러이다

롤업은 현재 차세대 번들러들의 많은 귀감이 되고 있는 번들러이다. 그동안 웹팩의 그늘아래 가려졌었으나, 라이브러리를 만들때 특화된 번들러로 각광을 받고 있다.

ESBuild 는 JavaScript 기반의 기존 번들러에 대항해 Golang으로 만들어진, 빠른 성능을 자랑하는 번들러이다. 다만 아직까지는 안전성과 라이브러리들과의 호환성이 떨어진다.

ESBuild를 랩핑하고 Rollup의 간단한 설정을 이어받은 차세대 번들러이다. 덕분에 설정은 간단하고 속도는 빠르다.



## vite
----

차세대 번들러 비교분석 https://bepyan.github.io/blog/2023/bundlers
https://www.youtube.com/watch?v=iX3Nu1FcZKA
웹펙에서 -> vite 로 갈아탄 경험 https://engineering.ab180.co/stories/webpack-to-vite




## vite 에서는 polyfill 을 지원하지 않는다.
-----

vitejs/plugin-legacy 또는 polyfill.io를 사용하여 Vite 프로젝트에 폴리필을 추가할 수 있습니다.

https://javascript.plainenglish.io/why-cant-vite-use-new-syntax-46b50886a1db




## next.js vs. vite.js
----


vite 에서는 SSR 타이밍 구현에 대한 라이브러리들을 추천하고 있다.
https://github.com/vitejs/awesome-vite?tab=readme-ov-file#ssr


둘을 비교
https://arc.net/l/quote/hmwlmzuc

vite 로 SSR 타이밍 만들어 보기
https://github.com/bluwy/create-vite-extra/blob/master/template-ssr-react/server.js
https://ko.vitejs.dev/guide/ssr.html




## Babel 과 Polyfill
----
### Babel
JavaScript 단에서의 발전으로 ES6 까지 나오게 되면서, 이전 브라우저에 대한 호환성이 대두가 되었고 이를 Babel 이 ES6 최신 문법을 ES5 문법으로 변환해주는 트랜스파일링(변환) 작업을 해결해주었다.

이후, React의 JSX, Typescript 정적타입 언어 등등의 다양한 변환을 제공하면서 프론트에 있어서는 필수적인 기술로 자리 잡았다.

Babel은 빌드 타임에 실행된다.

### Polyfill
폴리필은 하위 버전 브라우저에 없는 구현을 채워주는 역할을 한다. 
또한 브라우저마다 지원되는 기능이 다르기 때문에, 이런 크로스 브라우징을 런타임에 대응하려면 polyfill 이 필요하다.
소스코드가 어떤 브라우저 내에서 사용될 지 빌드타임에는 알 수가 없고, 모든 브라우저를 다 대응하면 그만큼 소스코드의 크기도 커지기 때문에 런타임에 돌아야 한다. 덕분에 하위 브라우저에 대해서도 대응을 할 수 있다.

Polyfill 은 런타임에 등록되지 않은 메서드나 기능을 주입해준다.
https://minsoftk.tistory.com/82

### 바벨 변환 과정
1. 파싱 -> 소스코드를 AST(Abstract Syntax Tree)로 변환한다.
2. 변환 -> 생성된 AST 를 순회하면서
	1. 하위 브라우저가 지원하는 오래된 문법으로 AST를 변경한다.
3. 생성 -> 변환된 AST 바탕으로 새로운 소스코드 생성


### 바벨 플러그인
개별적으로 바벨을 사용하기 위해 명령어를 통해서 실행하는 방법도 있겠지만, 번들러와 함께 preset으로 셋팅해두는게 생산성 측면에서 현명하다

- @babel/cli 를 통해 명령으로 실행
	- `npx babel SampleBabel.jsx --presets=@babel/preset-react ...(생략)...`
- ⭐️ 바벨 설정파일에 presets을 설정 + 웹팩의 babel-loader 를 셋팅한다.
	- .babelrc 또는 babel.config.js 설정 파일에 presets 을 지정해놓는다.
	- webpack 의 module에 `babel-loader` plugin을 설정한다

바벨의 대표 presets 으로
	- `@babel/preset-env` : ECMAScript2015+ 를 변환할 때 사용
	- `@babel/preset-flow` : flow를 변환
	- `@babel/preset-react` : react를 변환
	- `@babel/preset-typescript` : ts를 변환
등등이 있다.

### 폴리필 적용
폴리필을 적용하는 방법은 3가지가 있다.

- @babel/polyfill 을 import
- core-js 에서 필요한 폴리필만 import 사용
	- https://github.com/zloirock/core-js 참고해서 필요한 것들만 직접 셋팅하는 방법이다.
- ⭐️ 바벨 설정에서 preset 을 활용한다
	- `@babel/preset-env` 를 셋팅한다.
	- useBuiltIns 를 'usage' 로 설정한다. (필요한 폴리필만 포함하게 된다.)
		- https://babeljs.io/docs/usage

https://javascript.plainenglish.io/why-cant-vite-use-new-syntax-46b50886a1db
https://nukw0n-dev.tistory.com/25



## polyfill 최적화
----
polyfill 을 사용했을 시 당면하는 최적화 문제들이 있다.

1. 필요한 폴리필만 받자
2. 최신 브라우저에서도 폴리필을 불필요하게 받는다.

1번째 문제부터 풀어보자. 이 때 2가지가 있다.
- core-js 에서 필요한 폴리필만 import 해서 사용하는 방법
- ⭐️ 바벨 설정에서 preset 을 활용한다.
	-  `@babel/preset-env` 를 셋팅한다.
	- 타겟 브라우저를 설정한다.
	- useBuiltIns 를 설정한다.

2번째 문제는 Financial Times에서 관리하고 있는 polyfill.io 서비스를 적용하면 된다. 해당 사이트에서 생성한 polyfill bundle url 을 import 해주면, User-agent 에 따라 동적으로 필요한 폴리필 스크립트를 제공한다.

즉
- 최신 버전의 Chorme 에서는 아무 polyfill 이 내려오지 않는다.
- IE 11 에서 실행하면 많은 양의 polyfill 이 내려온다.

토스에서는 이 polyfill 서비스도 간혹 ES 표준대로 동작하지 않는 이슈가 있다고 하여, 자체적으로 Polyfill Node.js 서버를 구축했다고 한다. 시간 날 때 직접 따라 구현해보는 것도 재미난 도전과제가 되겠다.

다만 esbuild 번들러 기준으로 작성된 설정 코드라 나름의 커스텀은 필요하겠다.


https://toss.tech/article/smart-polyfills



## CommonJS vs. AMD
---
ES6 Module 의 등장으로 지금은 과거에 비해 논쟁이 사그러 들었지만, 여전히 node 에서는 CommonJS 명세를 따르고 있다.

JavaScript 진영에서
- 브라우저 밖에서도 JS를 사용하고자 하는 노력들이 있었고
- V8 의 등장에 힘입어

서버사이드에서도 JavaScript 가 사용할 수 있게 되었다. 하지만 이와 동시에

- 모듈 시스템의 필요했고
- 이에 맞는 표준화도 필요했다.

그래서 초기에 대표적으로 나온 것이 CommonJS 인데, AMD 는 CommonJS 진영에서 분리되어 독자적인 길을 가는 진영이였다.

### 비동기 지원
이 두 진영의 주요 쟁점은 비동기 지원이였다.

CommonJS 는 서버사이드 영역에 중점을 두고 있었고, 비동기 지원이 중요하지 않았다
	- 서버 사이드에서는 파일단위의 scope 가 보장되기 때문에, 변수들 끼리 덮어 씌워지는 JS의 고질적인 문제를 걱정할 필요가 없었다.
AMD 는 이런 모듈 시스템을 브라우저영역에서도 사용하려면, 비동기 지원이 필수라고 생각했다.

commonJS
```javascript
// 선언부
const $ = require('jquery'); 
const Z = require('zerocho'); 

module.exports = { a: $, b: Z, };
```
```javascript
// 사용부
const my = require('myModule'); 
const T = require('TweenMax'); 

console.log(my.a, my.b); 
console.log(T);
```

AMD
```javascript
// 선언부
define(['jquery', 'zerocho'], function($, Z) { 
	return { 
		a: $, 
		b: Z, 
	}
});
```
```javascript
// 사용부
require(['myModule', 'TweenMax'], function (my, T) { 
	console.log(my.a); // jquery 
	console.log(my.b); // zerocho 
	console.log(T); // TweenMax 
	console.log(jquery); // undefined 또는 에러 
});
```



그래서 이 둘은 각자의 노선을 향해 나아갔고, 지금에 와서는 사실 두 진영 다 브라우저, 서버사이드 영역들을 모두 지원을 하고 있다.
다만, 각자 주요 영역에서 사용할때 더 많은 장점이 있다고 하는데, 일단 거기까지 파진 않겠다.


### UMD, ES Modules
umd 는 두 그룹을 서로 호환되게 하는 방식이다. 어떤 모듈을 쓰던 동작하게 합니다. UMD 는 어떤 시스템이라기 보다는 코드 패턴에 가깝다.
ES6 에서 부터 Module 화가 지원되면서, 훨씬 간단한 방법으로 모듈화가 가능해졌다. 하지만 이는 문제가 있는데 최신 스팩이다 보니 지원을 하지 않는 브라우저들이 많았다.

그렇다보니 구형브라우저에서도 최신 JS를 대응해주기위한 트랜스파일러들이 등장하기 시작했는데, Babel 이 바로 그 역할을 한다.


https://arc.net/l/quote/dujqtslw
https://d2.naver.com/helloworld/12864




## Babel vs. Polyfill 의 차이
----

babel 은 
- ES6 이상을 -> ES5 으로 변환
- 빌드 타임
polyfill 은
- ES5 의 환경에 존재하지 않는 객체, 기존객체의 새로운 메서드, 새로운 메서드 등등
- 런타임

... 좀더 정리가 필요

https://arc.net/l/quote/rjktwmij





## vite
------
이게 vite 의 장점이나 스팩에 대해, 겉핧기 식 지식이 많고 마치 특정 기술이 다인양 이야기 하는 곳이 많아서 헷갈릴 수 있는데, 우선 vite 가 인기를 받기 시작한 가장 큰 이유는

-  번들링 속도가 빨라서

이다.

전통적인 번들러는 그 당시 JavaScript 를 모듈화해서 브라우저에 보여줘야 하는데 개별적으로 로드 받아버리면

- 네트워크 비용도 그만큼 늘어날테고
- 비동기 이슈

도 있을 것이다. 그래서 최선의 방법으로 모든 JS 파일을 1개의 번들러를 만드는데 초점이 맞춰져 있다.

하지만, 서비스들이 모두 고도화 되고 한 서비스에 다양한 npm 모듈들도 탑재가 되면서 번들의 크기가 점점 더 커지고 빌드 시간도 늘어났다.

이에, 차세대 빌더들이 해결하고자 하는 과제는 바로 번들링 속도이다.

그 중 하나는 바로 vite 이다.
vite는

- Esbuild 로 사전 번들링을 한다. 이 마저도 다른 번들러보다 훨씬 빠르다.
	- Esbuild 도 차세대 번들러인데, Go 언어로 작성되어 있어서 JS 로 작성된 번들러들보다 훨씬 빠르다.
- 개발 서버의 번들은 Native ESM을 이용한다.
	- 브라우저 요청 시 마다 소스코드를 변환하고 제공하기만 한다.
	- 수정된 모듈과 관련된 부분만 교체한다.
	- 각 dynamic import 를 실제 화면에서 사용되는 경우에만 처리한다.


하지만 이런 vite 도 프로덕션에서는 번들링을 한다. 이유는

- 프로덕션에서 개별 ESM을 가져오는것은 네트워크 비용이다.
- 최적의 프로덕션 성능을 위해서는
	-  트리쉐이킹
	- 지연로딩
	- 청크 분할
	등등으로 개선해야한다.


이때 vite 는 번들링 할때는 Esbuild 를 사용하지 않는다. 이유는

- Esbuild 와 vite 의 플러그인이 호환성이 잘 맞지 않다.
	- 오히려 Rollup 의 유연한 플러그인 API와 그 인프라를 활용한다.
- 대신 v4 부터 SWC 로 전환했으며,
- Rolldown 이라는 rust 기반 작업이 완료되면, 이 그외 다른 builder 들 보다 큰 빌드 성능을 가지게 될 것이다.


게다가
- vite 는 기본적으로 ES6을 타겟으로 번들을 생성하기 때문에,
	- polyfill 과 함께 설정을 해줘야한다.


사전 번들링 기능은 Esbuild
개발 서버의 원본 소스는 Native ESM 이용
프로덕션 배포 시 번들링 진행
번들링 시 Esbuild 는 사용하지 않고, Rollup 을 사용



https://arc.net/l/quote/qpbbqmeu
https://ko.vitejs.dev/guide/why.html
https://bepyan.github.io/blog/2023/bundlers



## 번들러의 역할은?
---
ESbuild 는 여러개의 ms 를 만들어낸다.