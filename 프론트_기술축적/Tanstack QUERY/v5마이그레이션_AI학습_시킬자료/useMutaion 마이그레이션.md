
지금 부터 아래 경로의 파일을 찾아서, 그 파일안에 있는 useMutation 를 위에 학습내용에 따라 코드를 모두 수정해줘


useMutation 는 2025.7.17 기준 80 files 에 존재한다.
v
```
src/features/alarm/components/Notification/NotificationModules.tsx  
src/features/auth/components/PhoneVerifyInputs/VerifyCodeInput.tsx  
src/features/auth/components/PhoneVerifyInputs/View.tsx  
src/features/company/components/MyInfo/InfoEdit.tsx  
src/features/driver/components/Detail/EditDriverInfoDrawer.tsx  
src/features/driver/components/Invitation/DriverInviModal.tsx  
src/features/driver/components/Registration/Form/Form.tsx  
src/features/geoFence/components/Detail/Detail.tsx  
src/features/geoFence/components/Detail/Edit.tsx  
src/features/geoFence/components/List/List.tsx  
```

v
```
src/features/group/components/Management/Vehicle/Detail/Detail.tsx  
src/features/group/components/Management/Vehicle/Form/Edit.tsx  
src/features/group/components/Management/Vehicle/Form/Register.tsx  
src/features/incident/components/Download/Download.tsx  
src/features/incident/components/Report/Aside/Aside.tsx  
src/features/incident/components/Report/Contents/ReportDetail/Media.tsx  
src/features/incident/components/Report/Contents/ReportDetail/MediaUploadModal.tsx  
src/features/incident/components/Report/Footer/Footer.tsx  
src/features/incident/components/Report/Form/Form.tsx  
src/features/incident/components/Report/VideoTimeSync/VideoTimeSync.tsx  
```
v
```
src/features/incident/components/UnconfirmedIncident/UnconfirmedIncidentProcessorModal.tsx  
src/features/incident/components/View/DeleteConfirm.tsx  
src/features/incident/components/View/View.tsx  
src/features/inspection/component/panel/InspectionDetailPanel.tsx  
src/features/inspection/component/panel/InspectionRegisterPanel.tsx  
src/features/inspection/component/panel/ScheduleRegisterPanel.tsx  
src/features/installer/components/Matching/Vehicle/QR/FooterButton.tsx  
src/features/llm/components/AIAssistant.tsx  
src/features/llm/hooks/useAIAssistant.tsx  
src/features/logbook/components/DownloadDialog.tsx  
```
v
```
src/features/logbook/components/DownloadTable.tsx  
src/features/me/components/View.tsx  
src/features/me/components/AboutMeDialog/PwdResetDialog.tsx  
src/features/me/components/AboutMeDialog/WithdrawalDialog.tsx  
src/features/member/components/Invitation/InvitationForm.tsx  
src/features/member/components/Management/Detail.tsx  
src/features/member/components/Management/Edit.tsx  
src/features/member/components/Management/List.tsx  
src/features/member/components/Registration/Form/Form.tsx  
src/features/ota/components/AutoUpdate/Edit.tsx  
```
v
```
src/features/ota/components/AutoUpdate/Register.tsx  
src/features/ota/components/Management/View.tsx  
src/features/ota/components/ReleaseUpdate/Register.tsx  
src/features/policy/component/Management/View.tsx  
src/features/policy/component/Modal/PolicyApply.tsx  
src/features/policy/component/Modal/PolicyFileUpload.tsx  
src/features/policy/component/Panel/ApplyStatusPanel.tsx  
src/features/policy/component/Panel/DetailPanel.tsx  
src/features/region/component/Detail/View.tsx  
src/features/region/component/Register/View.tsx
```
v
```
src/features/rndTask/component/Detail/View.tsx  
src/features/rndTask/component/Detail/WorkLocation.tsx  
src/features/rndTask/component/Panel/RouteRegisterPanel.tsx  
src/features/rndTask/component/Panel/ScheduleEditPanel.tsx  
src/features/rndTask/component/Panel/ScheduleRegisterPanel.tsx  
src/features/rndTask/component/Register/View.tsx  
src/features/rndTask/component/Register/WorkLocation.tsx  
src/features/role/component/Detail/View.tsx  
src/features/role/component/Register/View.tsx  
src/features/route/component/Detail/View.tsx  
```
v
```
src/features/route/component/Register/View.tsx  
src/features/station/component/Detail/View.tsx  
src/features/station/component/Register/View.tsx  
src/features/support/components/Incident/TelContactLinkButton.tsx  
src/features/threat/component/Detection/View.tsx  
src/features/vehicle/components/Control/Registration/ControlCommand/List/CommandList.tsx  
src/features/vehicle/components/Management/Detail/Card/Operation/EditableMemo.tsx  
src/features/vehicle/components/Management/Detail/Card/VehicleMonitoring/AbnormalEvent/RemoteDiagnosis.tsx  
src/features/vehicle/components/Management/Edit/Information/CustomField.tsx  
src/features/vehicle/components/Management/Edit/Information/InformationEdit.tsx
```
v
```
src/features/vehicle/components/Management/Edit/Operation/OperationEdit.tsx  
src/features/vehicle/components/Management/List/DownloadListButton.tsx  
src/features/vehicle/components/Schedule/ScheduleDetailEdit.tsx  
src/features/vehicle/components/Schedule/ScheduleDetailView.tsx  
src/features/vehicle/components/Schedule/Registration/View.tsx  
src/features/vehicleMaintenance/component/Management/DetailPanel.tsx  
src/features/vehicleMaintenance/component/Management/ImageUploadModal.tsx  
src/features/vehicleMaintenance/component/Management/RegisterPanel.tsx  
src/features/vehicleMaintenance/component/Management/ScheduleRegisterPanel.tsx  
src/pages/Report/Logbook/LogbookFormPage.tsx  

```


# 학습 시킬 내용

#### 기존 react-query import 문을 아래와 같이 변경한다.

v4 코드 예시
```
import { useQuery } from 'react-query';
```

v5 코드 예시
```
import { useQuery } from '@tanstack/react-query';
```


#### useMutation 에 넘기는 param 들을 {} 로 감싸서 넘기도록 변경한다.


아래와 같이 {} 로 감싼다.
fn 은 mutationFn: fn 으로 변경한다.
```
- useMutation(fn, options)
+ useMutation({ mutationFn, ...options })
```

v4 코드 예시
```
useMutation(
  updateUser,
  {
	  onSuccess: (data) => {
	    console.log('Data loaded:', data);
	  },
	  onError: (error) => {
	    console.error('Error fetching user:', error);
	  },
	  onMutate: (data) => {
	    console.log('Data loaded:', data);
	  },
	  onSettled: () => {
	    console.log('Query is done (success or error)');
	  },
  }
);

```

v5 코드 예시
```
useMutation({
  mutationFn: updateUser,
  onSuccess: (data) => {
    console.log('Data loaded:', data);
  },
  onError: (error) => {
    console.error('Error fetching user:', error);
  },
  onMutate: () => {
  },
  onSettled: () => {
    console.log('Query is done (success or error)');
  },
});

```


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


#### isLoading 은 isPending 으로 변경

- `loading` 옵션이 `pending`으로 변경되었으며, 마찬가지로 `isLoading` 플래그가 `isPending`으로 변경되었습니다.

```tsx
isPending: boolean;
// A derived boolean from the status variable above, provided for convenience.
isSuccess: boolean;
// A derived boolean from the status variable above, provided for convenience.
isError: boolean;
// A derived boolean from the status variable above, provided for convenience.
```

- `mutation`의 경우에도 `isLoading` 플래그가 `isPending`으로 변경되었습니다.

v4
```
const { isLoading } = useMutation({
	....
})
```

v5
```
const { isPending } = useMutation({
	....
})

```

해당 파일 전반적으로 isLoading 을 isPending 으로 변경해라



# [[프롬프트]]

아래는 **React Query v4 → v5 마이그레이션**에서 useMutation 관련 변경사항을 GPT가 이해하고 코드 수정 자동화를 돕도록 설계된 프롬프트입니다. 문맥과 명세를 최대한 정확하게 전달하도록 구성했습니다.

---

### **✅ React Query v4 → v5** 

### **useMutation**

###  **마이그레이션 프롬프트**

  

**📌 프롬프트 내용:**

````
You are helping migrate React Query code from v4 to v5. Focus on transforming `useMutation` and related options according to the following specifications:

---

1. ✅ **Update import path**

Replace all `react-query` imports with `@tanstack/react-query`.

For example:
```diff
- import { useMutation } from 'react-query';
+ import { useMutation } from '@tanstack/react-query';
````

---

2. ✅ **Convert useMutation API to new object format**
    

  

In v4, useMutation takes parameters positionally:

```
useMutation(mutationFn, options)
```

In v5, it must be passed as an object:

```
useMutation({
  mutationFn,
  ...options
})
```

So this:

```
useMutation(updateUser, {
  onSuccess: ...,
  onError: ...,
})
```

Becomes:

```
useMutation({
  mutationFn: updateUser,
  onSuccess: ...,
  onError: ...,
})
```

---

3. ✅ **Rename isLoading → isPending**
    

  

Anywhere useMutation returns isLoading, rename it to isPending.

  

For example:

```
- const { isLoading } = useMutation(...);
+ const { isPending } = useMutation(...);
```

Also apply to object destructuring and JSX/logic using isLoading.

---

4. ✅ **Rename cacheTime → gcTime**
    

  

In QueryClient options or any query config:

```
- cacheTime: 10 * MINUTE,
+ gcTime: 10 * MINUTE,
```

---

5. ✅ **Rename useErrorBoundary → throwOnError**
    

  

Update query options like this:

```
- useErrorBoundary: true,
+ throwOnError: true,
```

---

💡 Apply all these changes wherever they appear in the code. Ensure syntax remains valid and formatting is preserved.

```
---

필요하다면 이 프롬프트를 영어로 또는 TypeScript-aware 코딩 툴에 맞춰 수정해줄 수도 있어. 다른 API (`useQuery`, `queryClient`, `hydration` 등) 마이그레이션 프롬프트도 필요하면 말해줘!
```