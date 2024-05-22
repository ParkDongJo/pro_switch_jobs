# Firebase

- Firebase console 에 프로젝트 생성
- sdk Info 다운로드
	- iOS -> bundle ID
	- AOS -> app ID
- 패키지 설치
- Info 파일 셋팅
- 각 설정
	- iOS
	- AOS


https://rnfirebase.io/



# Fastlane

https://docs.fastlane.tools/getting-started/ios/setup/
https://green1229.tistory.com/350




# iOS

fastlane 을 빌드를 성공시키는데 꽤 애를 먹었다.

일단 하나 알게된건 fastlane 을 진행하면서 ios/ 루트에 Gemfile 이 따로 있어야 한다 여기에는 아래와 같은 설정이 있어야 한다.


```
source "https://rubygems.org"

  

gem 'dotenv'

gem 'fastlane'

plugins_path = File.join(File.dirname(__FILE__), 'fastlane', 'Pluginfile')

eval_gemfile(plugins_path) if File.exist?(plugins_path)
```


상세한 설명은 추후에

https://arc.net/l/quote/onbromvz




## iOS Release 와 Debug





## build 폴더 clean

```
scripts: { 
  "clean:android": "cd android && ./gradlew clean && cd ../",
  "clean:ios": "cd ios && xcodebuild clean && cd ../", 
}
```



```
<?xml version="1.0" encoding="utf-8"?>

<resources>

<string name="com_braze_api_key">cb7ce316-537d-4e63-9076-aa9b82ee51f5</string>

<string translatable="false" name="com_braze_custom_endpoint">sdk.iad-06.braze.com</string>

  

<!-- Firebase Automatic Token Registration -->

<bool translatable="false" name="com_braze_firebase_cloud_messaging_registration_enabled">true</bool>

<string translatable="false" name="com_braze_firebase_cloud_messaging_sender_id">25976777444</string>

  

<integer name="com_braze_trigger_action_minimum_time_interval_seconds">1</integer>

</resources>

```