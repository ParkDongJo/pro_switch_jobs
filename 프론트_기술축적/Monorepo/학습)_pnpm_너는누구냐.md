pnpm 은 패키지 매니저 이다. npm 의 한계점을 극복하려고 npm 진영에서 확장된 패키지 매니저이다.

# npm의 한계 (대표 2가지)
- 유령 의존성
	- v1 이하 
		- node_modules 폴더 하위로 넣는 구조가 아닌, 각 패키지별로 의존성을 nested 한 구조로 설치 되도록 설계 됨
		- 중복된 모듈만큼 다시 설치하는 낭비가 발생
	- v2 이상
		- node_modules 하위로 의존성이 걸린 패키지를 모두 최상위(root) 로 올려서 관리
		- 모듈의 중복은 없앴지만
		- 단순 특정라이브러리에서만 사용해도, 최상위(root) 로 올라가게 되면서 package.json 에 명시하지 파일조차 불러와 사용할 수 있게됨
			- ![[20260111134528.png]]
- 거대한 용량
	- NextJs 의 공식 문서에서 React 의 create-next-app 를 사용해보면, 11개의 패키지만 pacakge.json 에 명시 되어 있음에도, 실제 node_modules 에는 314개의 패키지가 설치된다. [관련자료](https://nextjs.org/docs/app/getting-started/installation)
	- 프로젝트가 커질수록 이 많은 의존성을 검증하고, 문제점을 미리 파악하기란 불가능에 가깝다.


# npm 은 이런 한계를 극복하고자 한 것이 pnpm이다.
- npm 보다 약 2배 이상 빠르다.
- 의존성 관리가 더 효율적이다.
- 모노레포 지원이 가능하다.


# 해결 방법
심볼링크
- soft link
- hard link

## content-addressable store
![[20260111135300.png]]
- 파일 내용을 기반으로 고유 hash 키 발급 하고 관리
- 버전, 패키지 이름이 달라도 같은 내용이면 같은 hash 키 사용
- 파일 기반임으로 변경된 파일에 대해서만 hash 키 갱신

이로 인해
- 패키지를 디스크에 한 번만 저장하고 재사용하기에 중복 설치를 방지한다
- 이미 설치된 패키지는 설치 과정을 생략하므로, 설치 속도가 매우 빨라진다
- `pnpm-lock.yaml` 파일에 이 해시 값이 기록된다. `pnpm`은 패키지를 설치할 때 해시를 계산하여 `pnpm-lock.yaml`에 기록된 값과 일치하는지 확인한다. 이는 패키지가 변조되었는지 검증하는 중요한 보안 메커니즘이 된다


# 글을 읽고 던진 질문
- 버전별 pnpm store 에서 해시를 어떻게 관리하는가?
	- 링크관리
		- ![[20260111135758.png]]
		- ![[20260111135814.png]]

- yarn 은 이걸 극복하지 못 했던 건가?
	- v1 이후로 방향성이 달라짐
	- v2 의 방향성은 PnP, yarn workspace 등등으로 극도의 성능과 framework 같이 설계 구조의 타이트함으로 가고 있음
	- pnpm 에 비해 규칙이 강하고, 여전히 plugin 미지원의 영역이 남아있을 수 있음


## 참고자료
---
https://po4tion.dev/pnpm
https://mugglim.tistory.com/46
https://velog.io/@minhoo0333/pnpm
https://jeonghwan-kim.github.io/2023/10/20/pnpm
https://pnpm.io/ko/motivation

