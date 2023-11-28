**목차**
- [this가 가르키는 곳](#this가 가르키는 곳)

## this가 가르키는 곳
------
this가 가르키는 곳을 찾을 때,
- 전역 this -> window
- 일반 함수 안에서 쓴 this -> window
- 메서드 안에서 쓴 this -> 종속된 클래스(Object)
- 생성자 안에서 쓴 this -> 생성한 객체
- 이벤트 핸들러 안에서 쓴 this -> 이벤트 핸들러를 등록해준 HTML 요소(또는 객체)
- arrow 함수 안에서 쓴 this -> 
- 명시적 바인딩을 한 this
