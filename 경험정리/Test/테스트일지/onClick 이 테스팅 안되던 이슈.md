
## 현상
onClick 을 테스트 했을 때, onClick 에 대한 이벤트가 먹히지 않았다. handler 에 console.log 를 찍어봐도 찍히지 않는걸 보면, 아예 이벤트 발생이 막히는 걸로 보였다.

## 원인
vitest.setup.ts 에 dispatchEvent 에 대한 모킹을 했었다. React의 onClick은 내부적으로 dispatchEvent를 사용하여 DOM에 이벤트를 전파한다. 그런데 이걸 vi.fn()으로 덮어씌우면, 클릭 이벤트는 **“전파되었지만 아무 일도 일어나지 않는 것처럼 보임”** 상태가 된다.

```typescript
Object.defineProperty(HTMLElement.prototype, 'dispatchEvent', {
 value: vi.fn(),
 writable: true,
});
```


## 방향
전역 모킹은 신중하게 해야한다.