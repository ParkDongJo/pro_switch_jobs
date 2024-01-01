1. 쿠팡 리더십 원칙 :  https://www.coupang.jobs/kr/why-coupang/#leadership-section
2. 쿠팡 엔지니어링 기술블로그: https://medium.com/@coupang-engineering-kr
3. 코딩 인터뷰 가이드: https://www.codinginterview.com/amazon-interview-questions
4. 코딩(알고리즘) 예제(Leetcode.com): https://leetcode.com/problemset/algorithms/?page=1&difficulty=MEDIUM&listId=wpwgkgt
5. 시스템 설계 및 디자인 가이드: https://www.educative.io/blog/complete-guide-to-system-design
6. 시스템 설계 및 디자인 인터뷰 가이드: https://www.educative.io/blog/complete-guide-system-design-interview



## Kafka


https://velog.io/@seungyeon/%EC%B9%B4%ED%94%84%EC%B9%B4-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%A0%EA%B9%8C



## AWS lambda
----

lambda 의 장점


lambda 의 단점

https://umanking.github.io/2023/03/06/aws-lambda-pros-and-cons/



## AWS
-----

### 컴퓨팅 서비스

#### EC2
AWS 서버에서 OS, CPU, 메모리 등등을 선택하여 인스턴스(가상 서버)를 생성할 수 있는 서비스

생성시
- 인스턴스 유형 선택
- VPC
- 스토리지 용량(EBS)
- 보안그룹 선택
	방화벽 기능 수행 - 허용여부만 설정 가능
	- 인바운드 : 외부 -> EC2
	- 아웃바운드 : EC2 -> 외부
	ACL

대상 추적 조정 정책
- Auto Scaling
	- 기준
		- CPU 사용율 (예 50% 이상)
		- 사용자 접속수
		- 특정 일정 설정 (주말/평일)
- 예측 스케일링 기능

#### 서버리스
사용자는 코드 개발에만 집중
- 프로그램 설치 & 코드작성
AWS 가 인프라 관리
- 소프트 웨어 작성
- 확장성 & 가용성 관리
- OS 설정





#### 컨테이너
ECS


### 스토리지

#### S3

#### EBS


## 네트워크 & 콘텐츠

#### VPC

#### ELB

#### Route53

#### CloudFront


## 데이터 베이스
#### RDS

#### Aurora

#### DynamoDB

#### DocumentDB


## 보안
#### IAM

#### CloudTrail

#### Config

#### GuardDuty

#### WAF



