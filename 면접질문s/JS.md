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





## 실행 컨텍스트 실행
------
JS 엔진은 <script /> 요소를 처음으로 만난 시점에서 Global 실행 컨텍스트를 생성하고 Call Stack 에 push 한다.

그 이후
함수 호출을 찾을 때 마다 해당 함수에 대한 실행 컨텍스트를 생성하고 Stack 맨 위에 push 한다.

![[call_stack.png]]




## 실행 컨텍스트 구성 요소
----
실행 컨텍스트는 현재 실행되는 함수의 세부 데이터 제공하는 객체.  실행 컨텍스트는 3가지로 구성되어 있다.

- Variable Environment
	- environment Record
	- outer Environment Reference
- Lexical Environment
	- environment Record
	- outer Environment Reference
- This Binding

#### Variable Environment------
컨택스트 생성 단계의 정보들이다. 코드 실행 직전에 생성되며

##### environment Record 는 컨택스트 내의
- 함수의 인자
- var로 선언된 변수가 메모리에 매핑되고 초기값으로 undefined
- 유사배열
- 선언된 함수명

어떤 식별자가 있는지만 관심있고, 어떤 값이 할당 되었는지는 관심 없다. 그래서 변수 선언만 끌어올리고 할당 과정은 원래 자리에 남겨두는 **호이스팅**이 일어난다.


##### outer Environment Reference 
컨텍스트의 외부 환경에 대한 정보를 저장한다.

바로 직전의 컨텍스트의 Lexical Enviroment 를 참조한다. scope chain 에 저장 되며, 현재의 Lexical Environment 에서 변수를 찾지 못했을때, 이 scope chain 을 타고타고 올라가서 찾아볼 수 있습니다.


#### Lexical Environment------
이미 만들어진 Variable Environment를 복사해서 만들어진다. 코드가 실행되면서 변수에 값이 할당되거나 변경되면 Lexical Environment 에만 업데이트 된다.

이때 
- let, const 로 선언된 변수가 메모리에 맵핑되고 초기값은 uninitialized 상태이며 할당되지 않는다.
- 함수 선언이 메모리에 맵핑 된다.


#### This binding------
this 의 값이 여기서 결정된다. 기본적으로 this는 전역을 가르키지만 몇가지 예외사항이 있는데, 그건 this 를 다루는 내용에서 살펴보자



## 호이스팅
------
브라우저의 JS 인터프리터가 코드 실행 전 모든 선언된 변수와 함수를 메모리에 미리 등록 해놓는 작업을 의미한다.

- 변수는
	- var  호이스팅 O
		- undefined 로 초기화
	- let  호이스팅 O
		- uninitialized 상태
		- 초기화 X
		- TDZ(Temporal Dead Zone)
	- const  호이스팅 O
		- uninitialized 상태
		- 초기화 X
		- TDZ(Temporal Dead Zone)
- 함수는
	- 선언식은 호이스팅 O = 함수 전체로 초기화
		- function fn() { ... }
	- 표현식은 호이스팅 △
		- 선언부는 변수로 호이스팅 O
		- 함수로 호출 불가
		- let fn = function() { ... }
	

실행 컨텍스트 생성 단계에서 Variable Environment 이 생성 되고, environmentRecord에 var 와 함수 선언식 그리고 그와 관련된 유사배열, 인자 까지 선언되고 초기화 된다.

하지만 let과 const 함수 호이스팅이 되지만 TDZ 상태이다. 이때 코드가 실행되면서 Lexical Environment 이 되어서 값할당 코드를 만나면 그제서야let, const로 선언한 변수에 값이 할당된다.

이때, 함수 표현식은 변수로 인식되어, 변수에 대한 선언만 등록되어, var는 undefined가 할당 되지만(let, const는 마찬가지로 TDZ상태) 함수로는 호출 할 수 없다. 

이런 흐름 때문에 호이스팅이 일어나는 것이다.

가장 정확하게 설명한 자료
https://youtu.be/EWfujNzSUmw?si=-bOqcKAegXC7-4m_

가장 괜찮은 자료
https://arc.net/l/quote/ronhlvyf


https://hangeoreum.tistory.com/entry/JS-%EC%8B%A4%ED%96%89-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8Execution-Context
https://kwangsunny.tistory.com/37
https://arc.net/l/quote/fcwtfyhc



## 클로저
------
클로저는 자바스크립트의 고유한 개념이 아니다. 이는 함수형 프로그래밍 언어에서 공통적으로 발견되는 특징중에 하나인데, 자바스크립트도 함수형 언어의 특징을 일부 가지고 있다.

클로저는 함수가 선언될 당시 자신의 외부 환경의 식별자를 기억하는 현상을 말한다.

그럼 내부적으로 왜 이런 현상이 나타나는 지 살펴보자.
우선 앞서 배운 실행 컨텍스트와 연관이 있다. 실행 컨텍스트는 함수 호출 시 생성이 되고, Call Stack 에 쌓인다.

이때 각각의 실행 컨텍스트는 Variable Environment로 인해 복사된 Lexical Environment 를 가지는데 이 어휘적 환경 내부에 있는 outer environment reference 는 외부의 환경에 대한 참조값을 가진다.

정확히는 scopeChain 이라는 참조 값을 가지는데, 이는 자신을 포함한 외부 환경을 가르킨다. 덕분에 이 상태에서 inner(그림 기반) 함수의 평가를 outer(그림 기반) 함수를 실행한 이후 지연된 평가를 했을 때에도, 여전히 outer의 환경을 저장하고 있으며, 데이터를 불러 올 수 있다.


![[Pasted image 20231222104546.png]]![[스크린샷 2023-12-22 오전 10.46.01.png]]

그림을 보면, ella 라는 변수에 담긴 함수는 inner 함수의 참조값이고 inner 함수는 outer의 외부 환경 참조값을 가지고 있다.
코드는 아래와 같이 표현될 수 있겠다.

```javascript
function outer() {
	let x = 10;
	function inner() {
		console.log(x);
	}
	return inner;
}

const ella = outer();
	ella(); // x = 10

```


클로저 덕분에 장점들이 있다.

- 함수로 모듈화가 가능해진다.
- 함수로 캡슐화가 가능해진다.

덕분에 함수형 프로그래밍에서 활용될 수 있는
- 커링
- 부분평가

등등의 기법들이 가능해진다.



## 스코프
----




## 프로토타입
-----
자바스크립트는 프로토 타입 언어이다.  자바스크립트의 객체에는 Prototype 이라는 내부 프로퍼티가 존재한다. 



## prototype vs. _proto_
----
Javascript 에서 함수는 객체이다. 함수를 생성하면 함수의 객체가 생성되고 동시에 함수의 prototype 객체가 생성된다. 이때,

- 함수의 prototype 은 함수의 