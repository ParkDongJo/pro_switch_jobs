관련 영상](https://www.youtube.com/watch?v=WZza2oISgJ8)

일반적인 상황에서 요소를 타겟팅 할때 -> getBy*
비동기로 요소를 타겟팅 할때 -> findBy*
- vitest 가 findBy* 로 요소가 DOM에 나타날 때 까지 기다려준다

아래 예시를 보면 바로 이해가 된다.

아래와 같은 경우는 userEvent 에 설계된 모든 함수는 비동기로 설계되어 있기 때문에, await 를 붙여줘야 한다.
그래서 click 이후 변화를 expect() 로도 충분히 감지 가능하다.

```typescript
test("button is disabled once clicked", async () => {
  const user = userEvent.setup();

  render(<Components />);

  const button = screen.getByRole("button");

  screen.debug(button); // 클릭 전
  await user.click(button); 
  screen.debug(button); // 클릭 후

  expect(button).toBeDisabled();
})

```


근데 이 버튼의 클릭이 아래와 같이 구현되어 있다고 가정해보자. 버튼의 클릭이벤트는 비동기로 동작한다.

```typescript
const handleClick = () => {
  setDisabled(true);

  const timer = setTimeout(() => {
    setOn(!on);
    setDisabled(false);
  }, 500);

  return () => clearTimeout(timer);
}

return (
  <button disabled={disalbed} onClick={handleClick}>
    {on ? "ON" : "OFF"}
  </button>
)
```

그때는 아래와 같이 findBy* 를 통해서 요소를 찾을 수 있다.

```typescript
test("ON button is enabled when clicked", async () => {
  const user = userEvent.setup();

  render(<Components />);

  await user.click(creen.getByRole("button")); 

  const button = await screen.findByRole("button", { name: "ON" });

  expect(button).toBeTheDocument();
  expect(button).toBeEnabled();
})

```


만약 여러 동작을 하고자 할때는 waitFor 이 더 용이하다.
