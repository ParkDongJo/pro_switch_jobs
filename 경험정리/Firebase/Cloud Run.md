
앱개발을 해야하는데, 서버단 작업을 빠르게 구축하는 방법은 단연 Firebase 가 먼저 떠올랐다. 물론 Supabase 도 있지만 실제 서비스 런칭까지 해볼 참이라 고려대상에서 제외했다.

AWS 를 활용하는 방법도 있지만, 서버 지식 이정도 가지고는 속도를 내기에는 부족하다.

하지만 Firebase 가지고는 부족한 부분이 있다. Firebase 는 실시간 데이터베이스이고 이와 관련된 코드가 클라이언트 단에 들어가는 건 피하고 싶다.

2022년 조은님이 인프콘에서 발표해주신  영상을 보고 GCP 로 API를 구축해보자 싶었다.
https://www.youtube.com/watch?v=HZFx0Y5bnlg

Firebase 연동도 쉬울 것 같고, 하나의 콘솔 에서 모두 관리하는 것이 좋을 것 같았다.

그래서 따라 시작한 것이

# Cloud Run & Cloud Build

사실 조은님의 영상만으로는 몇몇 빠져있는 과정들이 있다. 직접 자료나 에러를 확인해가면서 구축해야 했었다.

## Firebase 셋팅
Firebase 는 우선 내가 만들 프로젝트만 만들어둔다,


## Code 작성
gts + typescript
- Google TypeScript Style Guide라는 도구를 사용하여 TS 개발 환경 + ESLint를 한번에 구축

tsConfig 수정
- TypeScript 에서 ES Module을 사용하기 위한 옵션을 추가
	- esModuleInterop
	- resolveJsonModule

Express + nodemon + ts-node
- express, @types/express 설치
- nodemon, ts-node 설치
- package.json 에 실행 또는 빌드 스크립트 작성

Docker
- Dockerfile 작성
- Docker Image 생성 후 배포
- Docker Desktop 설치


## Cloud Run
우선 Google Cloud Console 로 접속한다. 

이때 Cloud Run 에서는 소스 저장소를 지속적으로 모니터링 하여, 소스코드가 업데이트 될 때마다 배포를 진행할 수 있는데, 이를 선택하고 CLOUD BUILD 로 설정을 해줘야 한다.

이때 CLOUD BUILD 로 설정 되어야 할 것들이 몇가지 있는데
- Github (다른 저장소들도 사용 가능) 에 Google Cloud Build 어플리케이션이 설치가 되어 있어야 한다.
- 그리고 Cloud Build 에서 저장소를 연결해야한다.

연결이 다 되면 Cloud Run 에서 설정해야할 것들을 설정한다.
- 이름 작성
- 리전 선택
- 인증 여부
- CPU
- 인스턴스 최소 개수 -> 1


## Cloud Build
저장소 연결
- 저장소 > 저장소연결
	- 소스코드 관리 제공업체 선택 -> Github
	- 인증
	- 저장소 선택
		- 계정선택
		- 저장소 선택
	- 트리거 만들기 (선택사항)

이 설정을 완료하면 Cloud Run 에서 연결된 저장소의 레파지토리를 모니터링 할 수 있게된다.



## 로드
이제 깃헙 소스를 가져와서 자동 빌드를 하게 된다. 이때 빌드가 한번에 되면 좋겠지만 그렇지 않다. 분명 로컬에서 했을 때는 Docker Load도 잘되고, Sever 구동도 잘 됐지만..

이때는 Cloud Run 에서 찍히는 로그를 보면서, 원인을 찾아가야한다.


조은님 영상에서도 틀린 부분이 있는데, package.json 에서 start 의 스크립트를

yarn clean && yarn compile && node build/src/index.js

이렇게 작성하였다. 물론 이 스크립트는 로컬에서는 잘 돌아가는 명령이다. 하지만 DockerFile에는

```

WORKDIR /usr/src/app  
COPY package.json ./  
  
RUN npm install  
  
COPY . ./  
  
RUN npm run compile  
RUN rm -rf ./src  
  
CMD ["npm", "start"]

```

이와 같이 되어 있다.  DockerFile 만 봐서는 문제가 없지만,
compile 을 한 후 rm -rf ./src 를 통해서 코드 자체를 삭제하고 있다. 하지만 start 명령에는 yarn compile 이 포함되어 있다.

기존 소스코드는 삭제가 된 상태에서, tsconfig.json 에 설정된 includes 설정부터 에러가 난다. 타고 들어갈 소스코드가 없으니까!!

그래서 start 의 스크립트는

node build/src/index.js

만 되어야한다.
그리고 한가지 더

```
CMD ["npm", "start"]
```

만 해서는 안된다.

```
CMD ["npm", "run", "start"]
```

해주는 것이  script 를 실행하여 정상적인 도커 Container 를 띄워주게 된다.

그리고 도커 컨테이너가 8080 포트를 열수 있도록, Docker 파일에

```
EXPOSE 8080
```

도 명시해준다.