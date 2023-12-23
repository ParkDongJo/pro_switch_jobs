## Flex 패턴 / Atomic 패턴으로 나눈 store

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


https://yozm.wishket.com/magazine/detail/2233/
https://careerly.co.kr/comments/73197



## Zustand
-----



https://arc.net/l/quote/idcaeqgp
https://ui.toast.com/posts/ko_20210812


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






## useSyncExternalStore
----




https://ingg.dev/zustand-work/#use-sync-external-store-work


jotai
https://dev.to/dai_shi/why-usesyncexternalstore-is-not-used-in-jotai-23h9
https://github.com/pmndrs/jotai/discussions/2137


recoil
https://recoiljs.org/blog/
https://github.com/facebookexperimental/Recoil/pull/2001


zustand
https://medium.com/dong-gle/%EC%9D%B4-%EA%B8%80%EC%9D%80-usesyncexternalstore%EB%A5%BC-%EC%9D%B4%EB%AF%B8-%EC%95%8C%EA%B3%A0-%EC%9E%88%EB%8B%A4%EB%8A%94-%EA%B0%80%EC%A0%95-%ED%95%98%EC%97%90-%EC%9E%91%EC%84%B1%EB%90%98%EC%97%88%EC%8A%B5%EB%8B%88%EB%8B%A4-460637179d0d

https://github.com/pmndrs/zustand/blob/0058a03b78bf628a27fb3cb4b6ebe3e6263dd0a0/src/react.ts#L62


react-query
https://github.com/TanStack/query/pull/3064/files#diff-b281d910f1fda02068add8ccf8b966ab625052d7314962c3a22f9eea5da5cbf3R81




## React Query