[관련 영상](https://www.youtube.com/watch?v=WZza2oISgJ8)
아래 테스트 코드를 보면 queryBy 와 getBy 의 차이를 알 수 있다.

요소가 존재하지 않을 때, 예외를 발생시킴 - getBy*
요소가 존재하지 않을 때, null을 반환함 - queryBy*

그래서 만약 존재하지 않는 컨테이너를 테스트 하고자 한다면, queryBy* 를 사용해야한다.

```typescript
expect(screen.getByRole('button', { name: 'OFF' })).toBeTheDocument();
// 존재하지 않음을 테스트 하기 때문에, queryBy* 를 쓴다.
// getBy* 를 쓰거나, not 을 빼면 테스트는 실패하게 된다.
expect(screen.queryByRole('button', { name: 'ON' })).not.toBeTheDocument();

```