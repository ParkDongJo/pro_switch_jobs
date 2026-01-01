# 학습이유
API 로 데이터를 받아서 상태로 관리하는데, 이 상태를 가공하고 체크하는 코드가 많다. 이때 하나의 테두리로 묶을 수 있을 법한 코드들도 파편화되거나, 불필요하게 뭉쳐지거나 하는 경우도 발생한다.

경험상 이걸 특정 hook으로 wrapping 해서 api 호출할때 매번 가공을 하는 코드를 마주한 적이 있었는데, 마주할 때마다 뜯어고치고 싶은 충동이 있었던것 같다.


# 해결책
> _어댑터 패턴은 호환되지 않는 인터페이스를 가진 객체들이 협업할 수 있도록 하는 구조_
[https://refactoring.guru/design-patterns/adapter](https://refactoring.guru/design-patterns/adapter)

Adapter 패턴으로 

```typescript
export class Adapter {
	private static value: unknown;
	
	static from<Source>(originData: Source) {
		this.value = originData;
		return this;
	}
	
	static to<Input, Output>(mapperFn: (value: Input) => Output) {
		const transformed = mapperFn(this.value as Input);
		return tansformed;
	}
}

```

이걸 기반으로 

```typescript
type RawNotification = {
	id: string;
	createdTimeStamp: string;
	updatedTimeStamp: string;
	typeId: number;
	statusId: number;
	message: string;
	rawMetadata: string;
}

export interface RawNotification {
  id: number;
  message: string;
  statusId: number;
  typeId: number;
  createdTimestamp: string | number;
}

export class NotificationAdapter {
  private value: RawNotification;

  constructor(obj: RawNotification) {
    this.value = obj;
  }

  get statusText(): string {
    switch (this.value.statusId) {
      case 1:
        return 'Info';
      case 2:
        return 'Success';
      case 3:
        return 'Warning';
      case 4:
        return 'Error';
      default:
        return 'Default';
    }
  }

  get typeText(): string {
    switch (this.value.typeId) {
      case 1:
        return 'Email';
      case 2:
        return 'SMS';
      case 3:
        return 'Push';
      default:
        return 'Default';
    }
  }

  get createdDateFormatted(): string {
    return new Date(this.value.createdTimestamp).toLocaleString();
  }

  adapt() {
    return {
      id: this.value.id,
      message: this.value.message,
      statusText: this.statusText,
      typeText: this.typeText,
      createdDate: this.createdDateFormatted,
    };
  }
}
```


생성자로 data 를 받고, 객체에 저장해두고 있다가 가공된 데이터를 꺼낼때는 Adapter 에서 꺼내어 쓰는 방식입니다.


```tsx
import React from 'react';
import { useQuery } from '@tanstack/react-query';

import { UserApi } from './api';
import { Adapter } from './Adapter';
import { NotificationAdapter } from './NotificationAdapter';

import { Notification } from './components';


interface Props {
  userId: number | string;
}
export const NotificationScreen: React.FC<Props> = ({
  userId,
}) => {

  const { data } = useQuery({
    queryKey: ['notificationsById', userId],
    queryFn: () =>
      UserApi.getAllNotificationsById({
        id: userId,
      }),
    select: (data) =>
		// select를 통해서 Adapter로 가공된 상태로 데이터 반환
		Adapter.from(data).to(
		    (item) => new NotificationAdapter(item).adapt()
	    ),
  });

  if (!data) {
    return null;
  }

  return <Notification {...data} />;
};
```

# 테스트를 해볼점
- react-query 에서 뿐만이 아닌, nextjs 에서는 어떻게 활용하면 좋을까?
- date 통째로 참조 형태가 되는데, 데이터를 변경하고자 할때는 react 의 상태 관리 라이프사이크를 벗어나지 않을까?
	- fetch 를 해온 상태는 다시 refetch 를 해오거나, caching 한 로컬의 상태를 임시로 변경하지, 상태 자체를 건들지는 않았던 것 같다!
	- fetch 해온 상태는 그대로 두고 이를 활용해 UI를 제어하는 상태를 따로 둬서 제어하거나 했던거 같다!


# Next.js 적용
Next 로 적용해보면

- Next.js 14의 장점인 **Server Component 데이터 패칭**으로
    - 클라이언트 번들 줄고
    - API 키/토큰도 서버에서 안전하게 처리 가능
- 변환(Adapter)은 **서버에서 끝내고** UI는 깔끔하게 렌더링만

fetch 영역
```typescript
// lib/api/userApi.ts
import type { RawNotification } from '@/lib/adapter/NotificationAdapter';

export const UserApi = {
  async getAllNotificationsById(params: { id: number | string }) {
    // 예시 URL (프로젝트에 맞게 변경)
    const res = await fetch(
      `${process.env.NEXT_PUBLIC_API_BASE_URL}/users/${params.id}/notifications`,
      {
        // 사용자별 알림은 보통 자주 변하니 캐시 끄는 예시
        cache: 'no-store',
        // 또는 next: { revalidate: 10 } 처럼 ISR도 가능
      }
    );

    if (!res.ok) {
      throw new Error('Failed to fetch notifications');
    }

    // 서버에서 받는 원본 배열 형태라고 가정
    const data = (await res.json()) as RawNotification[];
    return data;
  },
};
```

서버 컴포넌트
```tsx
// app/users/[userId]/notifications/page.tsx
import { UserApi } from '@/lib/api/userApi';
import { Adapter } from '@/lib/adapter/Adapter';
import { NotificationAdapter } from '@/lib/adapter/NotificationAdapter';
import { NotificationList } from './ui/NotificationList';

export default async function NotificationsPage({
  params,
}: {
  params: { userId: string };
}) {
  const raw = await UserApi.getAllNotificationsById({ id: params.userId });

  const notifications = Adapter.from(raw).to(
    (item) => new NotificationAdapter(item).adapt()
  );

  // NotificationList는 Server/Client 어느쪽이어도 OK
  return <NotificationList notifications={notifications} />;
}
```

클라이언트 컴포넌트
```tsx
// app/users/[userId]/notifications/ui/NotificationList.tsx
'use client';

'use client';

import React from 'react';

export type Props = {
  notifications: {
    id: number;
    message: string;
    statusText: string;
    typeText: string;
    createdDate: string;
  }[];
};

export function NotificationList(props: Props) {
  const { notifications } = props;
  return (
    <div>
      {notifications.map((n) => (
        <div key={n.id}>{n.message}</div>
      ))}
    </div>
  );
}
```
- Adapter 의 type 과 UI type 은 분리
- 대신 서버에서 어댑터 결과 스키마가 바뀌면, **UI Props 정의도 같이 바꿔줘야** 합니다.