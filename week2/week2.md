# Week 2

```
📅 2022.11.22 09:30
```

## 📚 주제 
1. Transaction
2. N+1 문제

<br/>

## 📝 질문

### Transaction
<details>
<summary>readOnly 옵션 설정할 경우 성능 향상 어떻게?</summary>
<div markdown="1">       
<br/>
  
- FLUSH 사용하지 않음
- snapshot 만들지 않기 때문에 성능 향상
- MySQL의 경우에는, 읽기 전용 트랜잭션에는 트랜잭션 id 부여하지 않아서 오버헤드 감소
- read-only 트랜잭션에서 영속성 컨텍스트?
  - snapshot 생성하지 않는다
  - flush 하지 않음
  - 변경 감지하지 않는다 (dirty checking) 
  ⇒ 성능 향상
</div>
</details>

<details>
<summary>JPA 내 JDBC 사용하는 것으로 아는데, JPA로 한번 더 감싸서 사용하는 이유?</summary>
<div markdown="1">       
<br/>
  
- **JPA 사용하는 이유** 
  - 엔티티에 대한 CRUD 메서드 자동 생성
  - 데이터베이스와 객체 지향 프로그래밍의 패러다임 차이 해결 (객체와 테이블 매핑)
    
- **JPA 내 JDBC 사용하는 것으로 아는데, JPA로 한번 더 감싸서 사용하는 이유**
  - 동기화
  - 추상화
  - AOP와 같은 기능 이용해서 @Transactional 어노테이션 사용 가능
</div>
</details>
       
<details>
<summary>트랜잭션 하나 당 하나의 영속성 컨텍스트 사용?</summary>
<div markdown="1">       
<br/>
  
- 트랜잭션 하나 당 하나의 영속성 컨텍스트 사용함
- 트랜잭션 시작하면 영속성 컨텍스트 생성되고, 
- 커밋/롤백되면서 트랜잭션 종료될 경우 영속성 컨텍스트도 닫히게됨
</div>
</details>
    
    
<details>
<summary>트랜잭셔널 메서드 내에서 트랜잭셔널 메서드 호출할 경우?</summary>
<div markdown="1">       
<br/>
  
- `@Transactional`의 propagation 옵션에 따라서 처리가 달라짐
- 영속성 컨텍스트 트랜잭션에 따라 사용됨  
</div>
</details>

<details>
<summary>Spring에서 영속성 컨텍스트와 트랜잭션 범위 다르게 가져가는 방법?</summary>
<div markdown="1">       
<br/>
  
- **OSIV(Open Session In View)**
  - 영속성 컨텍스트의 생존 범위 지정
  - `spring.jpa.open-in-view : true` (기본값) : 커넥션 시작부터 API 응답이 끝날 때까지 영속성 컨텍스트 유지
    - Controller에서도 지연 로딩을 사용 할 수 있음
    - DB 커넥션을 오랫동안 사용해 트래픽이 몰리면 커넥션 부족해질 수 있음
  - `spring.jpa.open-in-view : false` : 트랜잭션이 종료될 때 영속성 컨텍스트를 닫고 DB커넥션도 반환
    - 커넥션 리소스를 낭비하지 않음
    - 모든 지연로딩을 트랜잭션 안에서 처리해야함  
</div>
</details>

### N+1
<details>
<summary>JPQL 사용하는 이유</summary>
<div markdown="1">       
<br/>
    
- 객체 지향적으로 쿼리 날릴 수 있으니까 패러다임 불일치 문제 해결할 수 있음
- DB에 따라 SQL의 문법이 달라질 수 있음, JPQL 사용시 DB에 대해서 의존성이 없어짐
</div>
</details>


<details>
<summary>Batch Size</summary>
<div markdown="1">       
<br/>
    
- BatchSize에 size를 명시
- 1 객체를 가져올 때 해당 size만큼을 한 번에 가져오고,
- 해당 가져온 엔티티들의 id를 IN 절의 조건문으로 사용해서 연관 객체들을 조회하는 쿼리를 날리게 된다
- 즉 N+1번의 쿼리 대신 2번의 쿼리로 사용할 수 있게됨
</div>
</details>

<details>
<summary>패치 조인에서의 inner join 다른 방식과 차이?</summary>
<div markdown="1">       
<br/>
    
- 패치 조인 - inner join 방식
- 엔티티 그래프 - outer join 방식  
- 공통적으로는 둘 다 카티션곱을 사용해서 데이터 중복이 발생할 수 있다.
</div>
</details>

<details>
<summary>패치 조인과 일반 조인 차이가 무엇일까요</summary>
<div markdown="1">       
<br/>
    
- (team과 member가 1대 다 일때 select t from Team t join fetch t.members랑 fetch 뺀 거 차이)
- 일반 조인 
  - select 대상만을 조회
  - select 대상 영속화해서 가져오고, 조인 대상은 가져오지 않음
  - 이후 패치 전략에 따라 연관 객체 가져오게됨
- 패치 조인  
  - 조인 대상까지 한번에 영속화해서 가져오게된다
</div>
</details>
    

## 👥 참여

le2sky, jihyehann, jinsim, binimini	

- 발표 & 대답 : le2sky, jihyehann
- 질문 : jinsim, binimini
- 서기 : binimini
