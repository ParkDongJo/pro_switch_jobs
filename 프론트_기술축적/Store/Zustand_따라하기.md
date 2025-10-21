
# 기본 컨셉
클로저 활용, React 의 useSyncExternalStore 활용


# create 구조
크게 3개의 구조로 나뉘는것 같다. 1. 클로저를 활용한 내부 상태들, 2. store 의 기본 api들, 3. useSyncExternalStore 에 넘겨질 subscribe 

```
내부 state
내부 listeners

// store 의 기본 api 들
setState
getState

// store 의 그 외 api (서버쪽에서 사용될 api 로 보임)
getInitialState

// React의 useSyncExternalStore에 넘겨질 subscribe
subscribe
```

특히 subscribe 는 React 의 useSyncExternalStore에 에서 아래와 같이 사용된다.

> useSyncExternalStore 는 React 에서 아래의 경로를 따라가면 된다. 19v 기준 
>  packages/use-sync-external-sotre/src/useSyncExternalStore.js
>  > packages/use-sync-external-sotre/src/useSysncExternalStoreShim.js
>  >  > packages/use-sync-external-sotre/src/useSyncExternalStoreShimClient.js

```javascript
export function useSyncExternalStore<T>(
	getSnapshot: () => T,
	getServerSnapshot?: () => T,
): T {

...생략...

	useLayoutEffect(() => {
		inst.value = value;
		inst.getSnapshot = getSnapshot;
		
		if (checkIfSnapshotChanged(inst)) {
			forceUpdate({inst});
		}
	}, [subscribe, value, getSnapshot]);
	
	useEffect(() => {
		if (checkIfSnapshotChanged(inst)) {
			forceUpdate({inst});
		}
		const handleStoreChange = () => {
			if (checkIfSnapshotChanged(inst)) {
				forceUpdate({inst});
			}
		};
		return subscribe(handleStoreChange);
	}, [subscribe]);

...생략...

}
```

subscribe 가 변경점이 있을때 마다, 내부 useLayoutEffect, useEffect 를 통해서 트리거 되고 변경점이 있다라고 판단될 때, forceUpdate 를 실행한다.

zustand 에서 subscribe 는 아래와 같이 작성되어 있다.
```javascript
const subscribe: StoreApi<TState>['subscribe'] = (listener) => {
	listeners.add(listener)
	return () => listeners.delete(listener)
}
```

zustand 내부의 listeners 에 listener 를 등록하는 함수이다. 


# 주요 API

## setState
zustand의 setState 는 partial 라는 콜벡함수 또는 객체를 받는다. 이는 다음 변경될 state 에 대한 데이터 일 것이다.

```javascript
(set) => set((state) => state + 1);

// 또는

(set) => set({ state: 0 });
```

set 함수로 넘어오는 함수와 객체의 예제이다.

그리고 기존에 클로저로 가지고 있던 state 와 새롭게 받은 nextState 를 비교해서 서로 다르면, state 와 nextState 를 listener 에 넘긴다.


#### listener 는 뭐야?
listener 는 상태가 변경되면 실행되는 함수이다. 위에서 설명했듯 이전 상태와 그 다음 상태를 함께 받는다.
listener는 useSyncExternalStore에 넘겨질 subscribe에서 미리 받아놓는 함수이다.

그렇다면 도대체 subscribe에는 어떤 형태의 listener 가 넘어올까?

그건 React 의 useSyncExternalStore 에 그 답이 있다. 위에서 기재해둔 코드의 일부를 다시 가져오겠다.

```javascript
// useSyncExternalStore 의 내부

	useEffect(() => {
		if (checkIfSnapshotChanged(inst)) {
			forceUpdate({inst});
		}
		const handleStoreChange = () => {
			if (checkIfSnapshotChanged(inst)) {
				forceUpdate({inst});
			}
		};
		return subscribe(handleStoreChange);
	}, [subscribe]);
```

subscribe 에 넘겨지는 listener 는 handleStoreChange 이고 checkIfSnapshotChanged 가 true 일때, React의 강제 업데이트 forceUpdate() 를 실행한다.
아마 이전의 snapshot 과 다른 경우, 강제 상태 업데이트를 시키는 코드로 보인다.

하지만 여기서 또 이상한 점이 있다.

zustand 에서 listener를 호출할때 현재 상태와 다음 상태를 함께 넘겼다. 그런데 listener 인 handleStoreChange 에는 아무런 매개변수를 받고 있지 않고 있다?!!

추측컨데, zustand 를 꼭 react 에서 사용하는 경우가 아닌, vanilla script 에서도 사용할 수 있도록 좀 
더 넒은 범위의 상황을 고려한 거 아닐까? 생각이 든다.


# 직접 설계해본다면?
zustand 를 직접 살펴본 결과 아래 과정을 따라 store 를 설계하면 될것 같다.

- React 의 useSyncExternalStore 를 통해 store 를 생성한다.
- useSyncExternalStore 에 넘길 두가지 api 가 필요하다
	- listener 를 넘길 subscribe 
	- 현재의 snapshot 을 보낼 getState
- api 는 모듈화 한다. 이때 내부적으로는 아래와 같은 api 가 더 필요하다.
	- state를 저장해둘 private 상태
	- listener를 저장해둘 listeners Set 자료구조
	- 새 state 를 반영할 setState
		- 내부적으로는 listener 를 호출해야한다.


# 의문제기
zustand 의 setState 를 호출 할때 listener 도 함께 호출 된다. 이때 변경되는 state 에 대한 listener 만 호출 되는 것이 아니라 listeners 에 있는 모든 listener 가 호출된다.

그러면 매번 setState를 호출할 때마다

이런 구조를 좀 더 효율적으로 변경할 수 있지 않을까?

# 의문해소
zustand 는 Provider 를 따로 제공하고 있지 않다. v18 이후 부터 제공되는 React 를 useSyncExternalStore 를 통해서 React 의 상태를 업데이트 한다.

즉 create 로 생성되는 store 는 각자 독립적인 클로저 상태를 가지고 있을 것이다. 하나의 store 에 수많은 상태를 선언해두지 않는 이상 반복문으로 listner 를 호출하는 것은 크게 문제가 되지 않을 것이다.