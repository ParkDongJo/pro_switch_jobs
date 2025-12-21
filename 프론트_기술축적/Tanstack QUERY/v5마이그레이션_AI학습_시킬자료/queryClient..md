

지금 부터 아래 경로의 파일을 찾아서, 그 파일안에 있는 queryClient 를 위에 학습내용에 따라 코드를 모두 수정해줘

---

### **📁 1~10**

- src/components/molecules/infiniteData/Select/InfiniteSelect.tsx
- src/features/alarm/components/Notification/NotificationModules.tsx
- src/features/alarm/hooks/useAlarmsSocket.tsx
- src/features/company/components/MyInfo/InfoEdit.tsx
- src/features/driver/components/Detail/EditDriverInfoDrawer.tsx
- src/features/driver/components/Invitation/MemberSelect.tsx
- src/features/geoFence/components/Detail/Edit.tsx
- src/features/geoFence/components/List/List.tsx
- src/features/group/components/Management/Vehicle/Form/Edit.tsx
- src/features/incident/components/Report/Contents/ReportDetail/Media.tsx
    

---

### **📁 11~20**

- src/features/incident/components/Report/Contents/ReportDetail/MediaUploadModal.tsx
- src/features/incident/components/Report/Footer/Footer.tsx
- src/features/incident/components/Report/VideoTimeSync/VideoTimeSync.tsx
- src/features/incident/components/View/TableView.tsx
- src/features/llm/hooks/useAIAssistant.tsx
- src/features/member/components/Management/Edit.tsx
- src/features/ota/components/AutoUpdate/View.tsx
- src/features/ota/components/ReleaseUpdate/View.tsx
- src/features/vehicle/components/Management/Detail/Card/Operation/EditableMemo.tsx
- src/features/vehicle/components/Management/Detail/Card/VehicleMonitoring/AbnormalEvent/RemoteDiagnosis.tsx
    

---

### **📁 21~29**

- src/features/vehicle/components/Management/Detail/List/List.tsx
- src/features/vehicle/components/Management/Edit/Information/InformationEdit.tsx
- src/features/vehicle/components/Management/Edit/Operation/OperationEdit.tsx
- src/features/vehicle/components/Schedule/ScheduleDetailEdit.tsx
- src/features/vehicle/components/Schedule/ScheduleDetailView.tsx
- src/pages/Dashboard/DashboardPage.tsx
- src/pages/Vehicle/Detail/VehicleDetailTitle.test.tsx
- src/pages/Vehicle/Detail/Wrapper.tsx
- src/features/vehicle/components/Schedule/ScheduleDetailEdit.tsx (a9c10eb)
    

---

다른 기준으로 정리하거나, 각 파일 안의 queryClient 관련 메서드를 정리해드릴 수도 있으니 필요하면 알려주세요!


# 학습 내용


#### 기존 react-query import 문을 아래와 같이 변경한다.

v4 코드 예시
```
import { useQuery } from 'react-query';
```

v5 코드 예시
```
import { useQuery } from '@tanstack/react-query';
```


#### queryClient.* 에 넘기는 param 들을 {} 로 감싸서 넘기도록 변경한다.

아래와 같이 {} 로 감싼다.

```diff
- queryClient.isFetching(key, filters)
+ queryClient.isFetching({ queryKey, ...filters })
- queryClient.ensureQueryData(key, filters)
+ queryClient.ensureQueryData({ queryKey, ...filters })
- queryClient.getQueriesData(key, filters)
+ queryClient.getQueriesData({ queryKey, ...filters })
- queryClient.setQueriesData(key, updater, filters, options)
+ queryClient.setQueriesData({ queryKey, ...filters }, updater, options)
- queryClient.removeQueries(key, filters)
+ queryClient.removeQueries({ queryKey, ...filters })
- queryClient.resetQueries(key, filters, options)
+ queryClient.resetQueries({ queryKey, ...filters }, options)
- queryClient.cancelQueries(key, filters, options)
+ queryClient.cancelQueries({ queryKey, ...filters }, options)
- queryClient.invalidateQueries(key, filters, options)
+ queryClient.invalidateQueries({ queryKey, ...filters }, options)
- queryClient.refetchQueries(key, filters, options)
+ queryClient.refetchQueries({ queryKey, ...filters }, options)
- queryClient.fetchQuery(key, fn, options)
+ queryClient.fetchQuery({ queryKey, queryFn, ...options })
- queryClient.prefetchQuery(key, fn, options)
+ queryClient.prefetchQuery({ queryKey, queryFn, ...options })
- queryClient.fetchInfiniteQuery(key, fn, options)
+ queryClient.fetchInfiniteQuery({ queryKey, queryFn, ...options })
- queryClient.prefetchInfiniteQuery(key, fn, options)
+ queryClient.prefetchInfiniteQuery({ queryKey, queryFn, ...options })
```

```diff
- queryCache.find(key, filters)
+ queryCache.find({ queryKey, ...filters })
- queryCache.findAll(key, filters)
+ queryCache.findAll({ queryKey, ...filters })
```




### ⭐️ 'queryClient.getQueryData', 'queryClient.getQueryState' now accepts queryKey only as an Argument

[](https://github.com/ssi02014/react-query-tutorial/blob/main/document/v5.md#2-%EF%B8%8F-queryclientgetquerydata-queryclientgetquerystate-now-accepts-querykey-only-as-an-argument)

- `queryClient.getQueryData`의 인수가 `queryKey`만 받도록 v5에서 수정되었습니다.

```diff
- queryClient.getQueryData(queryKey, filters)
+ queryClient.getQueryData(queryKey)
```

- 마찬가지로 `queryClient.getQueryState`도 인수가 `queryKey`만 받도록 v5에서 수정되었습니다.

```diff
- queryClient.getQueryState(queryKey, filters)
+ queryClient.getQueryState(queryKey)
```


#### active: true | false 형태는 type: 'active' 로 변경해라

아래의 경우는
```typescript
queryClient.invalidateQueries({
	queryKey: [],
	active: true,
	exact: false,
});
```

이렇게 변경해라
```
queryClient.invalidateQueries({
	queryKey: [],
	exact: false,
	type: 'active',
});
```


# [[프롬프트]]

좋습니다! 아래는 queryClient 관련 마이그레이션 프롬프트에 **active: true | false → type: 'active' | 'inactive'** 변경사항을 포함하여 정리한 최종 버전입니다. 이 프롬프트는 GPT가 React Query v4 → v5 마이그레이션을 정확히 수행할 수 있도록 설계되었습니다.

---

### **✅ React Query v4 → v5** 

### **queryClient**

###  **&** 

### **queryCache**

###  **마이그레이션 프롬프트**

  

**📌 프롬프트 내용:**

````
You are migrating React Query code from v4 to v5. Your task is to transform all `queryClient` and `queryCache` method usages according to the following specifications:

---

1. ✅ **Update import paths**

Update all imports from `react-query` to `@tanstack/react-query`.

```diff
- import { useQuery } from 'react-query';
+ import { useQuery } from '@tanstack/react-query';
````

---

2. ✅ **Convert method arguments to object format**
    

  

In v5, many methods require an object form with queryKey and optional filters. Update the following methods accordingly:

```
- queryClient.isFetching(key, filters)
+ queryClient.isFetching({ queryKey: key, ...filters })

- queryClient.ensureQueryData(key, filters)
+ queryClient.ensureQueryData({ queryKey: key, ...filters })

- queryClient.getQueriesData(key, filters)
+ queryClient.getQueriesData({ queryKey: key, ...filters })

- queryClient.setQueriesData(key, updater, filters, options)
+ queryClient.setQueriesData({ queryKey: key, ...filters }, updater, options)

- queryClient.removeQueries(key, filters)
+ queryClient.removeQueries({ queryKey: key, ...filters })

- queryClient.resetQueries(key, filters, options)
+ queryClient.resetQueries({ queryKey: key, ...filters }, options)

- queryClient.cancelQueries(key, filters, options)
+ queryClient.cancelQueries({ queryKey: key, ...filters }, options)

- queryClient.invalidateQueries(key, filters, options)
+ queryClient.invalidateQueries({ queryKey: key, ...filters }, options)

- queryClient.refetchQueries(key, filters, options)
+ queryClient.refetchQueries({ queryKey: key, ...filters }, options)
```

For query execution functions:

```
- queryClient.fetchQuery(key, fn, options)
+ queryClient.fetchQuery({ queryKey: key, queryFn: fn, ...options })

- queryClient.prefetchQuery(key, fn, options)
+ queryClient.prefetchQuery({ queryKey: key, queryFn: fn, ...options })

- queryClient.fetchInfiniteQuery(key, fn, options)
+ queryClient.fetchInfiniteQuery({ queryKey: key, queryFn: fn, ...options })

- queryClient.prefetchInfiniteQuery(key, fn, options)
+ queryClient.prefetchInfiniteQuery({ queryKey: key, queryFn: fn, ...options })
```

---

3. ✅ **Update queryCache methods**
    

```
- queryCache.find(key, filters)
+ queryCache.find({ queryKey: key, ...filters })

- queryCache.findAll(key, filters)
+ queryCache.findAll({ queryKey: key, ...filters })
```

---

4. ✅ **Simplify getQueryData / getQueryState**
    

  

The following methods now accept only the queryKey (no filters):

```
- queryClient.getQueryData(queryKey, filters)
+ queryClient.getQueryData(queryKey)

- queryClient.getQueryState(queryKey, filters)
+ queryClient.getQueryState(queryKey)
```

---

5. ✅ **Replace active: true | false with type: 'active' | 'inactive'**
    

  

If a filter contains the active field, it must be replaced with a type field as follows:

```
- active: true
+ type: 'active'

- active: false
+ type: 'inactive'
```

Example:

```
queryClient.invalidateQueries({
  queryKey: ['some-key'],
  active: true,
  exact: false,
});
```

Becomes:

```
queryClient.invalidateQueries({
  queryKey: ['some-key'],
  type: 'active',
  exact: false,
});
```

---

💡 Apply all transformations carefully across the entire file or codebase. Be sure to preserve logic, destructuring, and formatting where appropriate.

```
---

필요하다면 이 프롬프트를 코드모드, codemod 스크립트용, 혹은 TypeScript-aware AST 포맷으로도 변환해 드릴 수 있습니다. 다음 마이그레이션 작업도 함께 준비할까요?
```