
https://github.com/thebergamo/react-native-fbsdk-next



문서에 부족한 내용

iOS

AppDelegate.mm 에 FBSDKCoreKit-swift 를 import 할 시 2가지가 전제가 되어야한다.
```
#import <FBSDKCoreKit/FBSDKCoreKit-swift.h>
```


조건 1> 아래 2개의 h 파일들도 import가 명시되어야 한다. (공식 문서에도 기재되어 있음)
```
#import <AuthenticationServices/AuthenticationServices.h>
#import <SafariServices/SafariServices.h>
#import <FBSDKCoreKit/FBSDKCoreKit-swift.h>
```
[공식문서](https://github.com/thebergamo/react-native-fbsdk-next?tab=readme-ov-file#32-ios)


조건 2> FBSDKCoreKit-swift 헤더 파일은 위 2개의 헤더파일보다 무조건 아래에 명시되어야 한다.
```
#import <AuthenticationServices/AuthenticationServices.h>
#import <SafariServices/SafariServices.h>
#import <FBSDKCoreKit/FBSDKCoreKit-swift.h>
```
[깃헙이슈](https://github.com/facebook/facebook-ios-sdk/issues/2183#issuecomment-1693569643)



Android

Manifest.xml 에서 아래 광고 ID권한을 설정하는 아래 코드를 추가 시, xmlns:tools 네임스페이스 설정이 필요하다
```
<uses-permission android:name="com.google.android.gms.permission.AD_ID" tools:node="remove"/>
```


```
<manifest xmlns:android="http://schemas.android.com/apk/res/android" xmlns:tools="http://schemas.android.com/tools">

</manifest>
```


문서에는 Android Studio 설정 부터 추가하라고 되어 있지만, 공식문서에는 build.gradle 변경은 npm 링크 단계에서 처리된다고 되어 있습니다. [공식문서_관련내용](https://github.com/thebergamo/react-native-fbsdk-next?tab=readme-ov-file#31-android)

그리고 
Link 작업은 0.60+ 이상으로는 autolink 될 것이기 때문에, 생략해도 되는 작업입니다.
0.59 이하라고 하더라도, 'Android Studio 설정 > 프로젝트 만들기' 의 gradle 작업은 Android Native 를 위한 설정이고, React Native와는 관련 없는 내용으로 보입니다.

결론적으로 'Android Studio 설정' 부터 하나도 빠짐없이 진행하는 것이 아니라. 
0.60+ 이상은
- Android Studio 설정 > 매니페스트 업데이트 부터 시작
0.59 이하 버전은
- Android Studio 설정 > 매니페스트 업데이트 부터 시작


