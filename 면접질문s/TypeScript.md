

## Interface Vs. Type
-----

- 상속에 대한 불필요한 연산
	- 둘다 상속이 가능하다
	- interface는 extends / type 은 & 연산자로 표현한다
	- extents 는 이름을 기반으로 캐싱을 하지만, type은 캐싱이 안되고 매번 & 연산자로 연산을 해야한다
- 같은 이름 + 다른 정의
	- interface 는 같은 이름의 다른 속성으로 정의했을 시, 최종적으로 2개의 타입을 병합한다.
	- type은 에러를 뱉는다.
- 인덱스 시그니처
	- type은 암묵적 인덱스 시그니처가 가능
	- interface 는 명시해줘야 한다.



https://devocean.sk.com/blog/techBoardDetail.do?ID=165230&boardType=techBlog#:~:text=interface%EB%8A%94%20%EA%B0%9D%EC%B2%B4%20%EC%83%81%EC%86%8D%EC%9D%84,%EA%B7%B8%EB%A0%87%EC%A7%80%20%EC%95%8A%EB%8B%A4%EB%8A%94%20%EC%B0%A8%EC%9D%B4%EC%A0%90%EB%8F%84%20%EC%9E%88%EC%8A%B5%EB%8B%88%EB%8B%A4.



## TypeScript 에서 SOLID
-----


https://medium.com/zigbang/typescript%EB%A1%9C-%EB%B3%B4%EB%8A%94-solid-e9384b208f6d




## React 에서 SOLID
----


https://seokzin.tistory.com/entry/React-SOLID-%EC%9B%90%EC%B9%99%EC%9D%84-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%97%90-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0



## Zod 라이브러리
------
런타임에서도 타입을 강화시키고, 숫자같은 경우 정수냐 실수냐 등도 강력하게 타입체크가 가능해진다.

https://xionwcfm.tistory.com/346