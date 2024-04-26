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