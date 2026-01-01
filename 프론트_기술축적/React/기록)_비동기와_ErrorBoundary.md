
# 현상
최초 화면으로 진입했을 시, CORS 이슈로 API에서 401 오류가 발생했었다. CORS 는 API 서버단에서 구현하면 됐지만 이상한건 아래와 같이 Error 가 날 것 그대로 화면에 일시적으로 보이고 401 대응 화면으로 넘어가는 현상이였다.


![[20251219_image.png]]
(로컬에서 테스트 할땐, 일부러 vite 의 cors 설정을 끄고 403으로 테스트 했었다.)

ErrorBoundary 로 Error 처리를 했고, useQuery 로 api 호출하고 있으니 Error 자동으로 잡혀야 맞는거 아닌가? 라는 생각이 들었다.


# 원인파악
크롬의 네트워크에서 [stop recoring network log] 기능을 활용해서, 401 이 발생하는 API 를 하나씩 확인해 나갔다.

그중 auth, member 관련된 api 호출이 가장 먼저 발생했었는데, 이 호출은 Router 단에 auth 를 체크하는 Provider 에서 발생 하고 있었다.

``
```tsx
useLayoutEffect(() => {

const instance = keycloakAuthProvider.getInstance();

if (instance.didInitialize) {

checkAndSetUserInfo();

checkAndSetPermission();

setIsAuth(true);

}


return isAuth ? (

<AuthGlobalStatusProvider>

<Outlet />

</AuthGlobalStatusProvider>

) : (
<></>
)
```

이렇게 체크를 하고 있는 곳이 있었는데, checkAndSetPermission, checkAndSetUserInfo가 모두 비동기였다.

더 잘못된 것은 isAuth 를 통해서, render 여부를 판단하고 있었지만 앞선 비동기가 성공/실패 여부와 상관없이 isAuth 를 set 해주고 있었다.


## 오류의 원흉!
비동기에 대한 처리도 문제였지만, 오류의 원인은 checkAndSetPermission 에서 발생하고 있었다. checkAndSetPermission 에서 호출 하고 있는 특정 API 가 오류에 대한 try-catch 없이 처리되고 있었다.


## Errorboundary 는
다음과 같은 에러를 처리하지 못한다.

- Event handlers
- Asynchronous code (e.g. setTimeout or requestAnimationFrame callbacks)
- Server side rendering
- Errors thrown in the error boundary itself (rather than its children) (공식문서 참고)

그러니 useLayoutEffect() 내부에서 발생하는 오류도 처리하지 못한다.


## 그럼 useQuery 는?
useQuery 의 동작을 이해하기 위해 간단한 예제로 실험해볼 수 있다.

앞서 짚어봤듯이 아래 예제는 Errorboundary 가 잡지 못한다.

```tsx
export default function App() { return ( <ErrorBoundary> <AsyncChild /> <Button /> </ErrorBoundary> );} 
// 비동기 작업에서 에러가 발생하는 경우
const AsyncChild = () => { const fireError = () => { throw new Error("에러 발생"); } setTimeout(fireError, 100); return <div></div>;} 
// 이벤트 핸들러에서 에러가 발생하는 경우
const Button = () => { return ( <button onClick={() => { throw new Error("에러 발생"); }} > fireError </button> );}
```

비동기 작업, 이벤트 핸들러에서 에러 발생하는 경우 모두!! Errorboundary 는 잡지 못한다!

Errorboundary 는 렌더링 과정에서 하위에서 부터 전파(버블링) 된 에러를 잡아서, 처리한다.
특히 UI 이 그려지기 전, getDerivedStateFromError 를 먼저 호출하게 되는데, getDerivedStateFromError 를 사용해서 에러를 감지하고, 그 즉시 fallback 을 통해 후속 처리를 해준다.

하지만 비동기작업, 이벤트 핸들러 처리, 서버사이드 렌더링 등등은 모두 렌더링 외부에서 일어나는 작업들이다.

이런 에러들을 렌더링 과정으로 가지고 오려면 아래 예제처럼 하면 된다.

```tsx
// 비동기 작업에서 에러가 발생하는 경우
const AsyncChild = () => { const [isAsyncError, setIsAsyncError] = useState(false); if(isAsyncError) throw new Error("에러 발생"); const updateErrorState = () => { setIsAsyncError(true); } setTimeout(updateErrorState, 100); return <div></div>;} 
// 이벤트 핸들러에서 에러가 발생하는 경우
const Button = () => { const [isHandlerError, setIsHandlerError] = useState(false); if(isHandlerError) throw new Error("에러 발생"); return ( <button onClick={() => { setIsHandlerError(true); }} > click </button> );}
```

state, setState 로 에러발생에 대한 상태를 관리하여, render 하는 과정에서 throw Error() 를 해버리면, Error 가 상위 컴포넌트로 전파가 된다.

상태 변화로 렌더링을 일으키고, 에러가 그 과정에서 동기적으로 실행하게끔 변경한 탓이다.


# 이제 useQuery 는?
react-query 의 useQuery 는 위와 같은 과정이 내부에 내장 되어 있다. 하지만 아래와 같이 기본 옵션으로 사용하면, Error 전파 효과를 볼 수 없다

```tsx
export default function App() { return ( <ErrorBoundary> <Children /> </ErrorBoundary> );} const useExampleQuery = () => { return useQuery({ queryKey: ['example'], queryFn: getExample, });}; function Children() { const { data } = useExampleQuery(); return <div>{data}</div>;}
```

useQuery 내부에서 에러를 catch하는 로직이 포함되어 있기 때문이다. 따라서 에러는 상위로 전파되지 않는다.

```tsx
export default function App() { return ( <ErrorBoundary> <Children /> </ErrorBoundary> );} const useExampleQuery = () => { const queryResult = useQuery({ queryKey: ['example'], queryFn: getExample, }); if (queryResult.error) { throw queryResult.error; } return queryResult;}; function Children() { const { data } = useExampleQuery(); return <div>{data}</div>;}
```

아래와 같이 수정을 하면 되긴 한다
```tsx
export default function App() { return ( <ErrorBoundary> <Children /> </ErrorBoundary> );} const useExampleQuery = () => { const queryResult = useQuery({ queryKey: ['example'], queryFn: getExample, }); if (queryResult.error) { throw queryResult.error; } return queryResult;}; function Children() { const { data } = useExampleQuery(); return <div>{data}</div>;}
```

- useQuery는 요청이 실패할 경우 컴포넌트를 리렌더링
- useBaseQuery는 useState와 useSyncExternalStore를 사용하고 있어서 React 리렌더링 사이클을 탈 수 있다.
- 실패시 발생하는 리렌더링 과정에 queryResult.error는 존재한다

하지만! v4 이후로는 이런 옵션을 제공한다.
- v5 기준 throwOnError 옵션 (v5 에서 이름이 변경된 이유는 use 키워드는 react-query에 국한된 이름이고, react-query 는 tanstack-query 라는 범용적인 라이브러리로 제공되고 있기 때문에, 범용적인 이름으로 변경함 )
- v4 기준 useErrorBoundary 옵션

를 true 로 주면, 리렌더링 과정에서 Error 를 상위로 전파할 수 있다.


# 정리하자면
결국 위의 이슈는 두가지의 이유 때문에 발생했다.

- 비동기 처리가 제대로 되지 않았다.
- 비동기로 인한 에러 처리는 Errorboundary 전파되지 않는다.
	- 따로 처리해주거나
	- 리렌더링 과정을 일으켜, 렌더링 과정에서 동기적으로 에러를 발생해야한다.


# 처리는
두 가지 지점은 아래와 같이 대응했다.

- Promise.all 을 활용해서 두 비동기 작업중 하나라도 오류가 발생하면, isAuth 가 true가 되지 않고 error 에 맞는 처리를 하도록 한다.
- try-catch 로 처리를 해줬다. 아래와 같은 경우들을 고려는 해봤다.
	- 리렌더링 과정으로 빼서 Errorboundary 를 타도록 할 순 없을까?
	- 더 상위에서 공통적으로 처리할 순 없을까?
	- api interceptor 에서 처리하는 것이 맞지 않을까?

여러 전략적인 판단하에 try-catch 로 감싸고 이부분을 공통로직으로 뺐다.