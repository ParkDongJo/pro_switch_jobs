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

코드로 나타내자면 아래와 같다. next() 메소득

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