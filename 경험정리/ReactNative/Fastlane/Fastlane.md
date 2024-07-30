
# 환경 설정
- brew install fastlane 실행 -> 설치
- {project}/ios/
	- fastlane init 실행 -> 구축
- /fastlane 에 생성된 파일
	- .env.default
	- Appfile
		- fastlane 에서 사용될 client id, apple id, project id 등등을 설정
	- Fastfile
		- 자동화 task 정의
	- Pluginfile
		- 사용할 plugin 설치
			- fastlane-plugin-firebase_app_distribution
			- fastlane-plugin-increment_version_code


# 파이프 라인 순서

- lane 실행
- 앱 버전/빌드 번호 변경
- 앱 버전 태그 PUSH -> 이벤트 실행
- GIT 코드 체크아웃
- SSH key, ruby, fastlane 등 설치
- keychain 초기화
- match 실행 -> 인증서 및 프로파일 불러오기
- 빌드
- 앱스토어 업로드
- 배포 결과 슬랙 노티 / 에러 결과 보고



# 환경별 분리

- react-native-config (vs. react-native) 설치
- .env 작성
- /build.gradle
	- minSdkVersion 설정
	- targetSdkVersion 설정
- app/build.gradle 
	- react-native cofig path 설정
	- android > defaultConfig
		- applicationId
		- minSdkVersion
		- targetSdkVersion 등등 설정
	- android > buildTypes 작성
		- debug / release 로 분리
		- signingConfig
		- minifyEnabled 등등 작성
	- android > productFlavors 작성
		- dev / stage / production 으로 분리
		- 환경별 - applicationId
		- 환경별 - minSdkVersion
		- 환경별 - targetSdkVersion 
	- android > signingConfigs 작성
		- storeFile
		- storePassword
		- keyAlias
		- keyPassword
