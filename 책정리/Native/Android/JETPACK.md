
JETPACK 이라는건 뭐지?


# Data binding

안드로이드를 개발할 때, 코드 아키텍쳐 하면, 대표적으로 MVVM 패턴 MVC 패턴 등등의 개념을 접하게 된다.
프론트에서 Data binding 이라고 하면, Angular 초기 버전에서 부터 View 에 data 에 대한 구독을 시켜서 렌더링을 하게 되는 흐름을 명칭한다.

안드로이드에서도 마찬가지로 보인다.
특이 Data binding 은 Jetpack 이라는 라이브러리를 통해서 더 쉽고 빠르게 구현이 가능해 보인다.

### 양방향 데이터 결합
Android 에서 Activity를 생성해서 View 와 연동하여, 데이터를 보여주는 방식은 아래와 같다

Activity <-> XML

이 때, 기능이 커질수록 코드는 View 와 관련된 코드와 로직과 관련된 코드가 뒤섞이게 된다.

ViewModel <-> Activity <-> XML

이때 View 와 관련된 데이터와 데이터제어를 ViewModel 이라는 곳에 응집시키고, 이를 Activity 에서 view에 1:1 매칭을 시킨다.

이에 대해 Jetpack 이라는 라이브러리를 통해, 프레임워크 차원에서 간단한 설정과 코드만 구축하면 자동으로 셋팅이 가능하도록 도와주는 거 같다.

안드로이도.. 참 많이 발전한거 같다.
