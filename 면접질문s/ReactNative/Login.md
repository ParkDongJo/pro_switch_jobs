
Login 셋팅시 https://rnfirebase.io/auth/usage 를 참고하여 firebase authentication 을 활용하면 쉽게 구현할 수 있다.

하지만 이 문서도 설명이 엄청 친절하진 않다.

일단 나는 email, google, kakao 로그인을 붙이려고 한다.


# 기본 셋팅
```bash
# Install & setup the app module
yarn add @react-native-firebase/app

# Install the authentication module
yarn add @react-native-firebase/auth
```

위 두 패키지는 무조건 필요하다

그런데 이때 pod install 이후 pod install 을 하고 앱을 실행시키면

firebaseCore를 못찾는 다는 에러가 계속 발생한다.

이 문제는
https://stackoverflow.com/questions/59916348/fatal-error-module-firebase-core-not-found

여기서 말하는 것처럼

```
pod 'FirebaseCore', :modular_headers => true
```
를 Podfile 에 추가해주자 그런 후 pod install 을 해주자



# 구글 로그인

```
yarn add @react-native-google-signin/google-signin
```

구글 로그인은 몇가지 셋팅이 필요하다.
react-native-firebase 공싱 문서에서는

https://rnfirebase.io/auth/social-auth#google
위 링크에 설명이 되어 있지만,
역시나 친절하지는 않다.


그래서 아래 개인 설명 페이지와 함께 참고해서 보자.
이것도 시간내서 정리해놓자

https://velog.io/@ddowoo/react-native-google-login-%EA%B5%AC%ED%98%84-firebase-%EC%97%B0%EB%8F%99




# 카카오 로그인

RN 은 Kakao 는 아래의 라이브러리가 있다.


https://github.com/crossplatformkorea/react-native-kakao-login/blob/main/README.md


# 네이버 로그인

RN 은 Naver 는 아래의 라이브러리가 있다.
https://ssilook.tistory.com/entry/React-Native-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%84%A4%EC%9D%B4%ED%8B%B0%EB%B8%8C-%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EB%84%A4%EC%9D%B4%EB%B2%84-%EB%A1%9C%EA%B7%B8%EC%9D%B8%ED%95%98%EA%B8%B0Android-Naver-Login

https://github.com/crossplatformkorea/react-native-naver-login/blob/main/README.md



# 소셜 로그인 (Webview)

일단 소셜 로그인을  구현한다고 했을 때,

- 애플
- 구글
- 페이슨
- 카카오
- 네이버

여기서 대표적인 로그인을 구글, 카카오, 네이버 로 간추릴 수 있겠다.

RN에 3가지를 모두 대응하면 되겠지만, 내가 여러번 앱을 만든다고 했을 때 위와 같은 짓은 매우 번거롭고 귀찮다.

그러면 모듈화가 필요하다! 모듈화를 하는 전략도 2가지로 간추려볼 수 있다.

- RN 모듈화로 한다
- Web 모듈화로 한다

RN 모듈화로 하는 방법도 2가지를 생각해볼 수 있는데

직접 Native 까지 모두 구현
Native 단 까지 직접 구현을 해서 모듈화 하는 것이다. 이미 카카오, 네이버에서는 Native 에 대한 방법을 잘 설명해뒀기 때문에 RN 에서는 Native Module 로 덮어씌워주면 될것이다.

https://developers.naver.com/docs/login/ios/ios.md
https://developers.kakao.com/docs/latest/ko/kakaologin/ios
https://developers.google.com/identity/sign-in/ios/start-integrating?hl=ko

라이브러리에 라이브러리를 설치해서 조합
이 방법은 그냥 RN 라이브러리를 모아둔 또 하나의 라이브러리다. 잘 될지는 모르겠지만 잘된다 하더라도 코드 용량도 불필요해 질뿐더러 문제가 되었을 시 디버깅이 복잡해질 수 있다.
새 버전 대응도 수동적으로 될 수 있다.


그리고 Web 모듈화이다.
이 방법은 Webview 로 만들어보는 것이다.

하지만 이때 제약 사항이 있다. Google 의 정책상 웹뷰로 로그인 하는 것을 제한하고 있다.
https://bangbang-e.oopy.io/4b74dd76-1bf0-4799-ac85-4b7680b4c54f

이때,
Kakao 와 Naver 는 웹뷰로도 로그인 구현이 가능하다.

----
React 소셜 로그인 구현
https://velog.io/@runprogrmm/React-%EC%86%8C%EC%85%9C-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EA%B5%AC%ED%98%84-%EC%B9%B4%EC%B9%B4%EC%98%A4-%EA%B5%AC%EA%B8%80-%EB%84%A4%EC%9D%B4%EB%B2%84
-----
React Native Webview로 카카오톡 로그인 구현하기
https://orangebrother.dev/blog/use_kakao_login_react_native_webview



# 소셜 로그인 (NPM)

라이브러리로 소셜 로그인을 구현하는 방법도 있다. 이 방법으로 할까 말까를 매우 고민했었다.

이유는
- RN 버전이 바뀔때 마다 이슈가 생긴다. 국내 커뮤니티가 상대적으로 작다보니, 대응이 느리거나 매번 이슈를 직접 두들겨 맞아야 한다.
- Native 에 대한 공수를 줄이고 싶었다.

하지만, 우선 초기버전에서는 라이브러리를 사용하기로 했다.
지금 당장 webview 로 하기에는 호스팅 신청이나 CDN, Storage 구축 부터 해야 했기 때문이다. Firebase 와 Google Cloud를 전적으로 사용하겠다고 결정한 이상.. 해당 학습이 선행이 되어야 한다.

이 모든 것이 비용이기 때문에, 우선은 소셜 로그인으로 구현한다.

Google - https://rnfirebase.io/auth/social-auth#google
Kakao - https://github.com/crossplatformkorea/react-native-kakao-login?tab=readme-ov-file
- iOS
- AOS
Naver - https://github.com/crossplatformkorea/react-native-naver-login?tab=readme-ov-file
- iOS
- AOS

로 설치하면 된다. 나름 편한 부분은 각 공홈에서 추가로 더 설정을 하거나 구현을 해야할 부분을 어느정도 자동으로 셋팅이 되어 있다.

그래서, 리드미 파일에 명시된 순서대로 설정을 차근차근 해주면 된다.


이제  이 각각의 로그인에서 받아온 auth 정보를 다시 한번더 Firebase 쪽으로 연동시키는 로직을 추가 개발하여 소셜로그인에 대한 정보를 모두 Firebase Auth 에 통합시킨다.






# React Native에서 Firebase Cloud Funtions를 활용한 카카오 커스텀 토큰 로그인

Firebase는 여러 소셜 로그인을 지원합니다. 하지만 한국에서 가장 널리 쓰이는 카카오와 네이버는 지원하지 않기에 커스텀 토큰 로그인을 통해 Firebase 소셜 로그인을 하는 것이 목표입니다. 그 과정에서 Cloud Funtions을 사용해 커스텀 토큰을 발급하는 서버 로직을 구현합니다.

음.. 이건 직접 한번 해봐야겠다.

https://velog.io/@psb7391/RN-Firebase-Cloud-Funtions%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-%EC%B9%B4%EC%B9%B4%EC%98%A4-%EC%BB%A4%EC%8A%A4%ED%85%80-%ED%86%A0%ED%81%B0-%EB%A1%9C%EA%B7%B8%EC%9D%B8



# RN 오픈소스 만든다면

https://deku.posstree.com/ko/react-native/make-opensource-library/



# RN firebase/functions 뻑킹 이슈

일단 이유를 알기 힘들었지만, ios 에서 @react-native-firebase 19 버전에서 functions 모듈도 함께 설치해서 사용하고자 했을 시

- 의존성이 있길래 podfile 에 의존성 헤더 설정을 해주니
- xcode 에서 앱 빌드 자체가 되지 않았다.

각 상황의 조치는 아래와 같다

- 첫번째 상황
에러 내용
``` The Swift pod `FirebaseFunctions` depends upon `FirebaseCoreExtension`, `FirebaseAppCheckInterop`, `FirebaseAuthInterop`, `FirebaseMessagingInterop`, and `GTMSessionFetcher`, which do not define modules. To opt into those targets generating module maps (which is necessary to import them from Swift when building as static libraries), you may set `use_modular_headers!` globally in your Podfile, or specify `:modular_headers => true` for particular dependencies. ```

해준 조치
```
# Podfile

target 'YourTargetName' do
  pod 'FirebaseFunctions', :modular_headers => true
  # Specify modular headers for other dependencies as needed
  pod 'FirebaseCoreExtension', :modular_headers => true
  pod 'FirebaseAppCheckInterop', :modular_headers => true
  pod 'FirebaseAuthInterop', :modular_headers => true
  pod 'FirebaseMessagingInterop', :modular_headers => true
  pod 'GTMSessionFetcher', :modular_headers => true
end

```


-  두번째 상황
에러 내용
``` Build service could not create build operation: unknown error while handling message: MsgHandlingError(message: "unable to initiate PIF transfer session (operation in progress?)") ```

특정 지을 수 있는 에러가 아니였고, functions 모듈을 제거하면 에러가 나지 않음!! 추측하건데 firebase 의 의존성에 의해, 관련 모듈들을 모두 설치를 해줘야 하는 것 같음!


### 그래서 나의 결정은
굳이 app 에서 functions 의 충돌을 해결해가며, 사용할 필요가 없다고 판단하였음

물론 사용하면 아래와 같은 장점들은 있을것임
- 인증을 알아서 해준다.
- 사용이 편리하다

하지만 충돌이 있고, 추후에 또 어떤 이슈가 있을지 예상하기 쉽지 않음 그래서 직접 http 호출로 function 을 사용하기로 함


# firebase/functions 함수 로컬 빌드

... 코드를 변경해줘도 계속 에러가 나길래.. 이리저리 수정하고 디버깅하다가...
문득 build 명령어가 생각남..
하.. 결국 서빙되는 코드는 빌드가 된 결과물이였음.. dev 라서 바로 로딩 될줄 알았지...

- npm build
- firebase serve --only functions --port=4002

해주자