
panResponder 선언
panResponder panhandler


## panhandler

permission 메서드
- onStartShouldSetResponder
	- Start (onPanResponderStart) 메서드 사용 여부
- onMoveShouldSetResponder
	-  Move (onPanResponderMove) 메서드 사용 여부
response 메서드
- onPanResponderGrant
	- 터치 액션에 대한 응답의 정상 작동
- onPanResponderReject
	- 터치 액션에 대한 응답의 비정상 작동
handler 메서드
- onPanResponderStart
	- 터치 액션을 시작 할  때
- onPanResponderMove
	- 터치 액션을 움직일 때
- onPanResponderEnd
	- 터치 액션이 끝났을 때
- onPanResponderRelease
	- 터치 액션이 끝났을 때


이때, listner 로 들어오는 gestureState 값

- `stateID` - gestureState 의 ID
좌표
- `moveX` - 제일 최신에 찍힌 x 좌표
- `moveY` - 제일 최신에 찍힌 y 좌표
- `x0` -이동 직전의 x 좌표
- `y0` - 이동 직전의 y 좌표
누적  거리
- `dx` - 터치가 시작된 이후 누적된 제스쳐 x 거리
- `dy` -  터치가 시작된 이후 누적된 제스쳐 y 거리
제스처 속도
- vx - 제스처 x 속도
- `vy` - 제스처 y 속도
터치 갯수
- `numberActiveTouches` - 터치 갯수


moveX -  dx  = 'y로  움직인 거리'
moveY -  dy = 'x로  움직인 거리'




```javascript


```
