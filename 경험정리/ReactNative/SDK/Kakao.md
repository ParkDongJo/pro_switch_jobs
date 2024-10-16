카카오 SDK 를 사용하는 법은 은근 까다롭다. 이에 대해 몇번 설정을 한 경험과 직접 Native Modules 를 일부 제작한 경험이 있는데 이에 대한 경험을 정리해놓자!







## 이슈 기록 - 카카오 SDK 가 난독화된 Production 환경에서만 안되는 이슈

ReactNative 에서 react-native-kakao-share-link 를 활용해서 kakao 메시지 공유 기능을 구현하고 있다.

react-native-kakao-share-link를 쓰는게 맞냐 안맞냐는 여기서는 논외다. 내가 오기도 전에 이미 사용하고 있었고, 나 또한
- 직접 만들던가
- 두부랩에서 제공하는 kakao sdk 라이브러리를 사용하던가

둘 중 하나가 낫다고 본다.

내가 겪은 이슈는 아래와 같은 현상이 발생했다.


##### 현상
카카오톡 초대링크를 보내기 위해서, 메시지 공유가 Production 앱에서 실행이 안된다는 이슈가 있었다.
실제로 확인을 해보니,

- iOS 정상 동작

Android 에서
- 로컬에서 빌드한 앱 -> 정상동작
- Stage Debug 환경 앱 -> 정상동작
- Stage Release 환경 앱 -> 정상동작
- 난독화 하지 않은 Production 환경 앱 -> 정상동작
- apk 로 난독화 한 Production 환경 앱 -> 정상동작
- abb 로 난독화 한 Production 환경 앱 -> `동작하지 않음!!`


##### 해결 방향
처음에는 카카오 데브옵스에 안드로이드는 릴리즈 해시키를 등록하도록 되어 있다. 릴리즈 해시키는 로컬에서 아래와 같은 방법으로 생성이 가능하다.


```
keytool -exportcert -alias androidreleasekey -keystore ~/../app.keystore | openssl sha1 -binary | openssl base64
```


하지만 이미 릴리즈 해시키는 내가 개발 시 잘 셋팅을 해뒀었다.
다음 의심을 하는 지점은 난독화였다. 묘~~ 하게도 Production 안드로이드 환경에서만 실행이 안되었고 실제로 난독화를 하지 않은 aab 파일로 테스트 해봤을 시 정상 동작하는 걸 확인이 됐다.

aab 파일을 apks 파일화 하여, android 디바이스로 파일 전송을 하면 된다 (나같은 경우 슬렉으로)
좀더 구체적인 방법은 아래와 같다

먼저 bundletool-all-1.17.1.jar 를 설치하고, 아래 명령어로 로컬에서 실행 시킨다.

```
java -jar "bundletool-all-1.17.1.jar" build-apks --bundle=app-production-release.aab --output=app.apks --ks="${현 위치에서 keystore 경로}" --ks-pass=pass:${키스토어 비번} --ks-key-alias={명칭} --key-pass=pass:${키 비번} --mode=universal
```
[https://wonyoung2.tistory.com/entry/AAB-%ED%8C%8C%EC%9D%BC-APK-%ED%8C%8C%EC%9D%BC%EB%A1%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0](https://wonyoung2.tistory.com/entry/AAB-%ED%8C%8C%EC%9D%BC-APK-%ED%8C%8C%EC%9D%BC%EB%A1%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0)
[https://m.blog.naver.com/teri_kim/222681983074](https://m.blog.naver.com/teri_kim/222681983074)


좀 더 디테일하게 테스트를 해본 결과,

- apk 로 난독화한 결과는 정상 실행이 되고
- aab 로 난독화한 결과는 정상 실행되지 않았다.

이 결과를 통해 처음에는 aab 로 난독화를 했을 시, 이에 대한 대응이 잘 안된거라고 추측하고 난독화 솔루션 업체에 문의를 했었는데

동료분의 도움으로 아래와 같은 원인이 있다는걸 알게 됐다.

aab 로 난독화 할때, 특히! 구글 앱서명을 함께 하는데, 앱 서명 대한 해시 키값을 앱 자체에서 추출하지 않고,  
구글 플레이 콘솔에 있는 키값을 해시하여 사용해야한다.



-----------
카카오 디벨로퍼 문서 (https://developers.kakao.com/docs/latest/ko/android/getting-started#before-you-begin-add-key-hash-using-console)
#### Google Play Console 앱 서명으로 릴리즈 키 해시 확인[](https://developers.kakao.com/docs/latest/ko/android/getting-started#before-you-begin-add-key-hash-using-console)

[Google Play 앱 서명(App Signing)](https://developer.android.com/studio/publish/app-signing#app-signing-google-play)을 사용한다면 직접 릴리즈 키 해시를 생성하지 않고, Google Play Console에서 얻은 SHA-1 인증서 지문(SHA-1 certificate fingerprint)을 Base64로 인코딩하여 사용해야 합니다. 자세한 내용은 [앱 서명 사용하기](https://support.google.com/googleplay/android-developer/answer/9842756?hl=ko#)를 참고합니다.

[Google Play Console] > [설정] > [앱 무결성] 메뉴의 [앱 서명키 인증서] 항목에서 [SHA-1 인증서 지문] 값을 복사합니다.  
만약 Google Play Console에 접근 권한이 없고 인증서 파일(deployment_cert.der)만 가지고 있다면 터미널에 다음과 같이 입력합니다.

```bash
keytool -printcert -file ./deployment_cert.der 
```

SHA-1 인증서 지문 값을 터미널에 다음과 같이 입력합니다.

```bash
echo "${PRINTCERT}" | xxd -r -p | openssl base64 
```

-----------


이렇게 구글 플레이 콘솔에서 제공하는 인증서 지문을 통해 새로 추출한 키를 카카오 디벨로퍼에 추가 등록해줘야 한다.
아래 링크에 다른 분이 이미 안드로이드에서 경험을 정리한 글이 있다,

https://dd-developer.tistory.com/81