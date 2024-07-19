
# 나왔던 이슈들
- iOS 에서 1번 푸시 이후 Bad Token 발생
- iOS 에서 Debug 모드 시 푸시 발송 되나, Release 실빌드에서 푸시 발송 안되는 이슈
- Android 푸시 알람 오지 않는 이슈
- Android 에서 푸시 알림 클릭 후 딥링크가 안되는 이슈


# iOS 이슈 해결 방법
## iOS 에서 1번 푸시 이후 Bad Token 발생 이유

iOS 에서 아래 설정을 제대로 안해준 상황
- enttitlements 파일에 aps-environment 설정
	- release -> production
	- debug -> development 로 설정되도록 해야합
- Signing & Capabillities 에
	- Background Mode
		- Remote notifications 를 환경별로 설정
	- Push Notification
		- 환경별로 설정

직접 NSLog 를 찍어가며 확인하기
- Push Notification 를 APNs 에 등록해주고 있는가?
	- [UIApplication.sharedApplication registerForRemoteNotifications]
- Device Token 발급 받았는가
	- didRegisterForRemoteNotificationsWithDeviceToken() 에서 찍어보기
- Push Notification 등록에 실패했다면, 에러메시지는?
	- didFailToRegisterForRemoteNotificationsWithError() 에서 찍어보기

해당 상황에서는 아래와 같은 에러가 발생했고, 에러를 바탕으로 문제해결을 할 수 있었음
```
Error Domain=NSCocoaErrorDomain Code=3000 "응용 프로그램을 위한 유효한 ‘aps-environment’ 인타이틀먼트 문자열을 찾을 수 없습니다." UserInfo={NSLocalizedDescription=응용 프로그램을 위한 유효한 ‘aps-environment’ 인타이틀먼트 문자열을 찾을 수 없습니다.}
```



## iOS 에서 Debug 모드 시 푸시 되나, Release 실빌드에서 푸시 발송 안되는 이슈

Apple 은 Push Notification 을 발송하는 시스템이 아래와 같이 되어 있다.

Push Token 등록 시
- App Push Server <- Device -> APNs

Push 전송 시
- App Push Server -> APNs -> Device

이 때, 어떻게 보면 App Push Server 가  APNs 에 푸시 발송을 trigger 하기 위해서는, APNs 가 이를 허용하는(위임) 증명이 필요한데, 이를 Push Notification 을 허용하는 key 파일 .p8 을 App Push Server 에 심도록 되어있다.

여기서 App Push Server 역할을 Braze 가 대신해주고 있다.

이때 Braze 에서 알림을 전송하면 APNs 에 trigger 되고 최종적으로 APNs 에서 내려다 주는 payload 라는 푸시 알림 데이터가 앱으로 전송되는데, 이대 형태는 아래와 같다

```
{ 
	ab = { 
			c = 		"NjY4N2NhNDQzODE0YjUwMDU5ODVlMzkzXyRfZGk9NjY4N2QyZTYwZTAzOTUwMjkyOTM5M2E0MTZlMDMxMzYmbXY9NjY4N2NhYjE1Y2RhZTIwMDU5NmIyZmY1JnBpPWNtcA=="; 
		}; 
		aps = { 
			alert = { 
				body = 42dot; 
				title = 42dot; 
			}; 
			"interruption-level" = active; 
		}; 
	}
}
```

여기서

alert 속성은 OS 단에서 알림메시지를 띄우게끔 하는 속성이다.
https://developer.apple.com/documentation/usernotifications/generating-a-remote-notification


그리고
APP은 forground 와 background 를 고려해야하는데, iOS 에서는 아래 listner 함수들이 있다.

forground 일 때
```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center  
       willPresentNotification:(UNNotification *)notification  
         withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler {  
  if (@available(iOS 14.0, *)) {  
    completionHandler(UNNotificationPresentationOptionList | UNNotificationPresentationOptionBanner);  
  } else {  
    completionHandler(UNNotificationPresentationOptionAlert);  
  }  
}
```


background 일 때
```
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {  
  BOOL processedByBraze = AppDelegate.braze != nil && [AppDelegate.braze.notifications handleBackgroundNotificationWithUserInfo:userInfo  
                                                                                                         fetchCompletionHandler:completionHandler];  
  if (processedByBraze) {  
    return;  
  }  
  
  completionHandler(UIBackgroundFetchResultNoData);  
}
```

여기까지 정상적인 payload 가 찍히는 것을 확인했다면, APP 까지는 정상적인 푸시 메시지가 전송되고 있는 것으로 보면 된다.

이때 Braze 내부적으로는 다시 Braze 서버로 푸시가 발송되었음을 데이터를 보내준다고 한다.

결록적으로는 APNs 에 대해 Debug 환경으로 보낼것이냐, Release 환경으로 보낼 것이냐에 대한 대시보드 설정이 있었다.

모든 App Push Server 에서 이런 설정이 있을지는 모르겠지만, 푸시를 발송하는 서버쪽도 Debug 에 대한 전송인지 Release 대한 전송 인지를 확인할 필요가 있다.
![[Pasted image 20240708220754.png]]





# Android 이슈 해결 방법
## Android 에서 푸시 알림이 오지 않는 이슈

iOS에서는 APNs 가 있다면, Android 에서는 FCM 이 있다. 여기서도 Braze 가 App Push Server 를 자처한다. 이때도 마찬가지로 위임에 대한 증명서가 필요하다.

FCM 같은 경우는 Google 에서 제공하는 서비스 이다. 이때 해당 앱이 연동이 되어야하는 클라우드 서비스가 있다.

- Firebase
- Google Cloud

일단 여기서는 Firebase 설정은 되어 있다고 보고, Google Cloud 만 살펴보자

- Service Accounts 에 Account 를 생성해야한다
- Firebase Cloud Messaging 에 대한 설정을 하고 key 를 발급 받는다.
- 이 key 파일에 대한 JSON 형태의 파일을 받을 수 있다.

이걸 Braze 대시보드에 올려줘야한다.

그런데, 이때 이 key 파일이 잘못 올려져 있었던 것 같다. 다시 하나씩 차근차근 실행하고 업로드 해서 이 문제는 해결 할 수 있었다.

https://www.braze.com/docs/developer_guide/platform_integration_guides/android/push_notifications/android/integration/standard_integration/#rate-limit

해당 문서에 잘 설명이 되어 있다.


## Android 알림 클릭 시 딥링크
딥링크에 대한 구축 및 테스트는 react-navigation 을 통해 구축을 했고,

iOS 는 
```
xcrun simctl openurl booted 'hyundaiselection://'
```

Android 는
```
adb shell am start -W -a android.intent.action.VIEW -d hyundaiselection://
```

같이 테스트를 한 상황이였다.

하지만 왜인지는 모르겠지만, Push 알림 클릭을 통해 들어가는 딥링크는 또 다른 걸로 보인다.

이에 대해서 Braze 문서에는 옵셔널로 되어 있지만,
이는 필수사항으로 해야 맞을 것 같다!!

![[Pasted image 20240708222622.png]]


어째든 저 링크를 타고 갔을 시 2개의 적용사항이 나온다

- braze.xml 에 적용하는 것
```xml
<bool name="com_braze_handle_push_deep_links_automatically">true</bool>
```
- custom 하기 위해 MainActivity 에 적용하는 것
```kotlin
val brazeConfig = BrazeConfig.Builder()
        .setHandlePushDeepLinksAutomatically(true)
        .build()
Braze.configure(this, brazeConfig)
```


나는 일단 braze.xml 만 적용했다. (그렇게 해도 정상 동작 한다)

일단 이유는 이미 deeplink 에대한 custom 설정은 react-navigation 에 해뒀기 때문이다. 아무래도 Braze 의 문서가 Native 코드를 위한 문서 설명과 뒤죽박죽 섞여 있다보니, 그런것 같다.