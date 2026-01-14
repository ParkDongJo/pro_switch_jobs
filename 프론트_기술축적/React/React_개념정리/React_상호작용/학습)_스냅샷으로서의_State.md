State 는 스냅샷 처럼 동작한다.

State 를 설정(변경)하면, 렌더링을 일으킵니다.

return 문을 통해 반환하는 JSX는 시간상 UI의 스냅샷과 같다.

# 렌더링은 그 시점의 스냅샷을 찍습니다.

- React가 함수를 다시 호출합니다.
- 함수가 새로운 JSX 스냅샷을 반환합니다.
- 그러면 React가 함수가 반환한 스냅샷과 일치하도록 화면을 업데이트합니다

실제로 스냅샷이 찍히는지 확인하기 위해, 이벤트 핸들러를 활용해보자

```tsx
export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button
        onClick={() => {
          setNumber(number + 5);

          // setTimeout 호출 시점에 그 시점의 state 를 스냅샷 을 저장하고 있다가 실행함
          setTimeout(() => {
            alert(number);
          }, 3000);
        }}
      >
        +5
      </button>
    </>
  );
}
```

렌더링의 이벤트 핸들러 내에서 state 값을 “고정”으로 유지한다. 리렌더링 전에 최신 state 를 읽고 싶다면,
state update 함수를 사용하면 된다.
