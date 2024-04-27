
네이티브에서는 위치, 카메라, 사진 파일 등등의 데이터나 기능에 접근하려면, 접근 권한에 대한 허용을 사용자에게 받아야 한다.
이때 대부분 앱들은 첫 설치 시 필요한 권한들을 요구하는 흐름으로 간다. 무조건 로그인을 해야만 앱을 사용할 수 있는 경우라면 다행이지만, 로그인 없이 몇몇 기능 이용이 가능한 앱들도 있다. 이때는 해당 화면에서도 권한 허용을 받을 수 있는 체크를 따로 해줘야 한다.

경험상 ReactNative 를 활용하면 react-native-permissions 로도 무리 없이 커버가 가능하지만, 간혼 Native 단에서 제어를 해야하는 경우도 있다. 하지만 특수한 상황일테니 이 부분은 다루지 말자.

https://arc.net/l/quote/eqcknfzq


## react-native-permissions
-----
react-native-permission 만 사용하면 되겠네 라고 생각한다면, 큰 오산이다. ReactNative 의 가장 큰 단점중 하나는 바로 이런 주요기능들을 모두 커뮤니티에 맡기고 있다는 점이다.

react-native-permissions 도 개인이 관리하고 있는 라이브러리이기 때문에, 설명도 불친절하고 사용법 안내가 잘 안되어 있다. 정신 바짝차려야한다.

이참에 각 Native 권한을 정리해보자

https://adjh54.tistory.com/206
https://velog.io/@dalbodre_ari/%EC%9A%B0%EC%95%84%ED%95%98%EA%B2%8C-react-native-%EA%B6%8C%ED%95%9C-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0


- granted
- [CASE2] 권한 상태가 수락되지 않은 상태
	- blocked
	- denied
	- imited
	- unavailable

## 주요 특장점
-----

아래 특징 정리는 2022년도 글을 참고하였다. (https://velog.io/@dalbodre_ari/%EC%9A%B0%EC%95%84%ED%95%98%EA%B2%8C-react-native-%EA%B6%8C%ED%95%9C-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0) 현재(2024) 기준 재확인은 분명 필요하다.

iOS
- 사용자가 한번이라도 권한 요청을 거절하는 경우 -> 앱에서 권한 재요청이 불가능
- iOS 13 버전 에서부터 위치 권한에 대한 흐름이 변경되었다. (이 부분은 따로 정리)
- iOS 13 버전 에서부터 일회성 권한 허용이 들어간다.

AOS
- 사용자가 거절을 해도, '다시 묻지 않기' 옵션을 선택하지 않는 이상 런타임에서 재요청 할 수 있다.
- 위치정보 권한이 포그라운드, 백그라운드로 나뉘어 있다.(이 부분은 따로 정리)
- Android 11 에서부터 일회성 권한 허용이 들어간다.
- Android 13 에서부터 파일 읽기,쓰기 권한 사용법이 바뀐다.
- Android 13 에서부터 알림 권한은 기본 셋팅이 '허용' 으로 되어 있지 않다.

나머지 각 특장점은 천천히 확인하면서 채워가자.


## 권한 종류
-----
권한 목록은 매 버전이 업데이트 될때마다 변경될 수 있다. 그래서 개별 블로그를 참고하기 보다는 공식 홈페이지를 필요할 때마다 확인하는 식으로 해야 학습이나 작업이 효율적으로 진행될 것 같다

### iOS 권한 종류
[iOS 권한 목록](https://developer.apple.com/documentation/bundleresources/information_property_list/protected_resources)

### AOS 권한 종류
[Android 권한 목록](https://developer.android.com/reference/android/Manifest.permission)



## react-native-permissions 사용법
----
사실 사용법을 정리하기에는 코드 보면서 하면 되기때문에 어려울 건 없다. 다만 친절하게 문서로 정리해주는 라이브러리가 아니다 보니, 약간의 좌충우돌을 해야한다.

현재 기준(2024.4)으로서는 아래와 같다.

1. 필요 권한 선언
2. 권한 체크
3. 권한 요청

사용법은 라이브러리에 example 을 참고하는 것이 가장 좋다.
https://github.com/zoontek/react-native-permissions/blob/master/example/src/App.tsx


1. 필요 권한 선언
react-native 에서 제공하는 Platform.select() 함수를 통해 각 OS 에 따라 필요한 권한만 Permission[] 형태로 리턴하게 한다.
```javascript

const PERMISSIONS_IOS = {...}
const PERMISSIONS_AOS = {...}

const PLATFORM_PERMISSIONS = Platform.select<
  typeof PERMISSIONS.ANDROID | typeof PERMISSIONS_IOS | typeof PERMISSIONS.WINDOWS | {}
>({
  android: PERMISSIONS_AOS,
  ios: PERMISSIONS_IOS,
  default: {},
});


```


2. 권한 체크
권한 체크는 개별 권한 체크와 멀티 체크가 있다.

- 개별 체크 - check()
	- 비동기 동작
- 멀티 체크 - checkMultiple()
	- 비동기 동작
	- 주로 첫 진입 시 멀티체크로 권한 체크를 하는 것이 좋다.

참고 코드
```javascript

```


3. 권한 요청
권한 요청도 개별 권한 요청과 멀티 요청이 있다.

- 개별 요청 - request()
	- 비동기 동작
- 멀티 요청 - requestMultiple()
	- 비동기 동작
	- 주로 첫 진입 시 멀티 체크 후 denied 되어 있는 목록을 받아서, 요청하는 방식으로 사용하는 것이 일반적이다.


**주의 사항**
알림인 Notification 같은 경우 따로 함수를 제공해주고 있다.

- 알림 체크 - checkNotification()
- 알림 요청 - requestNotification()

## Android 13 버전 이상 권한 이슈


https://adjh54.tistory.com/206
https://adjh54.tistory.com/465

https://adjh54.tistory.com/465#1.%20checkNotifications()%20%EB%A9%94%EC%84%9C%EB%93%9C%EB%A5%BC%20%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC%20%EA%B6%8C%ED%95%9C%20%ED%99%95%EC%9D%B8-1


## 앱 화면전환 (포그라운드 <-> 백그라운드)
-----
### AppState
https://reactnative.dev/docs/appstate

브라우저라는 통합된 환경에서 돌아가는 웹과 달리 앱은 하나의 OS 에 있는 여러 어플리케이션 중 하나이다. 그렇다보니 가끔 나의 앱에서 잠깐 나갔다가 들어오는 경우가 있다.

그때 포그라운드 -> 백그라운드 / 백그라운드 -> 포그라운드 로 전환하게 되는데, 이때 타이밍에서 특정 작업을 해야할 때가 있다.

다행히 ReactNative 의 AppState 라는 녀석을 제공해주고 있다.

AppState 의 listener 의 파라미터를 통해서 state 값을 받아온다. 이때 state 의 스팩은 아래와 같다.

- active - 앱이 포그라운드에서 실행중일때
- background - 앱이 백그라운드에서 실행중일때
- inactive [IOS] - 앱 비활성 기간 동안 발생
	- 포그라운드 <-> 백그라운드 전환 순간
	- 멀티태스킹 뷰 진입
	- 알림센터 열기
	- 전화 수신


```javascript
  const appStateRef = useRef(AppState.currentState);

  useEffect(() => {
    const subscription = AppState.addEventListener('change', nextAppState => {
      if (
        appStateRef.current.match(/inactive|background/) &&
        nextAppState === 'active'
      ) {
	    // 벡그라운드 -> 포그라운드
      } else if (
        appStateRef.current === 'active' &&
        nextAppState.match(/inactive|background/)
	  ) {
		// 포그라운드 -> 백그라운드
      }

      appStateRef.current = nextAppState;
    });

    return () => {
      subscription.remove();
    };
  }, []);

```
