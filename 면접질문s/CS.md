

## 동기 vs. 비동기





## Promise vs. Await / Async
-----
Promise는 JavaScript 에서 비동기를 처리에 사용되는 객체이다.  ES6에서 언어적 차원에서 지원한다.

Promise 는 아래와 같이 3개의 상태를 가진다.
- pending (대기)
	- Promise 객체 생성시
	- ``` new Promise() ```
- fulfilled (이행)
	- 인자로 넘어온 resolve 를 실행했을 시
	- ``` new Promise(function(resolve, reject) { resolve(); }); ```
- rejected (실패)
	- 인자로 넘어온 reject 를 실행했을 시
	- Promise 콜백함수 내부에서 에러가 발생 했을 시


#### Promise 활용
좀 뒤 설명하게 될 await / async 가 나온 뒤로, Promise 