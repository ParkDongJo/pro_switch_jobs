**목차**
- [this가 가르키는 곳](#this가-가르키는-곳)
- [apply, bind, call의 차이점](#apply,-bind,-call의-차이점)

<br/>
<br/>

## this가 가르키는 곳
------
자바스크립트에서 this는 기본적으로 window (전역객체)를 가르킵니다. 하지만 함수 내부에서 this가 선언되어 있을 때는 함수가 어떻게 호출 되는지에 따라 this에 바인딩할 객체가 동적으로 결정됩니다.

this가 가르키는 곳은 몇가지 상황에 따라 다릅니다.
- 전역 this -> window 또는 global
- 일반 함수 안에서 쓴 this -> window 또는 global
- inner 함수 안에서 쓴 this -> window 또는 global
- arrow 함수 안에서 쓴 this -> **상위 lexical scope의 this**
- 메서드 안에서 쓴 this -> 자신이 종속된 객체 인스턴스
- 생성자 함수 안에서 쓴 this
	- new 키워드로 생성시 -> 생성한 객체 인스턴스 
	- new 키워드로 생성하지 않았을 시 -> window 또는 global
- 이벤트 핸들러 안에서 쓴 this -> 이벤트 핸들러를 등록해준 HTML 요소(또는 객체)

<br/>
<br/>

## apply, bind, call의 차이점
------
이 3가지 내장 메서드들은 this 와 관련이 있습니다.

**call(this, a, b, c)**
- call()은 this를 바인딩 시키면서 a, b, c 등등 인자를 넘길 수 있다.
- this 바인딩과 동시에 함수를 실행 시켜 준다.

**apply(this, [a,b,c])**
- apply()는 this를 바인딩 시키면서, 인자로 배열을 넘겨준다. 결과는 같지만 인자 형태가 call()과 다르다.
- this를 바인딩과 동시에 함수를 실행 시켜준다.

**bind(this, a,b,c)**
- bind()는 this를 바인딩 시키면서 a, b, c 등등을 인자를 넘길 수 있다.
- 하지만!!! call()과 apply()와 달리 this 바인딩만 시키고 함수를 실행시키지 않는다.


##### 언제 사용하면 되나
- this 바인딩 시
- 유사배열 대응 시


<br/>
<br/>

## 실행 컨텍스트
------


<br/>
<br/>

## 호이스팅
------


<br/>
<br/>

## 클로저
------




## for in 과 for of 의 차이점
----
for in 은 객체의 속성을 반복하는데 사용된다. object 에서 key를 뽑아내는 형태로 구현할 수 있습니다.

```javascript
const obj = { a: 1, b: 2, c: 3 };

for (const prop in obj) {
  console.log(`obj.${prop} = ${obj[prop]}`);
}
```


for of는 배열, 문자열, 맵, 세트와 같은 이터러블 객체를 반복하기 위해 설계되었습니다. 

```javascript
const array1 = ['a', 'b', 'c'];

for (const element of array1) {
  console.log(element);
}
```


## 이터러블 과 이터레이터
------
이터러블 - 이터레이터를 리턴하는 [Symbol.iterator] () 메소드를 를 가진 값

이터레이터 - { value, done } 객체를 리턴하는 next() 를 가진 값

이터러블 프로토콜을 준수한 객체는

- for - of 를 통해 객체 순회가 가능하다
- ... spread 문법을 사용할 수 있다.

코드로 나타내자면 아래와 같다. next() 메소드를 호출하여 이터러블 객체를 순회한다.

```javascript
const iterable = {
	[Symbol.iterator]() {
		let i = 3;
		return {
			next() {
				return i == 0 ? {done: true} : {value: i--, done: false};
			},
			[Symbol.iterator]() {
				return this;
			}
		}
	}
};
```

이 덕분에 Map, Set, Array 와 같은 다양한 자료구조에서도 하나의 반복기 규약으로 순회가 가능하도록 합니다. 또한 이런 이터러블 규약을 직접 정의하여 커스텀 할 수 도 있습니다. 원한다면 JSON 객체도 가능합니다.



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



## debounce / throttle
----
