


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




## next.js vs. vite.js
----




둘을 비교
https://arc.net/l/quote/hmwlmzuc

vite 로 SSR 타이밍 만들어 보기
https://github.com/bluwy/create-vite-extra/blob/master/template-ssr-react/server.js
https://ko.vitejs.dev/guide/ssr.html




## Babel
----




https://toss.tech/article/smart-polyfills
https://javascript.plainenglish.io/why-cant-vite-use-new-syntax-46b50886a1db
https://nukw0n-dev.tistory.com/25