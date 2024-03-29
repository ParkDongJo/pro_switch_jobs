## Flex 패턴 / Atomic 패턴 / Proxy 패턴 으로 나눈 store

전역 상태를 관리하는 라이브러리도 다양해지면서, 
- 클라 State를 관리하는 Store
- 서버 State를 관리하는 Store

로 나뉠 수 있다. 우선 클라 State 를 기준으로 Store 를 분류해보겠다.
대표적인 Store 들을 구조를 기준으로 분류를 해보겠다.

대표적인 패턴으로는
- Flux 패턴
- Atomic 패턴
- 구독, 중개 패턴

등등이 있다.
여기서 모든걸 다 설명하기 보다는 내가 사용해본 대표적인 라이브러리들 위주로 설명해보겠다.
분류별로 가볍게 살펴보자면,

##### Flux 패턴
우선 Flux 패턴은 데이터 변경되기까지 
- 발생한 이벤트를 1차로 가공하는 Action, 
- Action을 Store 로 전달하는 Dispatch,
- 전역상태의 최종 저장소 Store
- 사용자에게 변경된 데이터가 보여질 View 

까지 이어지는 순서로 진행 되게끔 구성된 패턴이다.

Flux 패턴은 Redux, Zustnad 가 해당된다.

이 두 라이브러리가 Flux 패턴을 그대로 재현해 낸 것은 아니고 약간의 응용이 들어가 있다. 자세한 내용은 뒤에서 설명해보겠다.

##### Atomic 패턴
그리고 Atomic 패턴은 상태를 원자 단위처럼 작은 단위로 보고, 나누어 관리하는 방식이다. Amotic 이라는 '원자적' 이라는 의미에서도 알 수 있듯이 원자단위의 상태를 업데이트하는 구조이다.
가장 대표적인 라이브러리가 Recoil 이다 Jotai 는 Zustand 의 창시자가 Recoil 에 영감을 받아 만든 후속작이다.

Recoil 이 대표적으로 Context API 와 많이 비교되는데, Context API 는 여러 Provider 로 중첩해서 사용하는 형태인데, 이는 불필요한 리랜더링을 일으킬 수 있다.
하지만 원소적 단위로 상태를 나눈 Recoil 은 2차원적인 접근이 아닌 3차원적으로 직교하는 형태로 상태를 관리하기 때문에 불필요한 렌더링 없이 단위별로 업데트와 구독이 가능하다.

Atomic 패턴으로 Recoil 말도도 Jotai 가 있다. 나도 가볍게 Jotai 예시들을 봤지만 훨씬 사용법과 개념이 간단해서 요즘은 Recoil 보다 더 인기가 있어 보인다.


##### 옵저버, 중개 패턴
그리고 이 패턴에는 과거 Redux 의 쌍두 마차였던 Mobx 와 최근 떠오르고 있는 중개패턴 Valtio 가 있다. 특히 Valtio 의 사용법이 굉장히 심플하고 재미있는데, proxy 라는 API 로 객체를 만들어서 관리한다.

이 상태를 답고 있는 객체는 Valtio에서 제공되는 subscribe 라는 API를 통해, 컴포넌트 외부에서도 구독할 수 있게끔 해준다.
중개 패턴이라고 하기보다는 오히려 반응형 패턴이라고 봐야하는데, 그래서 Mobx 와 엮어서 분류한 것도 있다.


##### 장단점 따져보기
이 모든 라이브러리를 다 비교하기 보다는, 제가 선호하는 기준을 먼저 세워보겠다.

- 일단 사용법이 간단 해야한다.
- 프로젝트 크기에 따라
	- 프로젝트가 크면  라이브러리 사용 시 어느정도 규격이 있어야 한다.
	- 프로젝트가 작으면 라이브러리 사용이 유연했으면 좋겠다.
- 그 외에
	- 안정성
	- 참고자료
	- React 업데이트에 따른 팔로잉

사용 법이 간단해야한다는 점에서
- Flux 패턴은 Zustand
- Atomic 패턴은 Recoil, Jotai
- 중개 패턴은 Valtio 이다

그리고 프로젝트 크기에 따라서는 아래와 같이 조합할 것 같다
- 대 : Zustand + (React-Query)
- 중 : Jotai + (React-Query)
- 소 : valtio

로 나눌 것 같다. 이유는 아래와 같다. 프로젝트 규모가 클수록 사용법에 어느정도 규격이 있는 것이 좋다고 생각한다. 심지어 Zustand 는 Redux 보다 사용법이 더 간단한다. Redux-toolkit 보다 더 간단하다.

간단하면서도, 스토어의 구독 변경에 대해 통제를 한다. Zustand는 상태 관리를 내부적으로 클로저를 활용한다. 그리고 이 상태를 구독, 변경 하는 것에 대한 기능을 createStore 를 할때 제공한다.

덕분에, 내가 state 를 변경하고자 하면, 이 기능들을 통해서만 변경과 사용이 가능하다. 물론 좀 더 내부적으로 파고들면 react 에서 제공하는 externel sycn store API를 사용하지만, 일단 여기서는 논외로 하자

또한 Provider 를 랩핑하지 않아서, 불필요한 랜더링도 최소화 했고, 용량이 1.1kb 밖에 안된다는건 덤이다.


그리고 규모면에서 큰 프로젝트가 아니라면, 나는 Atomic 패턴 라이브러리를 선택할 것 같다.
이유는 사용이 유연하다는 장점 덕분이다. 우리가 사실 클라와 서버로 상태를 나누게 되면, 클라쪽 상태는 예전만큼 많지 않다.

특히 나는 전역 스토어의 사용을 미루고 미루다가 사용하는 편이다. 편리하지만 무분별하게 사용했을 시 코드의 추적이다 데이터의 흐름을 읽기가 쉽지 않기 때문이다.

그래서 중간 규모로 유연한 사용이 가능한 Atomic 패턴 라이브러리를 선정했다. 이들은 특히 React의 새버전의 화두인 동시성도 고려해서 만들어 졌다는 것도 흥미를 끈다 (물론 내부 코드를 살펴본 적은 없다...)

그 중에서도 사용해본 recoil 보다는 jotai를 선택했다.

이유는 jotai 가 좀더 간단하고 유연하다고 느꼈기 때문이다. 또한 Recoil의 업데이트는 세월아 내월아 이며, 여전히 베타버전으로 남아있다.
특히 Recoil 은 막상 써보면, 비동기 처리를 위한 Selector 사용이 그리 깔끔하지 않다고 느껴진다.

Jotai 는 순수 atom 과 파생된 atom 까지도 상태관리가 가능하며, Provider 마저도 범위를 다르게 지정해서 각자의 고유의 값을 지닐 수 있도록 설계할 수 있다. 난 이 지점이 특히 마음에 들었는데, 이 덕분에 큰 관심사 묶음을 Provider 단위로 나눌 수 있겠구나 라는 생각이 든다. 마치 향상된 Context API로 느껴졌다.


그리고 마지막으로
작은 규모면에서는 발티오를 뽑았다. 물론 사용해본적은 없지만 기본 사용법을 봤을때 정말 소규모 프로젝트에서는 발빠르게 구현할 수 있겠다 라는 생각을 했다.
만약 내가 사용한다면, 이벤트 페이지에서 전역 상태가 필요하다 한다면 발티오를 한번 사용해볼 법도 하다. 하지만 React 18 이상에서 Suspense 를 활용해야한다면 소규모라도 Jotai 를 선택하거나 아예 도입하지 않을 수도 있을 것 같다.

이 부분은 아직 깊게 생각해보진 않아서 더 살펴봐야할 것 같다.

#### React 의 향후 트랜드를 고려했을 시
Jotai 가 괜찮아 보인다. (아직 고민중)

- Server Component 가 향후 방향이라면
	- 전역 store 의 역할 의 축소
	- 더 사용하기 간단한 Atomic 패턴이 유리
- 동시성에 대한 고민
	- tearing 이라는 이슈
	- jotai 는 이에 대한 접근을 동시성이 더 중요하다고 말하고 있음

https://yozm.wishket.com/magazine/detail/2233/
https://careerly.co.kr/comments/73197



## Flux 패턴 vs. Atomic 패턴
----
### Flux 패턴
양방향 데이터 흐름의 단점을 보완하고자 나온 단방향 데이터 흐름에서 컴포넌트의 깊이가 깊어짐에 따라 **Props Drilling** 이라는 문제가 발생했다.

이 문제를 해결하고자 Flux 패턴이 제시 된다. Flux 패턴은 단방향 흐름을 유지하면서도 기존 양방향 바인딩의 MVC 패턴의 단점을 보완한다. 

Action - Dispatcher - Store - View 로 이뤄진 구성으로 각각 구분되어 있다. 

Action - Data는 Action 을 통해서 운반되며 
Dispatcher - 이런 Action 을 전달받아서 Store 로 전달한다.
Store - 전달된 Action 을 통해, 상태가 변경되고 상태가 변경되면 실행되어야 할 로직을 실행시켜, 해당 상태와 연계된 View 에 반영시킨다.
View - Store 를 구독하고 있다가 상태가 변경될 때마다 새로운 상태를 조회하고 이를 반영한다


### Atomic 패턴
Flux 패턴은 데이터의 흐름을 담당하는 각각의 모듈들이 분리되어 있다. 단 이런 구조 때문에

- 러닝커브가 발생
- 장황한 문법

등등이 단점이 될 수 있다. 이에 대해 반대하여, 상태를 원자 단위처럼 작은 단위로 보고 나누어 관리하는 방식인 Atomic 패턴이 제시된다.

문법이 간단하고, selector 라는 파생된 데이터라는 개념을 제공하기 때문에 다른 atom이나 selector를 받아서 가공하는 상태 정의도 가능하다.


https://hyunee-p.tistory.com/133


## Zustand
-----
개인적으로 zustand 의 사용이 제일 마음에 드는데, 그 중 하나는 사용법 때문이다. recoil 를 사용해본 경험이 있는데, atom 방식이 매우 간단하지만, 프로젝트 단위가 커질 수록 관리가 힘들었다.

관리가 힘들어진 원인을 개인적으로는 2가지로 삼는데
- 관심사의 가장 작은 단위가 atom이다 보니, 이 atom 이 늘어날 수록 관리가 어려워진다.
- state와 액션이 분리되어 있다. 

```javascript
function useCounter() {
	const [count, setCount] = useRecoilState(counterState);

	const increment = () => setCount(count + 1);
	const decrement = () => setCount(count - 1);

	return { count, increment, decrement, }; 
}
```
그렇다 보니 리코일은 위와 같이, 커스텀 훅스로 따로 뺄때도 있지만, store 만을 위해 hooks를 따로 뺀다는게 영 깔끔하지 못했다.

```javascript
import create from 'zustand'; // 선언

const useCounter = create((set) => ({ 
	count: 0, 
	increment: () => set(state => ({ count: state.count + 1 })), 
	decrement: () => set(state => ({ count: state.count - 1 })) 
})); // 사용 

const { count, increment, decrement } = useCounter();
```

하지만 zustand 는 액션기반 상태관리 라서, store 선언 시 관련 액션들을 함께 정의해주는 식으로 해도 된다.

만약 기존 state 기반으로 간단한 파생데이터를 가져오고자 한다면, get 으로 가져오게 정의하면 된다.
```javascript
const useStore = create((set) => ({ 
	first: 'Daishi', 
	last: 'Kato', 
	get fullName() { 
		return `${this.first} ${this.last}`; 
	}, 
}));
```

또한 내가 외부로 공개하고 싶은 함수기반으로 커스텀을 할 수도 있어서, 밖으로는 닫고 안으로는 캡슐화를 달성할 수도 있다.

이러한 사용법 덕분에, 큰 프로젝트일 수록 zustand 와 잘 어울린다고 생각한다.

그 외에
- Redux-devtool 을 함께 사용할 수 있다는 점
- 미들웨어로 불변성(immer), persist(storage 사용) 등등을 함께 적용할 수 있다는점
- 원한다면 일시적 업데이트로 store 의 업데이트로 인한 리랜더링을 제어 할 수 있다는 점
	- subscribe 를 사용하여 가능함
```javascript
// Zustand store를 생성합니다.
// 이 store에는 count 상태와 이 상태를 변경하는 set 함수가 포함됩니다.
const [useStore, api] = create((set) => ({
	count: 0,
	set: (fn) => set(produce(fn))
}));

const inputRef = useRef(null);

useEffect(() => {
	// 상태의 변경을 구독하고, count가 바뀔 때마다 input 값 업데이트
	const unsubscribe = api.subscribe(
		(count) => {
			if (inputRef.current) {
				inputRef.current.value = count;
			}
		},
		(state) => state.count
	);
	// 컴포넌트가 언마운트될 때 구독을 해제합니다.
	return unsubscribe;
}, []);
```


https://arc.net/l/quote/pkuaoqmn
https://yozm.wishket.com/magazine/detail/2233/
https://arc.net/l/quote/idcaeqgp
https://ui.toast.com/posts/ko_20210812
https://velog.io/@wns450/Context-API-%EC%97%86%EB%8A%94-%EC%83%81%ED%83%9C%EA%B4%80%EB%A6%AC-Zustand

제일 읽기 좋은 자료
https://ui.toast.com/weekly-pick/ko_20210812



## Context API
----
내가 전역 Store 에서 Context API를 따로 뺀 이유가 있다. Context API는 개인적으로 활용도가 다르다. 소수의 컴포넌트 단위에서만 state 를 공유하고자 할때, 전역 스토어가 필요하다면 이때 Context API를 고려해본다. 

이렇게 하면 state 의 관심사 분리도 자연스럽게 되면서,
전역 state의 사용 범위를 지정할 수 있기 때문이다.

하나의 프로젝트에서 서로 다른 파트가 일을 하게 될 때 유용하다고 생각이 든다. 또는 여러 컴포넌트가 함께 특정 state에 종속 되어 있을때도 유용할 것으로 본다.


다만 Context API의 큰 단점이 있는데, 바로 Provider 가 서로를 감싸는 코드 영역이 늘어나면 날수록 불필요한 리랜더링이 일어날 수 있다는 점이다.
아래와 같이 말이다.


```jsx
<ProviderA>
	<ProviderB>
	</ProviderB>
</ProviderA>

```

하지만 사실 Jotai 로 되어 있는 프로젝트라고 한다면 이 조차도 고려대상이 아니긴 하다.
jotai 는 Provider 로 여러곳에 store 범위를 지정해둘 수 있기 때문이다.






## useSyncExternalStore
----
**_useSyncExternalStore_** 는 **external store(외부 저장소)** 를 구독할 수 있게 해주는 **React Hook**이다.
여기서

external store 는 redux, zustand, jotai
internal store 는 props, state, useState, useReduce

로 분류된다.
React 18 부터는 동시성 렌더링이 등장하면서, useTransition 와 useDeferredValue 같은 새로운 hooks 들이 나왔다. 이들 덕분에 작업의 우선순위를 개발자가 지정할 수 있게 되었다.

이때 React 는 렌더링 중에 우선순위가 떨어지는 작업을 잠깐 멈추고 급한 일 부터 처리를 한다. 문제는 여기서부터 시작된다.

##### tearing
지금 부터 이야기할 문제는 tearing 이라는 문제이다. tearing 이 찢다는 의미를 가지고 있는데, 여기서는 시각적으로 UI 가 state의 불일치로 인해 2개 이상의 상태로 보여지는 현상을 말한다.
그림으로 아래의 상황은 동기적인 렌더링 과정이고 정상적인 상황이다.
![[external_store_1.png]]


![[react18_tearing_2.png]]
이는 React 18 이전에는 별 문제가 되지 않았다. 모든 렌더링이 끝나고 나서, Store 가 끝나니까 말이다.


![[react18_tearing_1.png]]
하지만, 이젠 렌더링 도중에 사용자가 값을 변경했고 이것이 Store 의 값이라면, 렌더링 도중에 Store 값이 바뀌고 다시 렌더링이 일어났을 시 남은 UI 업데이트에 대해서는 변경된 Store 값을 가져와서 보여줄 수 있다.

아래 그림처럼 UI 가 아래와 같이 UI가 따로따로 놀게 된다. 이것은 의도하는 바가 아니다.
![[react18_tearing_3.png]]

이는 사실 동시성과 여러 컴포넌트에 영향을 미치는 전역 Store 가 결합되면서 생기는 문제이다. React 는 이러한 문제를 해결하기 위해 바로 useSyncExternalStore 라는 hooks 를 지원한 것이다.

내부 코드를 들여다보지는 않았지만, Sync 라는 용어를 보면 내부적으로 동기식으로 변경을 시키는듯 하다.

###### 적용
대부분의 Store 라이브러리들은 해당 hooks 를 활용하여 각자만의 방법으로 대응해둔것 같습니다. 다 찾아보진 않았지만, 몇몇 단서들을 토대로 판단했습니다.

recoil
https://github.com/facebookexperimental/Recoil/pull/2001
zustand
https://github.com/pmndrs/zustand/blob/0058a03b78bf628a27fb3cb4b6ebe3e6263dd0a0/src/react.ts#L62
react-query
https://github.com/TanStack/query/pull/3064/files#diff-b281d910f1fda02068add8ccf8b966ab625052d7314962c3a22f9eea5da5cbf3R81

하지만 이것은 여전히 논란이 있어보입니다. 제가 그렇게 느끼는 것은 jotai 의 입장이 다르기 때문입니다.


##### Jotai 는 useSyncExternalStore 를 지원하지 않는다.
https://github.com/pmndrs/jotai/discussions/2137 깃헙 discussions 에 보면 useSyncExternalStore에 대한 논의가 있습니다. 요구사항은 분명 있었던것 같습니다.

하지만 Jotai 를 만든 카토 다이쉬는 자신의 입장을 분명히 밝혔습니다. 조타이의 주장은 그것보다 더 중요한 Suspense 와 동시 렌더링과 완벽한 호환을 지향해야한다 입니다.

useSyncExternalStore 는 그 자체로 동기식인데, 이는 타임 슬라이싱(즉 비동기식) 과 전혀 호환되지 않는 방식이라고 합니다.

그리고 tearing 을 아주 일시적인 현상으로 여기는것 같습니다. 그래서 그런 tearing 보다 더 중요하다고 여기는 비동기식 지원을 더 완벽히 하기 위한 조치인것으로 보입니다.

게다가 짧게만 언급했지만. useReducer 를 통해 내부적으로 해결을 하는 듯한 언급만 남기고 더 자세히 언급하진 않았다
하지만 Jotai 는 처음에는 useMutableSource(useSyncExternalStore의 이전 버전) 를 고려를 했었고, 이 tearing 에 대하여 꽤 많은 테스트를 했던것으로 보인다.
https://github.com/dai-shi/will-this-react-global-state-work-in-concurrent-rendering


jotai
https://dev.to/dai_shi/why-usesyncexternalstore-is-not-used-in-jotai-23h9
https://github.com/pmndrs/jotai/discussions/2137
https://blog.hwahae.co.kr/all/tech/tech-tech/6099

zustand
https://medium.com/dong-gle/%EC%9D%B4-%EA%B8%80%EC%9D%80-usesyncexternalstore%EB%A5%BC-%EC%9D%B4%EB%AF%B8-%EC%95%8C%EA%B3%A0-%EC%9E%88%EB%8B%A4%EB%8A%94-%EA%B0%80%EC%A0%95-%ED%95%98%EC%97%90-%EC%9E%91%EC%84%B1%EB%90%98%EC%97%88%EC%8A%B5%EB%8B%88%EB%8B%A4-460637179d0d




## React Query
----




## Composition event 란
-----
![[store_react_composition.png]]


https://arc.net/l/quote/akdzqexu



## 상태 분류
---
- 지역 state vs. 전역 state vs. 컴포넌트 간 state
- 클라이언트 state vs 서버 state