
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


#####



