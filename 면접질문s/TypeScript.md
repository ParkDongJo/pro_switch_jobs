

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



## Union Type, Intersection Type
----
### union 타입
#### 리터럴 타입에서
Union은 합집합이다. | 로 구분하는데 OR 연산자라고 보면 된다. 아래와 같이 사용한다.
```ts
type Marvel = "IronMan" | "Hulk";
type Dc = "BatMan" | "SuperMan";
```

#### 객체 타입에서
타입스크립트 관점에서는 `introduce()` 함수를 호출하는 시점에 `Person` 타입이 올지 `Developer` 타입이 올지 알 수가 없기 때문에 어느 타입이 들어오든 간에 오류가 안 나는 방향으로 타입을 추론하게 된다.
```typescript
interface Person {
  name: string;
  age: number;
}
interface Developer {
  name: string;
  skill: string;
}
function introduce(someone: Person | Developer) {
  someone.name; // O 정상 동작
  someone.age; // X 타입 오류
  someone.skill; // X 타입 오류
}
```


### Intersection 타입

#### 리터럴 타입에서
교집합이다. & 를 사용하고 AND 연산자라고 보면 된다. 아래와 같이 사용된다.
```ts
type FavoriteSport = "swimming" | "football";
type BallSport = "football" | "baseball";

type FavoriteBallSport = FavoriteSport & BallSport; // 'football'


type FavoriteSport = "swimming" | "ski";
type BallSport = "football" | "baseball";

type FavoriteBallSport = FavoriteSport & BallSport; // never
```


#### 객체 타입에서
인터섹션 타입(Intersection Type)은 여러 타입을 모두 만족하는 하나의 타입을 의미한다.
```typescript
interface Person {
  name: string;
  age: number;
}
interface Developer {
  name: string;
  skill: number;
}
type Capt = Person & Developer;

```

```
{
  name: string;
  age: number;
  skill: string;
}
```

한개만 아는놈.. https://fe-developers.kakaoent.com/2022/221124-typescript-tip/
캡틴판교 https://joshua1988.github.io/ts/guide/operator.html#intersection-type