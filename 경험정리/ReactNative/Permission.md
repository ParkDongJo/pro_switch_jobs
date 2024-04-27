
네이티브에서는 위치, 카메라, 사진 파일 등등의 데이터나 기능에 접근하려면, 접근 권한에 대한 허용을 사용자에게 받아야 한다.
이때 대부분 앱들은 첫 설치 시 필요한 권한들을 요구하는 흐름으로 간다. 무조건 로그인을 해야만 앱을 사용할 수 있는 경우라면 다행이지만, 로그인 없이 몇몇 기능 이용이 가능한 앱들도 있다. 이때는 해당 화면에서도 권한 허용을 받을 수 있는 체크를 따로 해줘야 한다.

경험상 ReactNative 를 활용하면 react-native-permissions 로도 무리 없이 커버가 가능하지만, 간혼 Native 단에서 제어를 해야하는 경우도 있다. 하지만 특수한 상황일테니 이 부분은 다루지 말자.

## react-native-permissions

react-native-permission 만 사용하면 되겠네 라고 생각한다면, 큰 오산이다. ReactNative 의 가장 큰 단점중 하나는 바로 이런 주요기능들을 모두 커뮤니티에 맡기고 있다는 점이다.

react-native-permissions 도 개인이 관리하고 있는 라이브러리이기 때문에, 설명도 불친절하고 사용법 안내가 잘 안되어 있다. 정신 바짝차려야한다.

이참에 각 Native 권한을 정리해보자

https://adjh54.tistory.com/206
https://velog.io/@dalbodre_ari/%EC%9A%B0%EC%95%84%ED%95%98%EA%B2%8C-react-native-%EA%B6%8C%ED%95%9C-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0


## 주요 특장점

iOS
- 사용자가 한번이라도 권한 요청을 거절하는 경우 -> 앱에서 권한 재요청이 불가능
- 


## iOS 권한 종류

https://velog.io/@kindcode/Android-IOS-%EA%B6%8C%ED%95%9C-%EB%AA%A9%EB%A1%9D

## AOS 권한 종류


## Android 13 버전 이상 권한 이슈


https://adjh54.tistory.com/206
https://adjh54.tistory.com/465

https://adjh54.tistory.com/465#1.%20checkNotifications()%20%EB%A9%94%EC%84%9C%EB%93%9C%EB%A5%BC%20%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC%20%EA%B6%8C%ED%95%9C%20%ED%99%95%EC%9D%B8-1
