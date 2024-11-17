
Cache 와 Query 의 관계
- 하나의 Cache 는 여러개의 Query를 소유할 수 있다.
- 하나의 Query 는 하나의 Cache 를 소유할 수 있다 == 바라볼 수 있다.
- QueryCache 에 queries 라는 Map 구조에 저장한다
	- queries 에는 넘겨받은 queryKey 로 해시한 queryHash로 구분한다.


Query 와 Observer 의 관계
- 하나의 Query 는 여러개의 Observer를 소유할 수 있다.
- 하나의 Observer 는 하나의 Query 를 소유할 수 있다. == 바라볼 수 있다.


생성 순서
- QueryClient 셋팅하면 
	- QueryClient 생성
	- QueryCache 생성
- useQuery() 를 실행하면
	- Observer 생성
	- Query 생성
	- Observer 와 Query 관계 설정


![[Pasted image 20241117181449.png]]



![[Pasted image 20241117181459.png]]


![[Pasted image 20241117181511.png]]



소스코드에서 메모

![[Pasted image 20241117181419.png]]

참고 자료
https://www.timegambit.com/blog/digging/react-query/01