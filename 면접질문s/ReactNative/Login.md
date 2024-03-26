
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



# 소셜 로그인

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



# React Native에서 Firebase Cloud Funtions를 활용한 카카오 커스텀 토큰 로그인

Firebase는 여러 소셜 로그인을 지원합니다. 하지만 한국에서 가장 널리 쓰이는 카카오와 네이버는 지원하지 않기에 커스텀 토큰 로그인을 통해 Firebase 소셜 로그인을 하는 것이 목표입니다. 그 과정에서 Cloud Funtions을 사용해 커스텀 토큰을 발급하는 서버 로직을 구현합니다.

음.. 이건 직접 한번 해봐야겠다.

https://velog.io/@psb7391/RN-Firebase-Cloud-Funtions%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-%EC%B9%B4%EC%B9%B4%EC%98%A4-%EC%BB%A4%EC%8A%A4%ED%85%80-%ED%86%A0%ED%81%B0-%EB%A1%9C%EA%B7%B8%EC%9D%B8



# RN 오픈소스 만든다면

https://deku.posstree.com/ko/react-native/make-opensource-library/
