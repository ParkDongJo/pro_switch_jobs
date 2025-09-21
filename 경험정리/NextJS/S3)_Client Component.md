
이 내용 다시 정리하기
## 'use client' 를 사용하면
- full page load 할 때
	- Sever Component 의 render 과정과 거의 유사하다.
- subsequent navigation
	- JavaScript 코드를 파싱해서 

NextJS 는 아래가 모두 가능
- SSG
- SSR


ClientPageRoot 라는 값을 사용해서 초기화가 됨





아래 내용 정리하기
---
# Next.js 15의 Server Component와 Client Component 완벽 정리

## 기본 개념

Next.js에서 **레이아웃과 페이지는 기본적으로 Server Component**입니다. 이를 통해 서버에서 데이터를 가져오고 UI의 일부를 렌더링한 후, 선택적으로 결과를 캐시하고 클라이언트로 스트리밍할 수 있습니다. 사용자 상호작용이나 브라우저 API가 필요한 경우에는 Client Component를 사용하여 기능을 추가할 수 있습니다.

## 언제 어떤 컴포넌트를 사용해야 할까?

### Client Component를 사용해야 할 때:

- **상태와 이벤트 핸들러가 필요할 때**: onClick, onChange 등
- **라이프사이클 로직이 필요할 때**: useEffect 등
- **브라우저 전용 API를 사용할 때**: localStorage, window, Navigator.geolocation 등
- **커스텀 훅을 사용할 때**

### Server Component를 사용해야 할 때:

- **데이터베이스나 API에서 데이터를 가져올 때** (데이터 소스와 가까운 곳에서)
- **API 키, 토큰 등의 비밀 정보를 사용할 때** (클라이언트에 노출하지 않고)
- **브라우저로 전송되는 JavaScript 양을 줄이고 싶을 때**
- **First Contentful Paint(FCP)를 개선하고 콘텐츠를 점진적으로 스트리밍하고 싶을 때**

### 실제 예시

```typescript
// app/[id]/page.tsx - Server Component
import LikeButton from '@/app/ui/like-button'
import { getPost } from '@/lib/data'

export default async function Page({
  params,
}: {
  params: Promise<{ id: string }>
}) {
  const { id } = await params
  const post = await getPost(id)
  
  return (
    <div>
      <main>
        <h1>{post.title}</h1>
        <LikeButton likes={post.likes} />
      </main>
    </div>
  )
}

// app/ui/like-button.tsx - Client Component
'use client'

import { useState } from 'react'

export default function LikeButton({ likes }: { likes: number }) {
  // 클라이언트 측 상호작용 처리
}
```

## Server와 Client Component는 어떻게 작동하나?

### 서버에서의 동작

Next.js는 서버에서 React API를 사용하여 렌더링을 조율합니다. 렌더링 작업은 개별 라우트 세그먼트(레이아웃과 페이지)별로 청크로 분할됩니다:

- Server Component는 **React Server Component Payload (RSC Payload)**라는 특별한 데이터 형식으로 렌더링됩니다
- Client Component와 RSC Payload는 HTML을 사전 렌더링하는 데 사용됩니다

**RSC Payload란?** RSC Payload는 렌더링된 React Server Components 트리의 압축된 바이너리 표현입니다. 이는 다음을 포함합니다:

- Server Component의 렌더링 결과
- Client Component가 렌더링되어야 할 위치의 플레이스홀더와 JavaScript 파일 참조
- Server Component에서 Client Component로 전달되는 모든 props

### 클라이언트에서의 동작 (첫 로드)

1. **HTML**이 사용자에게 빠른 비대화형 프리뷰를 즉시 보여줍니다
2. **RSC Payload**가 Client와 Server Component 트리를 조정하는 데 사용됩니다
3. **JavaScript**가 Client Component를 하이드레이션하여 애플리케이션을 대화형으로 만듭니다

**하이드레이션이란?** 하이드레이션은 React가 DOM에 이벤트 핸들러를 연결하여 정적 HTML을 대화형으로 만드는 과정입니다.

### 이후 네비게이션

- RSC Payload가 사전 가져오기되고 캐시되어 즉각적인 네비게이션이 가능합니다
- Client Component는 서버에서 렌더링된 HTML 없이 완전히 클라이언트에서 렌더링됩니다

## 실제 사용 예시들

### Client Component 만들기

파일 상단, import 구문 위에 `"use client"` 지시문을 추가하여 Client Component를 만들 수 있습니다:

```typescript
'use client'

import { useState } from 'react'

export default function Counter() {
  const [count, setCount] = useState(0)
  
  return (
    <div>
      <p>{count} likes</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  )
}
```

`"use client"`는 Server와 Client 모듈 그래프(트리) 간의 경계를 선언하는 데 사용됩니다. 파일이 `"use client"`로 표시되면, 모든 import와 자식 컴포넌트가 클라이언트 번들의 일부로 간주됩니다. 따라서 클라이언트용 모든 컴포넌트에 지시문을 추가할 필요는 없습니다.

### JavaScript 번들 크기 줄이기

클라이언트 JavaScript 번들 크기를 줄이려면, UI의 큰 부분을 Client Component로 표시하는 대신 특정 대화형 컴포넌트에만 `'use client'`를 추가하세요.

예시: Layout 컴포넌트는 대부분 정적 요소(로고, 네비게이션 링크)를 포함하지만 대화형 검색 바를 포함합니다:

```typescript
// app/layout.tsx - Server Component
import Search from './search'  // Client Component
import Logo from './logo'      // Server Component

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <>
      <nav>
        <Logo />
        <Search />
      </nav>
      <main>{children}</main>
    </>
  )
}

// app/ui/search.tsx - Client Component
'use client'

export default function Search() {
  // 대화형 검색 기능
}
```

### Server에서 Client Component로 데이터 전달

props를 사용하여 Server Component에서 Client Component로 데이터를 전달할 수 있습니다:

```typescript
// Server Component
import LikeButton from '@/app/ui/like-button'
import { getPost } from '@/lib/data'

export default async function Page({
  params,
}: {
  params: Promise<{ id: string }>
}) {
  const { id } = await params
  const post = await getPost(id)
  
  return <LikeButton likes={post.likes} />
}

// Client Component
'use client'

export default function LikeButton({ likes }: { likes: number }) {
  // likes prop 사용
}
```

**중요**: Client Component로 전달되는 props는 React에서 직렬화 가능해야 합니다.

### Server와 Client Component 교차 사용

Server Component를 Client Component의 prop으로 전달할 수 있습니다. 이를 통해 서버에서 렌더링된 UI를 Client Component 내에 시각적으로 중첩할 수 있습니다.

일반적인 패턴은 `children`을 사용하여 Client Component에 슬롯을 만드는 것입니다:

```typescript
// app/ui/modal.tsx - Client Component
'use client'

export default function Modal({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>
}

// app/page.tsx - Server Component
import Modal from './ui/modal'
import Cart from './ui/cart'

export default function Page() {
  return (
    <Modal>
      <Cart />  {/* Server Component */}
    </Modal>
  )
}
```

이 패턴에서 모든 Server Component는 props로 전달된 것을 포함하여 서버에서 미리 렌더링됩니다.

### Context Provider 사용하기

React context는 일반적으로 현재 테마와 같은 전역 상태를 공유하는 데 사용되지만, **Server Component에서는 React context가 지원되지 않습니다**.

context를 사용하려면 children을 받는 Client Component를 만들어야 합니다:

```typescript
// app/theme-provider.tsx - Client Component
'use client'

import { createContext } from 'react'

export const ThemeContext = createContext({})

export default function ThemeProvider({
  children,
}: {
  children: React.ReactNode
}) {
  return <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>
}

// app/layout.tsx - Server Component
import ThemeProvider from './theme-provider'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html>
      <body>
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  )
}
```

**팁**: Provider는 트리에서 가능한 한 깊은 곳에 렌더링해야 합니다. ThemeProvider가 전체 `<html>` 문서 대신 `{children}`만 감싸는 것에 주목하세요. 이렇게 하면 Next.js가 Server Component의 정적 부분을 더 쉽게 최적화할 수 있습니다.

### 타사 컴포넌트 사용하기

클라이언트 전용 기능에 의존하는 타사 컴포넌트를 사용할 때는 Client Component로 래핑하여 예상대로 작동하도록 할 수 있습니다.

예시: `acme-carousel` 패키지의 `<Carousel />` 컴포넌트가 `useState`를 사용하지만 `"use client"` 지시문이 없는 경우:

```typescript
// 직접 래핑하는 방법
'use client'

import { Carousel } from 'acme-carousel'

export default Carousel

// 이제 Server Component에서 사용 가능
import Carousel from './carousel'

export default function Page() {
  return (
    <div>
      <p>View pictures</p>
      <Carousel />
    </div>
  )
}
```

**라이브러리 작성자를 위한 조언**: 컴포넌트 라이브러리를 만들고 있다면, 클라이언트 전용 기능에 의존하는 진입점에 `"use client"` 지시문을 추가하세요. 이렇게 하면 사용자가 래퍼를 만들 필요 없이 Server Component로 컴포넌트를 가져올 수 있습니다.

## 환경 오염 방지

JavaScript 모듈은 Server와 Client Component 모듈 간에 공유될 수 있습니다. 이는 실수로 서버 전용 코드를 클라이언트로 가져올 수 있다는 의미입니다.

```typescript
// lib/data.ts - 문제가 있는 코드
export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      authorization: process.env.API_KEY,  // 클라이언트에 노출되면 안 됨!
    },
  })
  
  return res.json()
}
```

Next.js에서는 `NEXT_PUBLIC_` 접두사가 붙은 환경 변수만 클라이언트 번들에 포함됩니다. 접두사가 없는 변수는 Next.js가 빈 문자열로 대체합니다.

### server-only 패키지 사용

Client Component에서 실수로 사용하는 것을 방지하려면 `server-only` 패키지를 사용할 수 있습니다:

```javascript
// lib/data.js
import 'server-only'

export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      authorization: process.env.API_KEY,
    },
  })
  
  return res.json()
}
```

이제 이 모듈을 Client Component로 가져오려고 하면 빌드 시간 오류가 발생합니다.

반대로 `client-only` 패키지는 window 객체에 접근하는 코드와 같은 클라이언트 전용 로직을 포함하는 모듈을 표시하는 데 사용할 수 있습니다.

```bash
# 설치 (선택사항)
pnpm add server-only
```

Next.js에서는 `server-only`나 `client-only` 설치가 선택사항이지만, 린팅 규칙이 불필요한 종속성을 플래그하는 경우 문제를 피하기 위해 설치할 수 있습니다. Next.js는 내부적으로 이러한 import를 처리하여 모듈이 잘못된 환경에서 사용될 때 더 명확한 오류 메시지를 제공합니다.