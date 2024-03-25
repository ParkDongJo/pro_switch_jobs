
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