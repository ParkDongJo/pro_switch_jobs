
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

특히 React 18부터는 이 hydration 이 더 발전하여, 점진적인 Hydration 이 도입되었습니다.

v18에서부터는 서버단에서 pipeToNodeWritable를 이용해 html 코드를 스트리밍 형식을 통해 작은 청크 형태로 나누어 보내줄수 있습니다.
또한 클라쪽에서는 Suspense 로 감싸는 것을 통해 부분적으로 hydrate 를 하도록 하여, 모든 페이지가 인터렉션하게 될 때 까지 기다리는 것이 아니라. 원하는 컴포넌트의 우선순위를 높게 하면 훨씬 더 빨리 제 역할을 할 수 있게 되었습니다.

이 모든 변화를 통해 개발자가 전략적으로 hydrate 를 설계할 수 있게 되었습니다.


#### Suspense 란 무엇이고, 왜 사용하고, 어떻게 사용하나요.
------


#### React로 성능을 향상시킬 수 있는 방법들이 무엇이 있을까요.
------



#### TTFB, FCP,
------



#### setState 동작에 대해 설명해주세요.
------

