
Push Notification 은 예전에 안드로이드 공부하면서, FCM 을 다뤄본적이 있는데, RN을 하면서 iOS까지 다루다 보니 둘의 차이점을 비교해가면서 공부해보는 것도 꽤 재미있는 것 같다.

Push 의 원리는 아래와 같다.

![[push_notification.png]]

여기서 Push Token 발행 및 Push 전송을 맡는 

iOS 는 APNs
AOS 는 FCM

이 각각 있다.

Push 를 전송하기 위한 전제 조건은

- APNs 와 FCM 에 각각 계정이 등록되어 있어야 한다.
- App 서버가 있어야 한다
- App 에서는 App 서버로 push token 을 등록해줘야 한다.


여기서 APNs 와 FCM 의 역할은

- Push Device Token 을 발행해준다.
- App 서버로 부터 받은 Push Message 내용을 해당 Device 로 전달해준다.

즉 Device Token 을 통해 해당 Device 를 추적하고 Message 를 최종 전송하는게 APNs 와 FCM의 역할이다. 

> 이건 내 추측이지만,
 아마로 Mobile 벡그라운드에서 Socket 으로 계속 연결 되어 있을 것 같다. 그게 아니면 Background 에서 지속적으로 FCM 이나 APNs 에 찔러 보던가



## Braze 를 위한 필요한 셋팅
참고 자료 : https://ios-development.tistory.com/652

AppDelegate.mm

```c#
#import <React/RCTLinkingManager.h> // For deep link
#import <React/RCTBundleURLProvider.h>
#import "AppDelegate.h"
#import "BrazeReactUtils.h"
#import "BrazeReactBridge.h"

#import <BrazeKit/BrazeKit-Swift.h>

  
@implementation AppDelegate

  

static Braze *_braze = nil;
  

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
	self.moduleName = @"my_app";
	self.initialProps = @{};

	[FIRApp configure];

	// Setup Braze
	NSString *apiKey = [[NSBundle mainBundle] objectForInfoDictionaryKey:@"BRAZE_API_KEY"];
	NSString *endpoint = [[NSBundle mainBundle] objectForInfoDictionaryKey:@"BRAZE_ENDPOINT"];

	BRZConfiguration *configuration = [[BRZConfiguration alloc] initWithApiKey:apiKey endpoint:endpoint];
	configuration.triggerMinimumTimeInterval = 1;
	configuration.logger.level = BRZLoggerLevelInfo;
	
	Braze *braze = [BrazeReactBridge initBraze:configuration];
	AppDelegate.braze = braze;

	[self registerForPushNotifications];

	[[BrazeReactUtils sharedInstance] populateInitialUrlFromLaunchOptions:launchOptions];

	[super application:application didFinishLaunchingWithOptions:launchOptions];
	return YES;
}

  
// APNs에서 push를 받기위해 등록 요청
- (void)registerForPushNotifications {

	UNUserNotificationCenter *center = UNUserNotificationCenter.currentNotificationCenter;
	[center setNotificationCategories:BRZNotifications.categories];
	center.delegate = self;

	[UIApplication.sharedApplication registerForRemoteNotifications];

	// Authorization is requested later in the JavaScript layer via `Braze.requestPushPermission`.
}

  
// push 알림을 받았을때 Braze에 전달하여 push 분석과 로깅하는데 사용
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {

	BOOL processedByBraze = AppDelegate.braze != nil && 
	[AppDelegate.braze.notifications handleBackgroundNotificationWithUserInfo:userInfo fetchCompletionHandler:completionHandler];

	if (processedByBraze) {
		return;
	}

	completionHandler(UIBackgroundFetchResultNoData);
}


// push를 탭한 경우 처리
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response
withCompletionHandler:(void (^)(void))completionHandler {

	[[BrazeReactUtils sharedInstance] populateInitialUrlForCategories:response.notification.request.content.userInfo];

	// 사용자가 push에 관해 상호작용한 후 알림 응답을 braze로 전달
	BOOL processedByBraze = AppDelegate.braze != nil && 
[AppDelegate.braze.notifications handleUserNotificationWithResponse:response
withCompletionHandler:completionHandler];

	if (processedByBraze) {
		return;
	}
	completionHandler();
}


// foreground에 있는 동안의 push 표출 옵션
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
willPresentNotification:(UNNotification *)notification
withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler {

	if (@available(iOS 14.0, *)) {
		completionHandler(UNNotificationPresentationOptionList | UNNotificationPresentationOptionBanner);
	} else {
		completionHandler(UNNotificationPresentationOptionAlert);
	}
}


// APNs에 해당 디바이스가 등록이 완료된 경우, Braze에 토큰 등록
- (void)application:(UIApplication *)application
didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {

	[AppDelegate.braze.notifications registerDeviceToken:deviceToken];
}

  
// Deep Linking
- (BOOL)application:(UIApplication *)application
openURL:(NSURL *)url
options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
{
	NSLog(@"Calling RCTLinkingManager with url %@", url);
	return [RCTLinkingManager application:application openURL:url options:options];
}


+ (Braze *)braze {
	return _braze;
}

  
+ (void)setBraze:(Braze *)braze {
	_braze = braze;
}


@end
```

AppDelegate.h
```c#
#import <RCTAppDelegate.h>
#import <React/RCTBridge.h>
#import <UIKit/UIKit.h>
#import <UserNotifications/UserNotifications.h>
  
@interface AppDelegate : RCTAppDelegate <UNUserNotificationCenterDelegate>

@end
```


my_app.plist
```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
	<dict>
		<key>BRAZE_API_KEY</key>
		<string>dfsdsfs-ssss-dddd-xxxx-cccccccc</string>
		<key>BRAZE_ENDPOINT</key>
		<string>xxxxx.braze.com</string>
	</dict>
</plist>
```


Xcode
Target Project > Signing & Capabilities > 

- Background Modes
- Push Notifications

![[Pasted image 20240524093019.png]]





Braze
https://github.com/braze-inc/braze-react-native-sdk/blob/master/BrazeProject/ios/BrazeProject/AppDelegate.mm

https://dchkang83.tistory.com/150

https://velog.io/@slife518/react-native-%EC%97%90%EC%84%9C-%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95%ED%8C%8C%EC%9D%BC-%EB%B6%84%EB%A6%AC%ED%95%98%EA%B8%B0


Android

안드로이드는 

https://maejing.tistory.com/52


https://medium.com/%EB%B0%95%EC%83%81%EA%B6%8C%EC%9D%98-%EC%82%BD%EC%A7%88%EB%B8%94%EB%A1%9C%EA%B7%B8/%EC%95%8C%EB%A6%BC-%EA%B6%8C%ED%95%9C-%EC%9A%94%EC%B2%AD%EC%97%90-%EA%B4%80%ED%95%9C-%EB%AA%A8%EB%93%A0%EA%B2%83-feat-android-13-5f20d17b5d09


배포 기준 따로 설정 
https://mrgamza.tistory.com/614