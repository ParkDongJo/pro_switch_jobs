

# Azure pipelines

https://learn.microsoft.com/ko-kr/appcenter/build/export-to-azure-pipelines?source=recommendations


https://velog.io/@2ast/React-Native-firebase-app-distribution%EC%9C%BC%EB%A1%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%95%B1-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0




# iOS 테스트 앱 배포 센팅

RN 에서 기존 App Center 가 곧 지원이 중단되기 때문에, 더이상 App Center 를 통해서 배포가 불가능해짐
대안으로 Firebase 의 App distribute 를 통해 배포를 할 수 있다.

이를 fastlane 으로 설정을 할 수 있을텐데,

개발 환경은
-> Firebase App Distribute

프로덕션 환경은
-> App Store Testflight

로 배포 되게끔 스크립트를 작성한다.

일단 최대한 공식 홈페이지를 통해서 방법을 숙지하고자 했고, 그렇다고 해서 공식홈페이지가 친절하지는 않다.

공식 홈페이지는


[fastlane을 사용하여 테스터에 iOS 앱 배포](https://firebase.google.com/docs/app-distribution/ios/distribute-fastlane?hl=ko)
[ReactNative Distributing beta builds](https://thecodingmachine.github.io/react-native-boilerplate/docs/BetaBuild/#stuck-at-bundle-install-or-bundle-update-running-fastlane-init)


그 외 도움이 되었던 개인 홈페이지는

[React Native Fastlane으로 앱스토어 자동 배포하기](https://velog.io/@kimjisub/React-Native-Fastlane-Beta-Release)
[# React Native) fastlane으로 firebase app distribution 자동화하기](https://velog.io/@2ast/React-Native-fastlane%EC%9C%BC%EB%A1%9C-firebase-app-distribution-%EC%9E%90%EB%8F%99%ED%99%94%ED%95%98%EA%B8%B0)

그 외

https://green1229.tistory.com/438




# iOS 빌드 전 기본 셋팅

iOS 에서 기본적으로 셋팅해야할 개념들이 있다. 애플 개발자 어드민 페이지에

https://developer.apple.com/account/resources/devices/list

- 기기
- 인증서
- 프로파일링

을 등록해야한다.

Xcode tool 을 통해서 아주 손쉽게 할 수 있는데, 고건!! 추후에 또 정리해보자