
**목차**
- [리액트 라이프사이클에 대해]("#리액트 라이프사이클에 대해")
##### 리액트 라이프사이클에 대해
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
- 브라우저가 페인팅 하기 전에 특정 행동을 수행할 때 사용합니다.
- 동기적으로 동작합니다.
	- 그러므로 네트워크 요청, 비동기 작업을 하는 경우에는 가급적 피하는 것이 좋습니다.
	- 비동기를 쓰면 안되는건 아니지만, 성능적 이슈가 발생합니다.
- 코드상으로 차이점은 React 내부적으로 componentUpdateQueue 에 들어가는 flag가 서로 다릅니다. 이 다른 flag 를 통해서 내부적으로 판단하는 것으로 보입니다.

**useEffect**
d












