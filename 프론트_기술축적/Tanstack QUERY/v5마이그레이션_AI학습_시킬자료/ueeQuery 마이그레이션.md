
지금 부터 아래 경로의 파일을 찾아서, 그 파일안에 있는 useQuery 를 위에 학습내용에 따라 코드를 모두 수정해줘

useQuery 는 2025.7.17 기준 113 files 에 존재한다.
```
src/components/molecules/selectFilter/components/QueryFilter/VehicleGroups.tsx  
src/components/molecules/selectFilter/components/QueryFilter/VehicleGroupsTitleFilter.tsx  
src/components/molecules/selectFilter/components/QueryFilter/WorkflowEvent.tsx  
src/features/abnormal/Card/MetricsCard.tsx  
src/features/asset/api/assetConnectionLogList.ts  
src/features/asset/api/assetDetail.ts  
src/features/battery/components/SpeedingStatusChart.tsx  
src/features/company/components/MyInfo/Info.tsx  
src/features/driver/components/Detail/DriverDetailHeader.tsx  
src/features/driver/components/Detail/DriverInfoCard.tsx
```

```
src/features/driver/components/Detail/DrivingInfoCard.tsx  
src/features/driver/components/Detail/EditDriverInfoDrawer.tsx  
src/features/driver/components/Detail/SafeDriveCard.tsx  
src/features/driver/components/Detail/Terms.tsx  
src/features/driver/components/Registration/DriverRegister.tsx  
src/features/geoFence/components/LoaderGeoFenceCategory.tsx  
src/features/geoFence/components/Card/MetricsCard.tsx  
src/features/geoFence/components/Detail/Edit.tsx  
src/features/group/components/Management/Vehicle/Detail/Detail.tsx  
src/features/group/components/Selector/VehicleGroupSelectorModal.tsx  
```

```
src/features/incident/components/Download/Download.tsx  
src/features/incident/components/Download/List.tsx  
src/features/incident/components/Report/Report.tsx  
src/features/incident/components/Report/Contents/Analytics/Analytics.tsx  
src/features/incident/components/Report/Form/Form.tsx  
src/features/incident/components/UnconfirmedIncident/UnconfirmedIncidentInlineMessage.tsx  
src/features/incident/components/UnconfirmedIncident/UnconfirmedIncidentProcessorModal.tsx  
src/features/inspection/component/Header/Header.tsx  
src/features/inspection/component/Management/VehicleView.tsx  
src/features/inspection/component/panel/InspectionDetailEdit.tsx
```

```
src/features/inspection/component/panel/InspectionDetailPanel.tsx  
src/features/inspection/component/panel/InspectionDetailReadOnly.tsx  
src/features/inspection/component/panel/InspectionRegisterContent.tsx  
src/features/llm/components/AIAssistant.tsx  
src/features/member/components/CascadeSelect/MemberGroupList.tsx  
src/features/member/components/CascadeSelect/MemberList.tsx  
src/features/member/components/Registration/MemberRegister/MemberRegister.tsx  
src/features/ota/components/AutoUpdate/View.tsx  
src/features/ota/components/CampaignOtaUpdateLog/View.tsx  
src/features/ota/components/Management/UpdateList/List.tsx  
```
v
```
src/features/ota/components/ReleaseUpdate/View.tsx  
src/features/policy/component/Header/Header.tsx  
src/features/policy/component/Management/View.tsx  
src/features/policy/component/Panel/ApplyStatusPanel.tsx  
src/features/region/component/Detail/View.tsx  
src/features/region/component/Header/Header.tsx  
src/features/region/component/Register/View.tsx  
src/features/rndTask/component/Detail/View.tsx  
src/features/rndTask/component/Detail/WorkLocation.tsx  
src/features/rndTask/component/Header/Header.tsx
```
v
```
src/features/rndTask/component/Panel/RouteRegisterPanel.tsx  
src/features/rndTask/component/Panel/ScheduleDetailPanel.tsx  
src/features/rndTask/component/Register/CarConfiguration.tsx  
src/features/rndTask/component/Register/View.tsx  
src/features/role/component/Common/Select.tsx  
src/features/role/component/Detail/View.tsx  
src/features/role/component/Header/Header.tsx  
src/features/role/component/Management/View.tsx  
src/features/role/component/Register/BasicInfo.tsx  
src/features/role/component/Register/TableViewExpand.tsx  
```
v
```
src/features/route/component/Common/RoadRouteModal.tsx  
src/features/route/component/Detail/View.tsx  
src/features/route/component/Header/Header.tsx  
src/features/route/component/Register/RegisterContent.tsx  
src/features/route/component/Register/View.tsx  
src/features/schedulePanel/components/SchedulePanel.tsx  
src/features/station/component/Detail/View.tsx  
src/features/station/component/Header/Header.tsx  
src/features/threat/component/Header/Header.tsx  
src/features/trip/components/Rank/Card/MetricsCard.tsx  
```
v
```
src/features/trip/components/Summary/Card/MetricsCard.tsx  
src/features/vehicle/components/Control/Registration/ControlCommand/List/ListWrapper.tsx  
src/features/vehicle/components/Management/Card/MetricsCard.tsx  
src/features/vehicle/components/Management/Detail/DetailTabs.tsx  
src/features/vehicle/components/Management/Detail/Card/AccidentHistory.tsx  
src/features/vehicle/components/Management/Detail/Card/Asset.tsx  
src/features/vehicle/components/Management/Detail/Card/ControlHistory.tsx  
src/features/vehicle/components/Management/Detail/Card/DrivingMonitoring.tsx  
src/features/vehicle/components/Management/Detail/Card/Information.tsx  
src/features/vehicle/components/Management/Detail/Card/InterestAreaHistory.tsx  
```
v
```
src/features/vehicle/components/Management/Detail/Card/Maintenance.tsx  
src/features/vehicle/components/Management/Detail/Card/OTAHistory.tsx  
src/features/vehicle/components/Management/Detail/Card/Schedule.tsx  
src/features/vehicle/components/Management/Detail/Card/WorkflowHistory.tsx  
src/features/vehicle/components/Management/Detail/Card/Operation/Operation.tsx  
src/features/vehicle/components/Management/Detail/Card/VehicleMonitoring/View.tsx
src/features/vehicle/components/Management/Detail/interestArea/InterestAreaHistoryMap.tsx  
src/features/vehicle/components/Management/Detail/List/List.tsx  
src/features/vehicle/components/Management/Edit/Information/Form.tsx  
src/features/vehicle/components/Management/Edit/Insurance/InsuranceCompanySelect.tsx  
```
v
```
src/features/vehicle/components/Management/Timeline/Timeline.tsx  
src/features/vehicle/components/Monitoring/MonitoringList/MonitoringList.tsx  
src/features/vehicle/components/Monitoring/Panel/MonitoringPanel.tsx  
src/features/vehicle/components/Schedule/ScheduleDetailView.tsx  
src/features/vehicle/components/Schedule/Card/MetricsCard.tsx  
src/features/vehicle/components/Schedule/Registration/SelectedVehicleView.tsx  
src/features/vehicle/components/Schedule/Registration/View.tsx  
src/features/vehicle/components/Selector/VehiclesSelectorModal.tsx  
src/features/vehicle/components/Timeline/View.tsx  
src/features/vehicle/components/VehicleCascadeSelect/VehicleGroupList.tsx  
```
v
```
src/features/vehicle/components/VehicleCascadeSelect/VehicleList.tsx  
src/features/vehicle/hooks/useDashcamSrc.ts  
src/features/vehicle/hooks/useRemoteCamera.ts  
src/features/vehicleMaintenance/component/Header/Header.tsx  
src/features/vehicleMaintenance/component/Management/DetailPanel.tsx  
src/features/vehicleMaintenance/component/Management/RegisterContent.tsx  
src/pages/Member/DetailPage.tsx  
src/pages/Report/FleetIncidentReportPage.tsx  
src/pages/RnDTask/DetailPage.tsx  
src/pages/SmartManagement/GeoFenceDetailPage.tsx  
src/pages/Vehicle/Detail/Wrapper.tsx  
src/pages/VehicleBattery/VehicleBatteryPage.tsx  
src/routes/protected/index.tsx  
```

지금 부터 아래 경로의 파일을 찾아서, 그 파일안에 있는 useQueryWithInitParam 를 위에 학습내용에 따라 코드를 모두 수정해줘

useQueryWithInitParam 는 2025.7.17 기준 36 files 에 존재한다.
v
```
src/features/abnormal/components/List/View.tsx  
src/features/alarm/components/History/View.tsx  
src/features/asset/components/Management/View.tsx  
src/features/asset/components/Storage/Activity/View.tsx  
src/features/dailyDrive/components/View.tsx  
src/features/driver/components/List/Invitation/View.tsx  
src/features/driver/components/List/Management/View.tsx  
src/features/geoFence/components/History/List/List.tsx  
src/features/geoFence/components/List/List.tsx  
src/features/group/components/Management/Vehicle/ListView.tsx 
```
v
```
src/features/incident/components/History/View.tsx  
src/features/incident/components/View/View.tsx  
src/features/incident/components/View/Chart/AccidentStatusSummary.tsx  
src/features/incident/components/View/Chart/AccidentTimeSummary.tsx  
src/features/incident/components/View/Chart/AccidentTypeSummary.tsx  
src/features/inspection/component/Management/View.tsx  
src/features/ota/components/AutoUpdate/View.tsx  
src/features/ota/components/CampaignOtaUpdateLog/View.tsx  
src/features/ota/components/ReleaseUpdate/View.tsx  
src/features/ota/components/UpdateLog/View.tsx  
```
v
```
src/features/policy/component/Management/View.tsx  
src/features/region/component/Management/View.tsx  
src/features/rndTask/component/Management/View.tsx  
src/features/route/component/Management/View.tsx  
src/features/station/component/Management/View.tsx  
src/features/support/components/Incident/Title.tsx  
src/features/threat/component/Detection/View.tsx  
src/features/vehicle/components/Control/Registration/Registration.tsx  
src/features/vehicle/components/Management/Detail/driving/Driving.tsx  
src/features/vehicle/components/Management/List/SiriusView.tsx
```
v
```
src/features/vehicle/components/SafeDriving/List/View.tsx  
src/features/vehicle/components/Schedule/ReservationListPage.tsx  
src/features/vehicle/components/Schedule/ScheduleChartPage.tsx  
src/features/vehicleMaintenance/component/Management/View.tsx  
src/features/workflow/components/History/List/View.tsx  
src/pages/Report/Logbook/DownloadListPage.tsx  
```



아래 문서를 학습하고, 학습을 완료하면 '완료' 라고만 대답해

# useQuery, useQuries 학습내용


#### @tanstack/react-query 
기존 react-query import 문을 아래와 같이 변경한다.

v4 코드 예시
```
import { useQuery } from 'react-query';
```

v5 코드 예시
```
import { useQuery } from '@tanstack/react-query';
```



#### Supports a single signature, one object
useQuery and friends used to have many overloads in TypeScript: different ways how the function could be invoked. Not only was this tough to maintain, type wise, it also required a runtime check to see which types the first and the second parameter were, to correctly create options.

now we only support the object format.
```
useQuery(key, fn, options)
useQuery({ queryKey, queryFn, ...options })
useInfiniteQuery(key, fn, options)
useInfiniteQuery({ queryKey, queryFn, ...options })
useMutation(fn, options)
useMutation({ mutationFn, ...options })
useIsFetching(key, filters)
useIsFetching({ queryKey, ...filters })
useIsMutating(key, filters)
useIsMutating({ mutationKey, ...filters })
```


#### Callbacks on useQuery (and QueryObserver) have been removed
onSuccess, onError and onSettled have been removed from Queries. They haven't been touched for Mutations. Please see [this RFC](https://github.com/TanStack/query/discussions/5279) for motivations behind this change and what to do instead.

onSuccess, onError, onSettled 코드는 아래와 같이 대체 한다.

v4 코드
```
useQuery({
  queryKey: ['user', userId],
  queryFn: fetchUser,
  onSuccess: (data) => {
    console.log('Data loaded:', data);
  },
  onError: (error) => {
    console.error('Error fetching user:', error);
  },
  onSettled: () => {
    console.log('Query is done (success or error)');
  },
});

```

v5 코드
```
const { data, error } = useQuery({
  queryKey: ['user', userId],
  queryFn: fetchUser,
});

useEffect(() => {
  if (data !== undefined || error !== null) {
    console.log('Query is done (success or error)');
  }
}, [data, error]);

// OR

useEffect(() => {
  if (data) {
    console.log('Data loaded:', data);
  }
}, [data]);

useEffect(() => {
  if (error) {
    console.error('Error fetching user:', error);
  }
}, [error]);

```

The only good use-case that came out of the twitter discussion was scrolling a feed to the bottom when a new chat message arrived. That _is_ a good example, similar to animating an item if a fetch failed, because you'd want that to be different on a per-component level. Still, those cases are the minority by far, and if that's what you want to do, you can achieve the same thing with `useEffect`. `data` and `error` returned from `useQuery` are referentially stable, so you can set up an effect pretty easily without having to worry about it running too often.

APIs need to be simple, intuitive and consistent. The callbacks on `useQuery` look like they fit these criteria, but they are bug-producers in disguise. It's pretty bad because they will likely do what you want when you first implement them, but they have a toll when you refactor or extend your App as it grows. They also invite antipatterns because you can introduce error-prone state-syncing without feeling bad while doing so.

Almost all examples I've seen can and will break in some cases, and I've seen these cases be reported on a regular basis in issues, discussions, and discord or stackoverflow questions. Those bugs are painfully hard to track. Don't believe me? I'll leave it to [Theo](https://twitter.com/theo) then:





# 옵션사항
#### Rename cacheTime to gcTime
Almost everyone gets cacheTime wrong. It sounds like "the amount of time that data is cached for", but that is not correct.

cacheTime does nothing as long as a query is still in use. It only kicks in as soon as the query becomes unused. After the time has passed, data will be "garbage collected" to avoid the cache from growing.

gc is referring to "garbage collect" time. It's a bit more technical, but also a quite [well known abbreviation](https://en.wikipedia.org/wiki/Garbage_collection_\(computer_science\)) in computer science.
```
const MINUTE = 1000 * 60;

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
-      cacheTime: 10 * MINUTE,
+      gcTime: 10 * MINUTE,
    },
  },
})
```


#### The useErrorBoundary option has been renamed to throwOnError
To make the useErrorBoundary option more framework-agnostic and avoid confusion with the established React function prefix "use" for hooks and the "ErrorBoundary" component name, it has been renamed to throwOnError to more accurately reflect its functionality.

```
const todos = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodos,
-  useErrorBoundary: true,
+  throwOnError: true,
})
```

#### TypeScript: Error is now the default type for errors instead of unknown

](https://tanstack.com/query/v5/docs/framework/react/guides/migrating-to-v5#typescript-error-is-now-the-default-type-for-errors-instead-of-unknown)
Even though in JavaScript, you can throw anything (which makes unknown the most correct type), almost always, Errors (or subclasses of Error) are thrown. This change makes it easier to work with the error field in TypeScript for most cases.

If you want to throw something that isn't an Error, you'll now have to set the generic for yourself:
```
useQuery<number, string>({
  queryKey: ['some-query'],
  queryFn: async () => {
    if (Math.random() > 0.5) {
      throw 'some error'
    }
    return 42
  },
})
```

For a way to set a different kind of Error globally, see [the TypeScript Guide](https://tanstack.com/query/v5/docs/framework/react/typescript#registering-a-global-error).



### Removed keepPreviousData in favor of placeholderData identity function

We have removed the keepPreviousData option and isPreviousData flag as they were doing mostly the same thing as placeholderData and isPlaceholderData flag.

To achieve the same functionality as keepPreviousData, we have added previous query data as an argument to placeholderData which accepts an identity function. Therefore you just need to provide an identity function to placeholderData or use the included keepPreviousData function from Tanstack Query.

> A note here is that useQueries would not receive previousData in the placeholderData function as argument. This is due to a dynamic nature of queries passed in the array, which may lead to a different shape of result from placeholder and queryFn.

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
} = useQuery({
  queryKey,
  queryFn,
- keepPreviousData: true,
+ placeholderData: keepPreviousData
});
```


#### Infinite queries now need a initialPageParam
Previously, we've passed undefined to the queryFn as pageParam, and you could assign a default value to the pageParam parameter in the queryFn function signature. This had the drawback of storing undefined in the queryCache, which is not serializable.

Instead, you now have to pass an explicit initialPageParam to the infinite query options. This will be used as the pageParam for the first page:

useInfiniteQuery 는 ({ pageParam = 0 }) 에 있는 수치를 initialPageParam: 0 속성으로 수정해라!
아래 예제 코드를 참고해

```
useInfiniteQuery({
   queryKey,
-  queryFn: ({ pageParam = 0 }) => fetchSomething(pageParam),
+  queryFn: ({ pageParam }) => fetchSomething(pageParam),
+  initialPageParam: 0,
   getNextPageParam: (lastPage) => lastPage.next,
})
```

#### status: loading has been changed to status: pending and isLoading has been changed to isPending and isInitialLoading has now been renamed to isLoading

The loading status has been renamed to pending, and similarly the derived isLoading flag has been renamed to isPending.

For mutations as well the status has been changed from loading to pending and the isLoading flag has been changed to isPending.

Lastly, a new derived isLoading flag has been added to the queries that is implemented as isPending && isFetching. This means that isLoading and isInitialLoading have the same thing, but isInitialLoading is deprecated now and will be removed in the next major version.

To understand the reasoning behind this change checkout the [v5 roadmap discussion](https://github.com/TanStack/query/discussions/4252).



# 주요사항
#### new hooks for suspense
With v5, suspense for data fetching finally becomes "stable". We've added dedicated useSuspenseQuery, useSuspenseInfiniteQuery and useSuspenseQueries hooks. With these hooks, data will never be potentially undefined on type level:

suspense: true 설정을 보면, useQuery 를 useSuspenseQuery 설정으로 변경해라

v4 코드
```
const { data } = useQuery({
  queryKey: ['user', userId],
  queryFn: () => fetchUser(userId),
  suspense: true,
});
```

v5 코드
```
import { useSuspenseQuery } from '@tanstack/react-query';

const { data } = useSuspenseQuery({
  queryKey: ['user', userId],
  queryFn: () => fetchUser(userId),
});
```

v4 코드
```
const {
  data,
  fetchNextPage,
  hasNextPage,
  isFetchingNextPage,
} = useInfiniteQuery({
  queryKey: ['comments', postId],
  queryFn: ({ pageParam = 0 }) => fetchComments(postId, pageParam),
  getNextPageParam: (lastPage) => lastPage.nextCursor,
  suspense: true,
});
```

v5
```
import { useSuspenseInfiniteQuery } from '@tanstack/react-query';

const {
  data,
  fetchNextPage,
  hasNextPage,
  isFetchingNextPage,
} = useSuspenseInfiniteQuery({
  queryKey: ['comments', postId],
  queryFn: ({ pageParam }) => fetchComments(postId, pageParam),
  initialPageParam: 0,
  getNextPageParam: (lastPage) => lastPage.nextCursor,
});
```

v4
```
const results = useQueries({
  queries: [
    {
      queryKey: ['user', userId],
      queryFn: () => fetchUser(userId),
      suspense: true,
    },
    {
      queryKey: ['posts', userId],
      queryFn: () => fetchPosts(userId),
      suspense: true,
    },
  ],
});
```

v5
```
import { useSuspenseQueries } from '@tanstack/react-query';

const results = useSuspenseQueries({
  queries: [
    {
      queryKey: ['user', userId],
      queryFn: () => fetchUser(userId),
    },
    {
      queryKey: ['posts', userId],
      queryFn: () => fetchPosts(userId),
    },
  ],
});
```



The experimental suspense: boolean flag on the query hooks has been removed.
You can read more about them in the [suspense docs](https://tanstack.com/query/v5/docs/framework/react/guides/suspense).

When using suspense mode, status states and error objects are not needed and are then replaced by usage of the React.Suspense component (including the use of the fallback prop and React error boundaries for catching errors). Please read the [Resetting Error Boundaries](https://tanstack.com/query/v5/docs/framework/react/guides/suspense#resetting-error-boundaries) and look at the [Suspense Example](https://tanstack.com/query/v5/docs/framework/react/examples/suspense) for more information on how to set up suspense mode.

If you want mutations to propagate errors to the nearest error boundary (similar to queries), you can set the throwOnError option to true as well.

Enabling suspense mode for a query:

```
import { useSuspenseQuery } from '@tanstack/react-query'

const { data } = useSuspenseQuery({ queryKey, queryFn })
```

This works nicely in TypeScript, because data is guaranteed to be defined (as errors and loading states are handled by Suspense- and ErrorBoundaries).

On the flip side, you therefore can't conditionally enable / disable the Query. This generally shouldn't be necessary for dependent Queries because with suspense, all your Queries inside one component are fetched in serial.

placeholderData also doesn't exist for this Query. To prevent the UI from being replaced by a fallback during an update, wrap your updates that change the QueryKey into [startTransition](https://react.dev/reference/react/Suspense#preventing-unwanted-fallbacks).

[

### throwOnError default

](https://tanstack.com/query/v5/docs/framework/react/guides/suspense#throwonerror-default)

Not all errors are thrown to the nearest Error Boundary per default - we're only throwing errors if there is no other data to show. That means if a Query ever successfully got data in the cache, the component will render, even if data is stale. Thus, the default for throwOnError is:

```
throwOnError: (error, query) => typeof query.state.data === 'undefined'
```

Since you can't change throwOnError (because it would allow for data to become potentially undefined), you have to throw errors manually if you want all errors to be handled by Error Boundaries:

tsx

```
import { useSuspenseQuery } from '@tanstack/react-query'

const { data, error, isFetching } = useSuspenseQuery({ queryKey, queryFn })

if (error && !isFetching) {
  throw error
}

// continue rendering data
```


# 예외사항
1번째 suspense: true 인 옵션 코드가 있으면, `useSuspenseQuery`, `useSuspenseInfiniteQuery`, `useSuspenseQueries` 중에 하나로 적용해라
2번째 suspense: true 이지만 enabled 옵션 코드가 있으면 `useQuery`로 사용해라 그리고 기존 suspense, enabled을 그대로 둬라



# useQueryWithInitParam 학습사항

suspense속성이 있다면, suspense 속성은 제거 해줘
keepPreviousData이 있다면, 기존 keepPreviousData이 placeholderData: keepPreviousData 로 수정해줘

```typescript
const { data, isFetching } = useQueryWithInitParam( 
	[endpointKeys.CLIENT__GET_ASSET_STORAGES_LIST, tab, queryParams],
	() => match(tab)
		.with('STORAGE', () => api.client.asset.getAssetStorageList(queryParams))
		.otherwise(() => undefined),
	{
		keepPreviousData: true,
		initParams: ['pagination'],
	},
);

```

```typescript
import {
   useQuery,
   keepPreviousData
} from "@tanstack/react-query";

const { data, isFetching } = useQueryWithInitParam( 
	[endpointKeys.CLIENT__GET_ASSET_STORAGES_LIST, tab, queryParams],
	() => match(tab)
		.with('STORAGE', () => api.client.asset.getAssetStorageList(queryParams))
		.otherwise(() => undefined),
	{
		placeholder: keepPreviousData,
		initParams: ['pagination'],
	},
);

```