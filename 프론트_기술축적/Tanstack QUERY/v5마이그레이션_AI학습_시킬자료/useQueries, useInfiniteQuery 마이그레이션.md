useQueries 는 2025.7.17 기준 4 files 에 존재한다.
```
src/features/region/component/Detail/View.tsx
src/features/region/component/Register/View.tsx
src/features/vehicle/components/Management/Detail/driving/DrivingHistoryList.tsx
src/lib/react-query.ts
```

useQueriesTyped  는 2025.7.17 기준 3 files 에 존재한다.
```
src/features/ota/components/Management/View.tsx:
src/features/vehicle/components/Management/Detail/Card/Device.tsx:
src/features/vehicle/components/SafeDriving/View/Score.tsx:
```


지금 부터 아래 경로의 파일을 찾아서, 그 파일안에 있는 useInfiniteQueryWithInitParam 를 위에 학습내용에 따라 코드를 모두 수정해줘

useInfiniteQuery 는 2025.7.17 기준 11 files 에 존재한다.
```
src/components/molecules/infiniteData/Select/InfiniteSelect.tsx  
src/features/driver/components/Invitation/MemberSelect.tsx  
src/features/rndTask/component/Panel/RouteRegisterPanel.tsx  
src/features/rndTask/component/Register/BasicInfo.tsx  
src/features/rndTask/component/Register/CarConfiguration.tsx  
src/features/rndTask/component/Register/WorkLocation.tsx  
src/features/route/component/Detail/View.tsx  
src/features/route/component/Register/View.tsx  
src/features/schedulePanel/components/SchedulePanel.tsx  
src/features/station/component/Detail/View.tsx  
src/features/station/component/Register/View.tsx
```

useInfiniteQueryWithInitParam  는 2025.7.17 기준 3 files 에 존재한다.
```
src/features/alarm/components/Notification/NotificationModules.tsx: 
src/features/installer/components/Matching/Vehicle/QR/Vehicles.tsx:
src/features/vehicle/components/Management/Detail/interestArea/InterestAreaHistoryList.tsx:
```


# 학습 내용 

#### @tanstack/react-query 
기존 react-query import 문을 아래와 같이 변경한다.

v4 코드 예시
```
import { useInfiniteQuery } from 'react-query';
```

v5 코드 예시
```
import { useInfiniteQuery } from '@tanstack/react-query';
```


#### useInfiniteQuery 에 넘기는 param 들을 {} 로 감싸서 넘기도록 변경한다.
아래와 같이 {} 로 감싼다.
fn 은 mutationFn: fn 으로 변경한다.
```
- useInfiniteQuery(fn, options)
+ useInfiniteQuery({ mutationFn, ...options })
```

v4 코드 예시
```
useInfiniteQuery(
  [...(queryKey || [])],
  ({ pageParam = 1 }) => {
	  ...생략...
  },
  {
	getNextPageParam: (lastPage) => {
		...생략...
	},
	select: ({ pages, pageParams }) => {
		...생략...
	},
  }
);

```

v5 코드 예시
```
useInfiniteQuery({
  queryKey: [...(queryKey || [])],
  queryFn: ({ pageParam }) => {
    ...생략...
  },
  initialPageParam: 1,
  getNextPageParam: (lastPage) => {
    ...생략...
  },
  select: ({ pages, pageParams }) => {
    ...생략...
  }
});

```



### infinite queries now need a 'initialPageParam'

- 이전에는 `undefined` 값을 가진 `pageParam`을 `queryFn`에 전달했고, `queryFn`에서 `pageParam`에 대한 기본 값을 정의했습니다. 하지만 이런 경우 직렬화 할 수 없는 쿼리 캐시에 `undefined`인 상태로 저장된다는 단점이 있습니다.
- v5부터는 아래 예제처럼 `infinite Query` 옵션에 명시적인 `initialPageParam`을 전달해야 합니다.

```diff
useInfiniteQuery({
   queryKey,
-  queryFn: ({ pageParam = 0 }) => fetchSomething(pageParam),
+  queryFn: ({ pageParam }) => fetchSomething(pageParam),
+  initialPageParam: 0,
   getNextPageParam: (lastPage) => lastPage.next,
})
```


### Manual mode for infinite queries has been removed
getNextPageParam 가 없다면, getNextPageParam 를 넣어줘라
```tsx
function Projects() {
  const fetchProjects = ({ pageParam = 0 }) =>
    fetch("/api/projects?cursor=" + pageParam);

  const {
    status,
    data,
    isFetching,
    isFetchingNextPage,
    fetchNextPage,
    hasNextPage,
  } = useInfiniteQuery({
    queryKey: ["projects"],
    queryFn: fetchProjects,
    getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
  });

  // Pass your own page param
  const skipToCursor50 = () => fetchNextPage({ pageParam: 50 });
}
```


#### keepPreviousData 변경
아래와 같이 placeholderData: keepPreviousData 로 keepPreviousData를 가져와서 주입해줘
```
import {
   useQuery,
+  keepPreviousData
} from "@tanstack/react-query";

const {
   data,
-  isPreviousData,
+  isPlaceholderData,
} = useInfiniteQuery({
  queryKey,
  queryFn,
- keepPreviousData: true,
+ placeholderData: keepPreviousData
});
```



아래는 **React Query v4 → v5 마이그레이션**을 위한 useInfiniteQuery 관련 변경사항을 GPT가 이해하고 자동화 수정 작업을 수행할 수 있도록 구성한 프롬프트입니다.

---

### **✅ React Query v4 → v5** 

### **useInfiniteQuery**

###  **마이그레이션 프롬프트**


**📌 프롬프트 내용:**

````
You are helping migrate React Query code from v4 to v5. Focus specifically on transforming useInfiniteQuery according to the following rules:

---

1. ✅ **Update import path**

Replace all `react-query` imports with `@tanstack/react-query`.

For example:
```diff
- import { useInfiniteQuery } from 'react-query';
+ import { useInfiniteQuery } from '@tanstack/react-query';
````

---

2. ✅ **Convert useInfiniteQuery API to object syntax**
    

  

In v4, useInfiniteQuery often uses positional arguments:

```
useInfiniteQuery(queryKey, queryFn, options)
```

In v5, it must be passed as a single object:

```
useInfiniteQuery({
  queryKey,
  queryFn,
  ...options
})
```

So this:

```
useInfiniteQuery(['items'], ({ pageParam = 1 }) => fetchItems(pageParam), {
  getNextPageParam: (lastPage) => lastPage.next,
})
```

Becomes:

```
useInfiniteQuery({
  queryKey: ['items'],
  queryFn: ({ pageParam }) => fetchItems(pageParam),
  initialPageParam: 1,
  getNextPageParam: (lastPage) => lastPage.next,
})
```

---

3. ✅ **Add initialPageParam if missing**
    

  

React Query v5 requires that infinite queries explicitly define initialPageParam.

  

If queryFn destructures pageParam with a default value (e.g. pageParam = 0), remove the default and instead add initialPageParam: <default> in the query options object.

```
- queryFn: ({ pageParam = 1 }) => fetchPage(pageParam),
+ queryFn: ({ pageParam }) => fetchPage(pageParam),
+ initialPageParam: 1,
```

---

4. ✅ **Ensure getNextPageParam is defined**
    

  

React Query v5 does not support manual mode for infinite queries.

  

If getNextPageParam is missing, insert a placeholder or inferred implementation. Use:

```
getNextPageParam: (lastPage, pages) => lastPage.nextCursor
```

as a default pattern.

---

5. ✅ **Clean up destructured values**
    

  

If the return value from useInfiniteQuery includes isLoading, rename it to isPending.

```
- const { isLoading } = useInfiniteQuery(...);
+ const { isPending } = useInfiniteQuery(...);
```

This change aligns with v5’s renaming of isLoading → isPending.

---

💡 Apply all of the above transformations consistently throughout the codebase to fully migrate useInfiniteQuery to React Query v5 format. Ensure proper formatting and syntax validity.

```
---

필요하면 위 프롬프트를 다국어로 번역하거나 `eslint` 코드모드와 통합할 수도 있어요. 다음으로 마이그레이션할 훅이 있다면 말씀해 주세요!
```