


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

등등의 설정을 지정해줄 수 있다. 그 외 많은 옵션들이 있다.

