
**목차**
- [리액트 라이프사이클에 대해]("#리액트 라이프사이클에 대해")
- [useMemo 는 어떤 hooks이고, 언제 사용하나요?](# useMemo 는 어떤 hooks이고, 언제 사용하나요?)
- [useCallback 는 어떤 hooks이고, 언제 사용하나요?](#useCallback 는 어떤 hooks이고, 언제 사용하나요?)

## 리액트 라이프사이클에 대해
------
리액트의 라이프 사이클은 크게 3개의 경우에서, 역할에 맞게 배치 되어 있습니다.
- 생성될 때 (Mounting)
- 업데이트 할 때 (Updating)
- 제거 할때 (Unmounting)

![[react_lifecycle.jpeg]]

대표적인 생명주기들을 순서대로 나열해 봅시다.
1. constructor
2. render
4. componentDidMount
5. (fetch 완료 -> setState 실행)
6. shouldComponentUpdate
7. render
8. (setState 완료)
9. componentDidUpdate
10. componentWillUnmount


이때 React hooks 등장 이후로 함수형 컴포넌트에서는 hooks 을 활용 하여, 각 주기별로 아래와 같이 사용해야 합니다.

|분류|클래스형 컴포넌트|함수형 컴포넌트|
|------|---|---|
|Mounting|constructor()|() => { ...컴포넌트 내부... }|
|Mounting|render()|return()|
|Mounting|componentDidUpdate()|useEffect(() => {}, [x, y])|
|Updating|shouldComponentUpdate()|React.memo(컴포넌트)|
|Updating|render()|return()|
|Updating|componentDidUpdate()|useEffect(() => {}, [x, y])|
|UnMounting|componentWillUnmount()|useEffect(() => () => { ... })|

여기서 끝나면 안됩니다. hooks 와 관련해서 좀 더 짚고 가야할 것이 있습니다.
![[react_lifecycle_func.png]]

useEffect 와 나란히 눈에 들어오는 hooks 이 있습니다. 바로 useLayoutEffect() 입니다. 이 hooks 는 사실 크게 쓰일 일은 많이 없습니다. 하지만 특정 상황에 사용하면 큰 이점을 가져갈 수 있습니다. 이해하기 쉽게 2개의 hooks 를 비교해 봅시다.


**useLayoutEffect** 
- 브라우저가 페인팅(컴포넌트 랜더링) 하기 직전에 특정 행동을 수행할 때 사용합니다.
- 동기적으로 동작합니다.
	- 그러므로 네트워크 요청, 비동기 작업을 하는 경우에는 가급적 피하는 것이 좋습니다.
	- 비동기를 쓰면 안되는건 아니지만, 성능적 이슈가 발생합니다.
- 코드상으로 차이점은 React 내부적으로 componentUpdateQueue 에 들어가는 flag가 서로 다릅니다. 이 다른 flag 를 통해서 내부적으로 판단하는 것으로 보입니다.

**useEffect**
- 페인트(컴포넌트 렌더링) 이후에 실행됩니다.
- 비동기적으로 동작합니다.
	- 그러므로 네트워크 요청, 비동기 작업을 하는 경우에 유용합니다.
	- deps 에 특정 props, state 를 걸어주면 componentDidUpdate 역할도 수행합니다.
- 코드상으로 차이점은 React 내부적으로 componentUpdateQueue 에 들어가는 flag가 서로 다릅니다.

위와 같은 차이점들 때문에,
**useLayoutEffect**
- 페인팅 전 애니메이션 구현
- 특정 값에 의한 레이아웃 초기 위치값 설정
- 레이아웃 깜빡임
- 페인팅 성능측정 시

유용합니다. 그 외에 작업들에 대해서는 useEffect 를 활용하는 것이 좋습니다. 위 내용대로라면 사실 크게 사용할 일은 많이 없습니다. 상황에 맞춰 적절히 사용합시다.


<details>
<summary><b>참고자료</b></summary>
- https://velog.io/@ahsy92/%EA%B8%B0%EC%88%A0%EB%A9%B4%EC%A0%91-React%EC%9D%98-%EB%9D%BC%EC%9D%B4%ED%94%84-%EC%82%AC%EC%9D%B4%ED%81%B4-09q2s7uw
- https://velog.io/@hoi/React-Functional-Component%EC%9D%98-React.memo-%EC%82%AC%EC%9A%A9%EA%B8%B0
- https://all-dev-kang.tistory.com/entry/%EB%A6%AC%EC%95%A1%ED%8A%B8-useEffect%EC%99%80-useLayoutEffect-%EB%B9%84%EA%B5%90%EC%8B%9C%EB%A6%AC%EC%A6%88
- https://velog.io/@sunhwa508/useLayoutEffect%EB%8A%94-%EC%96%B8%EC%A0%9C-%EC%82%AC%EC%9A%A9%ED%95%A0%EA%B9%8C
</details>


## useMemo 는 어떤 hooks이고, 언제 사용하나요?
------
리액트는 다음과 같은 상황에서 리랜더링 됩니다.
- prod 가 바뀔 때
- state 가 바뀔 때

이때 useMemo 는 deps(의존성 배열)에 등록된 props, state가 
- 변경될 때만 변경된 props, state 을 연산한 새로운 값을 Return 하고
- 그렇지 않은 경우는 메모제이션된 값을 반환합니다.


덕분에 useMemo는 아래와 같은 장점이 있습니다.
- 불필요한 랜더링에 영향받지 않도록, 성능 최적화를 할 수 있습니다.

하지만 useMemo 는 단점이 있습니다.
- 남용하게 되면, 불필요한 메모리 사용을 증가시킵니다.
- useMemo의 상용법에 의해 코드를 늘리고, 가독성을 해치게 됩니다.


##### 사용하지 말아야 할 때
- 값 싼 연산을 할때, useMemo를 쓰지 않는것이 더 낫습니다.
- 컴포넌트 성능향상에 미미한 값변화, 차라리 React.memo()로 컴포넌트를 감싸는 것이 좋습니다
	- 하지만 memo() 는 업데이트 여부를 판단할 때, 얕은 복사를 합니다. props가 객체일 시 얕은 복사로는 한계가 있습니다. 중첩구조를 가진 props는 큰 효과를 기대하기 힘듭니다.
- 최적화가 필요한지 확신이 안설때는 하지마세요.
	- 차라리 최적화가 필요한 시점이 왔을 시, performance 측정을 통해 정확한 지점에 최적화를 해주는 것이 더 좋습니다.
- 종속 배열에 넣어준 props, state가 너무 자주 바뀌는 값이라면, useMemo를 쓰지 않는 것이 낫습니다.

##### 사용 해야 할 때
- 값 비싼 연산을 할 때, useMemo()를 사용하면 효과가 큽니다.
- 최적화를 꼭 useMemo로 할 필요는 없습니다. memo(), 디바운싱, 코드 분할, 지연 로딩 등등 적절한 요소에 적절한 최적화를 하는 것이 더 올바른 개발 형태일 것입니다.
- 자식 컴포넌트가 memo()로 최적화가 되어 있다면, 그에 넘겨주는 props, state 최적화를 고려해봅니다.
- 최적화가 필요할때, 의심 되는 부분을 performance 측정 후 최적화를 결정해도 늦지 않습니다.



<details>
<summary><b>참고자료</b></summary>
	<a href="https://javascript.plainenglish.io/stop-using-usememo-now-e5d07d2bbf70#9c12">
		# Stop Using useMemo Now!
	</a>
</details>

#### useCallback 는 어떤 hooks이고, 언제 사용하나요?
------
useMemo 와 달리 useCallback 은 deps(의존성 배열)에 등록된 props, state가 
- 변경될 때만 새로 들어온 callback을 새로 생성한 함수를 반환하고,
- 그렇지 않은 경우는 메모제이션된 callback 함수를 반환합니다.

여기서 중요한 점은 함수가 새것이냐, 이전것이냐 입니다. 동작 순서를 한번 정리해볼까요?

1. useCallback 에 넘겨진 callback 은 매 렌더링 마다 생성
2. deps(의존성 배열)에 넘겨진 이전값과 현재값 비교
3. 결과에 따라
	- 서로 다르면 -> 새로 생성한 callback
	- 같으면 -> 이전에 저장해둔 callback

자! 여기서 보면 무조건 callback 은 생성을 하게끔 되어 있습니다. 다만 비교에 의해 참조값을 새로 가져가냐, 그대로 가져가냐의 차이일 뿐입니다. 그러니 사실 useCallback 을 통해 함수 생성을 최적화 하는건 말이 안됩니다.
그래서, 간혹 글들을 보면 함수 생성을 불필요하게 하는 걸 막기위해, useCallback을 사용한다고 하는데, 이건 틀린 말입니다. 또한 함수의 생성은 실행보다 비용이 크지 않습니다.

##### 사용 해야 할 때
- 함수가 useEffect 의 deps(의존성 배열)의 요소일 때, useCallback 을 통해 최적화를 시켜줍니다.
- 자식 컴포넌트가 React.memo()로 최적화를 했고, 이 컴포넌트에게 함수가 전달될 때 최적화를 시켜줍니다.
- 이벤트 구독의 callback 으로 사용하는 함수를 선언했을 시, 렌더링으로 인해 참조값이 변경되지 않도록 useCallback()으로 감싸주는 것이 좋습니다.


<details>
<summary><b>참고자료</b></summary>
	<a href="https://velog.io/@woohm402/useCallback-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0">
		# useCallback 코드단 파보기
	</a>
	<a href="https://stackoverflow.com/a/71276243/10425435">
		# useCallback 사용해야 할 때
	</a>
</details>


#### Hydration 에 대해 설명
------
공식 문서는 hydrate 사용에 대해 아래와 같이 경고문구를 해놨습니다.

```
ReactDOM.render()를 사용해서 서버에서 렌더링한 컨테이너에 이벤트를 보충하는 것은 권장되지 않으며 React 17 버전에서 삭제될 예정입니다. hydrate() 를 사용해 주세요.
```

결국 쉽게 말하면
hydrate 란 이벤트 핸들러 함수들을 정적인 DOM 에 붙여서 동적으로 상호작용이 가능하도록 만들어 놓는 과정이다.

웹기술이 발전하면서, 과거 서버에서 매 요청마다 html을 만들어주는 형태에서 SPA 방식으로 발전하게 되었습니다. 
이런 CSR 방식은
- 최초 페이지 렌더링 성능
- SEO의 한계
등등과 같은 한계가 있습니다. 때문에 SSR을 통해서 CSR의 한계점을 극복하는 쪽으로 흘러갑니다. 하지만 SSR 시 최초 내려지는 HTML과 JS는 서로 연동이 되어있는 상황이 아닙니다. 그때의 HTML DOM은 이벤트 연동이 하나도 되지 않은 정적 파일입니다.

이 때 페이지 성능 측정에 사용되는 개념들이 TTFB, FCP, FMP, TTI 가 있습니다.

이때 TTI가 자바스크립트의 실행이 완료되어. 페이지가 상호작용 가능하게 될 때까지의 시간인데, HTML 코드에 JS 코드를 매칭시켜 동적인 이벤트가 가능하게 하는(=이벤트 리스너를 달아주는) hydration 개념이 나오게 된 것입니다.


React는 16v 이후 부터 render() 를 SSR 에 사용하는 것을 더이상 지원하지 않고, hydrate() 를 사용하도록 권장하고 있습니다.

둘의 차이점이 있습니다.
- ReactDOM.render(A, B, [cb])
	- A를 B에 하위 DOM요소로 주입하여 렌더링 처리
	- CSR 타이밍
- React.hydrate(A, B, [cb])
	- B에 A요소를 찾아서, 정해진 JS 이번트들만 부각시킨다.
	- SSR 타이밍

특히 React 18부터는 이 hydration 이 더 발전하여, Streaming HTML 과 선택적 Hydration 이 도입되었습니다.

Streaming HTML은 서버단에서 pipeToNodeWritable를 이용해 html 코드를 스트리밍 형식을 통해 작은 청크 형태로 나누어 보내줄수 있습니다.

선택적 Hydration 클라쪽에서는 Suspense 로 감싸는 것을 통해 부분적으로 hydrate 를 하도록 하여, 모든 페이지가 인터렉션하게 될 때 까지 기다리는 것이 아니라. 원하는 컴포넌트의 우선순위를 높게 하면 훨씬 더 빨리 제 역할을 할 수 있게 되었습니다.

이 모든 변화를 통해 개발자가 전략적으로 hydrate 를 설계할 수 있게 되었습니다.


#### Suspense 란 무엇이고, 왜 사용하고, 어떻게 사용하나요.
------
Suspens는 클라이언트 쪽에서 React 에게 컴포넌트가 읽어 드리고 있는 데이터가 아직 준비가 안되었음을 알려줄 수 있는 매커니즘 입니다. 즉 화면에 즉시 나타내주지 않아도 되는 컴포넌트나 관련 비동기 작업이 오래 걸린다고 하면, Suspense 를 고려해볼만 합니다.

Suspense 로 감싼 컴포넌트의 랜더링을 관련 비동기 작업이 끝날때까지 지연시키고, 그 동안 fallback에 정의된 컴포넌트를 보여줍니다. 그리고 비동기를 통한 데이터가 준비가 되면, 하위 컴포넌트를 랜더링 시켜줍니다.

```jsx
<Suspense fallback={<Spinner />}>
  <UserList />
</Suspense>
```


사실 Suspense 의 장점은 Suspense 만 가지고는 다 설명할 수 없다. Suspense만 본다면

- **컴포넌트 렌더링을 병렬적으로 처리할 수 있다**

이를 살펴보기위해, 어떻게 Suspense 가 비동기 처리를 하는지를 살펴볼 필요가 있다. 

먼저 데이터를 패칭해오는 아래의 코드를 보자
```js
function fetchUser(userId) {
  let user = null;
  const suspender = fetch(
    `https://jsonplaceholder.typicode.com/users/${userId}`
  )
    .then((response) => response.json())
    .then((data) => {
      setTimeout(() => {
        user = data;
      }, 3000);
    });
  return {
    read() {
      if (user === null) {
        throw suspender;
      } else {
        return user;
      }
    }
  };
}

export default fetchUser;
```

여기서 중요한 점
- 데이터를 fetching 중일때는 promise 를 반환한다.
- 데이터가 있으면 데이터를 반환한다.

그리고 랜더링 시점에 fetch 한 후 Suspense 로 비동기 대응을 하는 컴포넌트 코드를 보자.
```jsx
import { Suspense } from "react";
import User from "./User";
import fetchUser from "./fetchUser";

function Main() {
  return (
    <main>
      <h2>Suspense 사용</h2>
      <Suspense fallback={<p>사용자 정보 로딩중...</p>}>
        <User resource={fetchUser("1")} />
      </Suspense>
    </main>
  );
}

export default Main;
```

```jsx
import React, { Suspense } from "react";

function User({ resource }) {
  const user = resource.read();

  return (
    <div>
      <p>
        {user.name}({user.email}) 님이 작성한 글
      </p>
    </div>
  );
}

export default User;
```

여기서 중요한 점
- 비동기 처리 실행하는 영역이 Suspense 로 감싸져 있어야 한다.
- 감싸져 있는 영역에 비동기 처리 데이터가 promise 로 반환되면 fallback에 주입된 컴포넌트가 보여지게 된다.


결국 여기서 알수 있는 점은 Suspense 로 promise 가 감지 됐을 시, fallback 이 동작한다는 것 입니다. 그러기 위해서는 비동기 로직은 Suspense 로 감싸져있어야 하구요.

그렇다면, 이런 생각할수 있습니다. 컴포넌트를 랜더링 순서를 병렬적으로 처리할 수 있지 않을까.
맞습니다!


<details>
<summary><b>참고자료</b></summary>
	<a href="https://www.daleseo.com/react-suspense/">
		# Suspense 동작원리
	</a>
	<a href="https://happysisyphe.tistory.com/54">
		# 여러 비동기 작업을 Suspense 로 대응할 시 주의할 점
	</a>
</details>


#### HTML 스트리밍 이란 무엇이죠?
------
HTML 스트리밍이란 HTML을 더 작은 청크로 분할하고 이러한 청크를 서버에서 점진적으로 클라이언트에게 전송하는 과정을 의미합니다.

![[html_streaming.png]]

React v18 부터 지원되는 스팩입니다. 기존 SSR 방식은 아래 단계들을 순차적으로 진행합니다. 그리고 각 단계들은 블로킹적입니다.

- 데이터를 서버에서 가져온다
- 서버는 HTML 을 렌더링 한다
- 서버는 HTML, CSS, JS를 클라에 전송한다
- 클라는 HTML, CSS를 사용하여, 정적인 화면을 페인팅한다.
- 클라쪽 React가 UI와 JS 이벤트를 hydration 한다.

최종단계인 hydration이 완료되려면, 모든 컴포넌트 코드를 다운로드한 후에 UI와 상호작용 할 수 있죠. 이는 사용자에게 좋지 않는 경험을 줄 수 있습니다.


서버에서 스트리밍 형식으로 HTML을 전달하기 위해서는 아래와 같이 renderToPipeableStream() 을 사용해야합니다.
```jsx
// server.js
import { pipeToNodeStream } from 'react-dom/server'

export function render(res) {
  const data = createServerData()
  const { startWriting, abort } = pipeToNodeWritable(
    <DataProvider data={data}>
      <App assets={assets} />
    </DataProvider>,
    res,
    {
      onReadyToStream() {
        res.setHeader('Content-type', 'text/html')
        res.write('<!DOCTYPE html>')
        startWriting()
      },
    }
  )
}
```

![[js_chunk.png]]

이때, 클라측에서는 아래와 같이 Suspense 와 lazy로 선택적인 hydration 을 처리해야합니다. 무엇을 지연시킨 후 비동기 처리에 대비하고 무엇을 빠르게 처리할 지 정의해두는 것이죠.

```jsx
// app.js
import { Suspense, lazy } from 'react'
import Loader from './Loader'

const Comments = lazy(() => import('./Comments'))

function App() {
  return (
    <main>
      <Header />
      <Suspense fallback={<Loader />}>
        <Comments />
      </Suspense>
      <Footer />
    </main>
  )
}
```

```jsx
// index.js
import { hydrateRoot } from 'react-dom'
import App from './App'

hydrateRoot(document, <App />)
```


스트리밍은 각 컴포넌트를 청크로 간주할 간주 할 수 있습니다.  덕분에 선택적인 hydration 이 가능해 지는 것입니다.
- 우선순위가 높은 컴포넌트 먼저 전송
- 데이터에 의존하지 않는 컴포넌트 먼저 전송

때문에, 성능적인 면에서
- TTFB, FCP를 줄일 수 있다. 선택적으로 먼저 그려지는 컴포넌트들이 먼저 보일 것 입니다.
- TTI 도 개선될 수 있다. 선택적으로 hydration이 되기 때문에, 사용자경험이 향상 됩니다.


<details>
<summary><b>참고자료</b></summary>
	<a href="https://velog.io/@hamjw0122/Next.js-Hydration">
		# 모든 문제 해결의 종점 Streaming HTML & Selective Hydration
	</a>
	<a href="https://patterns-dev-kr.github.io/rendering-patterns/selective-hydration/">
		# 선택적 Hydration에 대하여
	</a>
</details>


#### React로 성능을 향상시킬 수 있는 방법들이 무엇이 있을까요.
------
React 를 사용한다는 것 자체가 성능향상에 어느정도 도움이 되는데요. 그 이유는 리엑트는 가상돔을 사용하기 때문입니다. 가상돔에 대해서는 추가 질문 주시면, 말씀드리도록 하고 지금은 그 외에 리엑트의 성능향상 방법을 말씀드리겠습니다.

useMemo(), useCallback(), React.memo()
- 메모제이션 기법으로 랜더링 성능을 향상시킬 수 있습니다.
useTransition(), useDeferredValue()
- state 나 props 와 관련된 랜더링 우선순위에 대해 지정해줘서 성능을 개선할 수 있습니다.
React.lazy()
- 코드 분할하고, 원하는 시점에 동적 import 를 함으로써, 번들을 나눠 불러올 수 있습니다.
Next.js SSR
- Next.js 를 활용하여, SSR 타이밍으로 첫 랜더링 성능 향상

사실 React로만 성능향상은 쉽지 않습니다. 프론트에서 해볼 수 있는 성능향상에 대해 더 이야기 해보겠습니다.



## useTransition(), useDeferredValue() 
------
React 18 부터 등장하는 hooks 들 입니다. 이 둘은 비슷한 면이 있으면서도 차이점이 있습니다.

공통점은
둘다 제어하고자 하는 state나 props의 변경을 우선순위를 낮추고자 하는데 사용합니다.

차이점은
useTransition() 은
- state 업데이트 코드인 함수를 wrapping 해서 사용합니다.
- 작업을 처리하고자 하는 컴포넌트 내부에 상태 업데이트 코드를 사용할 수 있는 경우 사용합니다.
```javascript
  const [isPending, startTransition] = useTransition();
  const [filterTerm, setFilterTerm] = useState('');
  const filteredProducts = filterProducts(filterTerm);

  function updateFilterHandler(event) {
    startTransition(() => {
      setFilterTerm(event.target.value);
    });
  }
```

useDeferredValue() 는
- props 또는 value를 wrapping 해서 사용합니다.
- state 업데이트를 직접 업데이트 처리할 수 없는 경우 사용합니다.
- 사용자의 입력이 완료되었다고 판단 시 새로운 값을 리턴합니다.
```javascript
function Product({ data }) {
  const deferredData = useDeferredValue(data);

  return (
    <div>
      {deferredData}
    </div>
  );
}
```


이때 다시 값을 뱉는 useDeferredValue 는 넘어온 datas 값의 동일여부만 관심있습니다. 즉 이 값의 변화여부에 따라 Componet의 리랜더링 여부를 제어하기 위해서는 useMemo() 함께 사용할 것을 권장하고 있습니다.

```javascript
function Product({ data }) {
  const deferredData = useDeferredValue(data);

  const memoedData = useMemo(() => {
	  <p>{deferredData}</p>
  }, [deferredData])

  return (
    <div>
      {memoedData}
    </div>
  );
}
```

이 둘은 결국 지정한 작업의 우선순위를 낮추고자 할때 사용하는 hooks 들 입니다. 시간이 오래 걸리는 작업에 적용하면 리액트가 작업 우선 순위에서 좀 더 느긋하게 처리합니다.
덕분에 더 중요한 작업을 먼저 처리하겠죠!


## useTransition()  Vs. useDebounce()
------
최신 hooks와 우리가 이전에 사용하던 useDebounce()와 useTrottle() 은 차이점이 있습니다.



#### TTFB, FCP, FMP, TTI는 무엇인가요
------

https://soojae.tistory.com/41#TTFB(Time%20to%20First%20Byte)-1



#### setState 동작에 대해 설명해주세요.
------

