

## 동기 vs. 비동기





## Promise vs. Await / Async
-----
Promise는 JavaScript 에서 비동기를 처리에 사용되는 객체이다.  ES6에서 언어적 차원에서 지원한다.

#### Promise()
Promise 는 아래와 같이 3개의 상태를 가진다.
- pending (대기)
	- Promise 객체 생성시
	- ``` new Promise() ```
- fulfilled (이행)
	- 인자로 넘어온 resolve 를 실행했을 시
	- ``` new Promise(function(resolve, reject) { resolve(); }); ```
	- then() 으로 후속처리를 한다
- rejected (실패)
	- 인자로 넘어온 reject 를 실행했을 시
	- Promise 콜백함수 내부에서 에러가 발생 했을 시
	- catch() 로 후속처리를 한다


#### Promise 활용
좀 뒤 설명하게 될 await / async 가 ES8에서 나온 뒤로, Promise 는 마치 구시대 유물로 취급하는 경우가 있는데, 그건 잘못된 접근으로 생각된다.

Promise 가 적절한 경우가 있다.
- then() 을 통해 후속 처리 시점을 다르게 가져가야할 때
- Promise.all() 로 병렬 처리를 해야할 때


#### Async() / Await()
async/await 를 활용해 비동기식 코드를 동기식으로 처리할 수 있다. 단순한 Promise 작업을 좀 더 보완하는 측면도 있다.

사용시 규칙이 있다.
- async 함수 안에서 await 를 사용할 수 있다.
- async 함수는 Promise 객체를 리턴한다.
	- 그래서 동기식으로 처리하고 싶은 영역은 모두 async / await 가 이어진다

#### 