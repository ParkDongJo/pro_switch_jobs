
- workflows
	- on
	- jobs
		- steps
			- run
			- uses
				- with


# RN APP BUILD순서

- Github 으로 코드 Checkout
	- actions/checkout
- Node 환경 설치
	-  actions/setup-node@master
- [Android] JDK 설치
	- actions/setup-java@v1
- Ruby 설치
	- ruby/setup-ruby@v1
- npm install
- [iOS] Pods 설치
	- cd ios && pod install
- Fastlane 설치
	- bundle install
	- bundle update fastlane
- Fastlane 실행
	- bundle exec fastlane {명령어}




https://gist.github.com/victorbruce/c6c3190bc9d607d5876a2d88f8b31ca7

https://medium.com/@malikchohra/ci-cd-pipeline-for-react-native-apps-use-fastlane-and-github-actions-dcf101edc423

https://deku.posstree.com/en/react-native/github-actions-fastlane/


https://f-lab.kr/insight/android-app-ci-cd-with-github-actions