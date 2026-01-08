# 고민의 시작

얼마전 [([[기록)_비동기와_ErrorBoundary]])] 에 대한 경험을 기록한 내용이 있다. [(ErrorBoundary)] 는

- 이벤트 헨들러에서 발생한 에러
- 비동기 코드 에러
- 서버사이드에서 발생한 에러
- 에러 바운더리 내부에서 나는 에러

들은 처리하지 못한다.

그래서,

라우터 이동 시 발생할 수 있는 비동기 처리들에 대해 상위에서 감싸고 그에 따른 에러를 처리하도록 구현을 했다.

# Router 계층의 에러 처리

하지만!!! react-router 의 context 에 errorElememt(https://reactrouter.com/api/components/Route#errorelement) 에 라우터 에러에 대한 일반적인 처리를해줄 수가 있다.

```tsx
const router = createBrowserRouter([
  {
    path: "/",
    element: <RootPage />,
    errorElement: <ErrorPage />, // 라우팅 에러 처리
    loader: async () => {
      const response = await fetch("/api/data");
      if (!response.ok) {
        throw new Error("Failed to fetch"); // ErrorPage로 이동
      }
      return response.json();
    },
    children: [
      // ... 라우트 설정
    ],
  },
]);
```

- 라우팅 관련 에러를 라우터에서 처리 할 수 있다. (404, 403 등등에 대한 에러도 router 에서 정의가능)
- loader 나 action 에서 throw 된 에러
- 잘못된 URL 접근

등등의 처리에 사용된다고 한다.

# 최신버전에서의 Await

렌더링 계층도 더 세부적으로 나눌 수 있을 것 같다.

- ErrorBoundary 렌더링 최상위층에서 관리
- 각 컴포넌트 렌더링 에러 관리

Suspense 와 함께 쓰는 Await 라는 개념도 react-router 최신 버전에서 제공되고 있다. 이때 여기에서도 errorElement 를 제공해준다.

```tsx
import { Await, useLoaderData } from "react-router";

export async function loader() {
  // not awaited
  const reviews = getReviews();
  // awaited (blocks the transition)
  const book = await fetch("/api/book").then((res) => res.json());
  return { book, reviews };
}

function Book() {
  const { book, reviews } = useLoaderData();
  return (
    <div>
      <h1>{book.title}</h1>
      <p>{book.description}</p>
      <React.Suspense fallback={<ReviewsSkeleton />}>
        <Await
          resolve={reviews}
          errorElement={<div>Could not load reviews 😬</div>}
          children={(resolvedReviews) => <Reviews items={resolvedReviews} />}
        />
      </React.Suspense>
    </div>
  );
}
```

https://reactrouter.com/api/components/Await

에러의 비동기 처리에 대한 각 컴포넌트 영역에서 에러 처리를 할 수 있어보인다.

# 정리해보면

프론트에서는 에러 처리가 크게는 3계층으로 나눠서 구축할 수 있을 것 같다!

1. 라우터 계층
   1. 라우팅과 비동기 데이터 처리에 대한 에러
2. 렌더링 계층
   1. 최상위 계층 Errorboundary 에서 에러 처리
   2. 각 컴포넌트에서 Await + Suspense 에서 에러 처리

## 참고자료

---

https://lim-2.tistory.com/63
