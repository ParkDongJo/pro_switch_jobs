
## Animated 기본
Animated 는 커스텀 하기 좋은 컴포넌트이며, RN 에서 애니메이션 구현을 위해 제공해주는 애니메이션 컴포넌트 중 하나이다.

기본적인 사용법은 아래와 같다

```javascript
export default App() {
	//  움직이기 전 값 설정
	const opacityAnim = useRef(new Animated.Value(1)).current

	// 어떤 인터렉션을 줄 것인지 선언
	// start()
	const handleClick = () => {
		Animated.timing(opacityAnim, {
			toValue: 0,
			useNativeDriver: true,
		}).start();
	}

	return (
		<>
			<Button title="button" onPress={handleClick} />
			<!-- 움직일 대상 선언 -->
			<Animated.Text style={{opacity: opacityAnim}}>X</Animated.Text>
		</>
	)
}
```


## Animated 인터렉션
- timing\
- spring
- decay

## Animated 결합
- sequence
- delay
- parallel
- stagger

## Animated 그 외
- 사칙연산
	- add
	- subtract
	- divide
	- multiply
- 핸들러
	- start
	- reset
	- loop

## Animated 보간법
- interpolate