먼저  React-Query v5 에서 풀고자 하는 바는 크게는 아래와 같습니다.

- 번들사이즈 20퍼센트가 줄었음
- useQuery 에 대한 공통사용을 통합함
- DX를 좀 더 직관적으로 만들려고 노력하였음
	- cacheTime -> gcTime
	- keepPreviousData + placeholderData
	- 공유가능한 mutation state
- suspense 의 안정화
	- suspense 지원 Hooks
	- enabled, placeholderData, throwOnError 속성 비지원
	- QueryErrorResetBoundary 지원
	- 실험적인 기능들
		- 서버에서 Suspense와 함께 사용 가능 (next.js)
		- useQuery().promise

이 내용은 1년 전 로드맵을 통해서 차근차근 준비했습니다.
https://github.com/TanStack/query/discussions/4252


로드맵의 모든 내용이 v5에 들어간 것 같지는 않습니다.

공식 깃헙에 5v 마이그레이션 문서를 보면
https://github.com/ssi02014/react-query-tutorial/blob/main/document/v5.md

23가지의 변경사항들이 있습니다.
그 중 저희 FMS 는 대략적으로 9가지 변경사항들이 우리 프로젝트와 직접적으로 연관이 있습니다.

위 문서를 기준으로 나열해보면

1. 1) ⭐️ Supports a single signature, one object
2. 2) ⭐️ 'queryClient.getQueryData', 'queryClient.getQueryState' now accepts queryKey only as an Argume
3. 3) ⭐️ Callbacks on useQuery (and QueryObserver) have been removed
4. 6) ⭐️ Rename 'cacheTime' to 'gcTime'
5. 7) ⭐️ The 'useErrorBoundary' option has been renamed to 'throwOnError'
6. 9) ⭐️ Removed 'keepPreviousData' in favor of 'placeholderData' identity function
7. 13) ⭐️ infinite queries now need a 'initialPageParam'
8. 17) ⭐️ 'status: loading' has been changed to 'status: pending' and 'isLoading' has been changed to 'isPending' and 'isInitialLoading' has now been renamed to 'isLoading'
9. 21) ⭐️ new hooks for suspense


아래 2 사항은 해당은 안되지만 알아두면, 적용이 가능할 것들 입니다.

1. 18) Simplified optimistic updates
2. 20) new 'combine' option for 'useQueries' 


우선 위의 내용은 더 큰 카테고리로 그룹화가 가능합니다

# 그룹화
Query 사용법 변경됨
1. 1) ⭐️ Supports a single signature, one object
2. 2) ⭐️ 'queryClient.getQueryData', 'queryClient.getQueryState' now accepts queryKey only as an Argume
3. 3) ⭐️ Callbacks on useQuery (and QueryObserver) have been removed

Query options이 변경됨
4. 6) ⭐️ Rename 'cacheTime' to 'gcTime'
5. 7) ⭐️ The 'useErrorBoundary' option has been renamed to 'throwOnError'
6. 9) ⭐️ Removed 'keepPreviousData' in favor of 'placeholderData' identity 
7. 13) ⭐️ infinite queries now need a 'initialPageParam'

Query flag가 변경됨
4. 17) ⭐️ 'status: loading' has been changed to 'status: pending' and 'isLoading' has been changed to 'isPending' and 'isInitialLoading' has now been renamed to 'isLoading'

Suspense 지원
21) ⭐️ new hooks for suspense


이제 이걸 그룹별로 살펴봅시다.

## Query 사용법 변경됨
v4 에서는 두 형태가 모두 지원되었다고 합니다. 하지만 아래와 같은 이유로 {} 형식만 지원하는 것으로 변경되었습니다.

- 유지보수 힘들었음
- 매개변수 확인을 위한 런타임 검사 필요
- 불필요한 코드 양산

```javascript
- useQuery(key, fn, options)
+ useQuery({ queryKey, queryFn, ...options })
- useInfiniteQuery(key, fn, options)
+ useInfiniteQuery({ queryKey, queryFn, ...options })
- useMutation(fn, options)
+ useMutation({ mutationFn, ...options })
- useIsFetching(key, filters)
+ useIsFetching({ queryKey, ...filters })
- useIsMutating(key, filters)
+ useIsMutating({ mutationKey, ...filters })


- queryClient.isFetching(key, filters)
+ queryClient.isFetching({ queryKey, ...filters })
- queryClient.ensureQueryData(key, filters)
+ queryClient.ensureQueryData({ queryKey, ...filters })
- queryClient.getQueriesData(key, filters)
+ queryClient.getQueriesData({ queryKey, ...filters })

... 생략 ...
```


getQueryData 같은 경우는 queryKey 만 받도록 더 간소화 되었습니다.
```javascript
queryClient.getQueryData(queryKey)
```


useQuery의 옵션인 `onSuccess`, `onError`, `onSettled`가 제거되었습니다. 제거가 된 가장 큰 이유는 사용법이 직관적이지만, 결국 여러 컴포넌트의 조합속에서 버그를 유발하고 또 그 버그를 지나치기 쉬운 구조라는 게 이유입니다.

캐시를 설정하는 경우 그 시간만큼 캐시에서만 데이터를 가져올 뿐, onSucces 은 호출이 안된다고 합니다. 이때 onSucces 에서 상태 변화하는 코드가 있다면 버그를 유발하는 경우가 발생합니다.

좀 더 자세한 내용은 아래 블로그 글을 읽어보세요
https://tkdodo.eu/blog/breaking-react-querys-api-on-purpose



## Query 의 Options 가 변경됨
과거의 `cacheTime`, `useErrorBoundary`, `keepPreviousData` (isPreviousData) 가 
현재의 `gcTime`, `throwOnError`, `placeholderData` 로 변경되었습니다.

각 옵션들이 변경된 이유가 있는데요.

#### gcTime
cacheTime 는 좀 더 직관적인 의미전달을 위해서 gcTime 으로 변경되었습니다.

#### throwOnError
기존의 useErrorBoundary 는 use 라는 접미사가 React 라는 프레임워크에 종속되는듯한 접미사라서  throwOnError 로 변경되었습니다.

#### placeholderData
keepPreviousData 가 변경된 이유는 placeholderData 와 거의 유사하게 동작해서 입니다.

둘 다 데이터 로딩 중에 임시로 대체하는 데이터를 제공하는데 쓰입니다. 

굳이 따지자면
keepPreviousData 같은 경우 이전의 데이터를 보여주고
placeholderData는 에 설정된 데이터를 보여줍니다.

그래서 아래와 같이 변경되었습니다.

```javascript
import {
   useQuery,
+  keepPreviousData
} from "@tanstack/react-query";

const {
   data,
-  isPreviousData,
+  isPlaceholderData,
} = useQuery({
  queryKey,
  queryFn,
-
+ placeholderData: keepPreviousData
});
```

keepPreviousData 는 (prev) => prev 형태로 이전데이터를 그대로 전달하는 함수로 바뀌었고 이 keepPreviousData를 placeholderData 옵션으로 전달하게끔 변경되었습니다.


#### initialPageParam
initialPageParam 는 useInfiniteQuery 에 해당 되는 속성입니다. 최초 값을 initialPageParam 에 설정하도록 변경되었습니다.

```javascript
useInfiniteQuery({
   queryKey,
-  queryFn: ({ pageParam = 0 }) => fetchSomething(pageParam),
+  queryFn: ({ pageParam }) => fetchSomething(pageParam),
+  initialPageParam: 0,
   getNextPageParam: (lastPage) => lastPage.next,
})
```



## Query flag가 변경됨
제가 생각하기엔 가장 이해가 필요한 변경점이라고 생각합니다. 표면적으로는
`loading` 옵션이 `pending`으로 변경되었으며, 마찬가지로 `isLoading` 플래그가 `isPending`으로 변경되었습니다

v4 기준 상태 체계는 아래와 같이 분류됩니다.

- Query Status: loading, success, error
- Fetch Status: idle, fetching, paused
![[Pasted image 20250618024616.png]]

Query State 가 쿼리 결과 데이터의 상태에 초점
Fetch State 는 네트워크 fetch의 상태에 초점

를 나타내는데, 

fetch도 일어나지 않았고, 단지 data 가 없는 상황일 때,  Query State 는 loading 입니다. 이 부분 오해를 낳습니다. 이 경우 로딩바가 돌아야하는 상황이 아닌 단지 데이터가 초기에 없을 뿐입니다.
```
const result = useQuery({
  queryKey: ['user', id],
  queryFn: fetchUser,
  enabled: false, // disabled 상태
})
```


그래서 아래와 같이 변경되었습니다.

기존의 isLoading --> isPending
앞으로의 isLoading <-- isPending && isFetching 으로 구현

정리하면
isPending 은 데이터가 없는 상태 (fetch 와 관련없음)
isFetching 은 네트워크 요청이 진행 중인 상태!
isLoading 진짜 로딩 중인 상태!


## Suspense 지원
v5에서는 `data fetching`에 대한 `suspense`가 마침내 안정화되었다고 합니다. 그러면서 `useSuspenseQuery`, `useSuspenseInfiniteQuery`, `useSuspenseQueries` 3가지 훅이 추가되었습니다.


useSuspenseQuery 들은 `enabled`, `throwOnError`, `placeholderData` 를 속성으로 넘길 수 없습니다.

useSuspenseQuery 들은 isLoading, isError 같은 상태들을 다루지 않습니다. 대신 - Suspense 와 ErrorBoundary 조합으로 대응을 합니다.


그런데 내부 코드를 살펴보면 실제로는 useSuspenseQuery 내부에서
enabled, throwOnError, placeholderData 등등을 고정값으로 설정 해뒀습니다.

```typescript
'use client'
...

export function useSuspenseQuery<
	TQueryFnData = unknown,
	TError = DefaultError,
	TData = TQueryFnData,
	TQueryKey extends QueryKey = QueryKey,
>	(
	options: UseSuspenseQueryOptions<TQueryFnData, TError, TData, TQueryKey>,
	queryClient?: QueryClient,
): UseSuspenseQueryResult<TData, TError> {

if (process.env.NODE_ENV !== 'production') {
	if ((options.queryFn as any) === skipToken) {
		console.error('skipToken is not allowed for useSuspenseQuery')
	}
}

  
return useBaseQuery({
		...options,
		enabled: true,
		suspense: true,
		throwOnError: defaultThrowOnError,
		placeholderData: undefined,
	},
	QueryObserver,
	queryClient,
) as UseSuspenseQueryResult<TData, TError>

}
```

소스 코드 내부에 좀 더 봐보면
suspense.ts > shouldSuspend()
```typescript

// v5 의 shouldSuspend
export const shouldSuspend = (  
  defaultedOptions:  
    | DefaultedQueryObserverOptions<any, any, any, any, any>  
    | undefined,  
  result: QueryObserverResult<any, any>,  
) => defaultedOptions?.suspense && result.isPending


// v4 의 shouldSuspend
export const shouldSuspend = (  
  defaultedOptions:  
    | DefaultedQueryObserverOptions<any, any, any, any, any>  
    | undefined,  
  result: QueryObserverResult<any, any>,  
  isRestoring: boolean,  
) => defaultedOptions?.suspense && willFetch(result, isRestoring)

// true 일때
// - 로딩중이면서
// - 데이터 패칭중이면서
// - isRestoring 이 아닐때
export const willFetch = (  
  result: QueryObserverResult<any, any>,  
  isRestoring: boolean,  
) => result.isLoading && result.isFetching && !isRestoring

```

결론지어보면 v5 부터는 suspense 는 suspense 가 true 이면서 result.isPending 이 true 일때 Suspend 가 취급된다고 봐야한다,


isPending 은 v5 이후로 기존의 isLoading 이면서 현재 데이터가 없는 상태 (fetch 와 관련없음) 으로 봐야하는 값이다.

기존의 v4 보다 훨씬 깔끔한 로직으로 돌아간다.


#### 실험해보고 싶은 점>
useQuery 도 useBaseQuery 이다. types 에 보면 UseQueryOptions에 여전히 suspense 속성이 살아있다. 
useQuery 에 suspense 속성과 enabled를 모두 true 로 주면 기존처럼 사용할 수 있는거 아닌가?
```
export interface UseQueryOptions<
  TQueryFnData = unknown,
  TError = DefaultError,
  TData = TQueryFnData,
  TQueryKey extends QueryKey = QueryKey,
> extends OmitKeyof<
    UseBaseQueryOptions<TQueryFnData, TError, TData, TQueryFnData, TQueryKey>,
    'suspense'
  > {}
```



#### 그 외
그외에 QueryErrorResetBoundary 라는 것이 지원되고

아래와 같은 실험적인 기능들도 들어갔습니다.
- 서버에서 Suspense와 함께 사용 가능 (실험적 / by use Next.js)
- useQuery().promise & React.use() (실험적)





# 알아두면,  좋을 것들
### Simplified optimistic updates
낙관적 업데이트를 쉽게 하는 방법을 제공합니다. useMutation 이후에 isPending 과 함께 variables를 꺼내서 UI 가 표현되는 방식을 좀 더 부드럽게 표현할 수 있습니다.

```javascript
const queryInfo = useTodos();
const addTodoMutation = useMutation({
  mutationFn: (newTodo: string) => axios.post("/api/data", { text: newTodo }),
  onSettled: () => queryClient.invalidateQueries({ queryKey: ["todos"] }),
});

if (queryInfo.data) {
  return (
    <ul>
      {queryInfo.data.items.map((todo) => (
        <li key={todo.id}>{todo.text}</li>
      ))}
      {addTodoMutation.isPending && (
        <li key={String(addTodoMutation.submittedAt)} style={{ opacity: 0.5 }}>
          {addTodoMutation.variables}
        </li>
      )}
    </ul>
  );
}
```

기존에는 낙관적 업데이트가 아래와 같이 캐시기반의 방식이 있었다면, 간단하게 flag 와 variables 로 UI 를 표현할 수 있습니다.

```
onMutate: async (newTodo) => {
  const previousTodos = queryClient.getQueryData(['todos'])

  queryClient.setQueryData(['todos'], (old) => [...old, newTodo])

  return { previousTodos }
}
```

다만

v5 에서 부터 추가된 UI 기반 방식은 하나의 컴포넌트에서만 제어가 필요할 때 유효하고, 여러 컴포넌트를 일관되게 제어해야한다면, 캐시방식을 써야합니다.


#### new 'combine' option for 'useQueries'
`seQueries` 결과 데이터를 단일 값으로 결합하려면 `combine` 옵션을 사용할 수 있습니다.