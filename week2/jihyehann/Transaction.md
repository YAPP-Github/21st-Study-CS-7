# Transaction

<details>
<summary>트랜잭션 개념</summary>
<div markdown="1">       

[참고](https://github.com/jaejlf/CS_Study/blob/main/Database/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98(Transaction)/jihyehann.md)

트랜잭션이란 데이터베이스의 상태를 변환시키는 하나의 논리적 기능을 수행하기 위한 작업의 단위를 말하며, ACID 성질을 가지고 있다.

- Atomicity (원자성): 트랜잭션의 연산은 데이터베이스에 모두 반영되든지 아니면 전혀 반영되지 않는다. 
- Consistency (일관성): 트랜잭션 작업 처리의 결과가 항상 일관되어야 한다. 
- Isolation (독립성,격리성): 둘 이상의 트랜잭션이 동시에 병행 실행되는 경우 어느 하나의 트랜잭션 실행 중에 다른 트랜잭션의 연산이 끼어들 수 없다.
- Durablility (영속성,지속성): 성공적으로 완료된 트랜잭션의 결과는 (시스템이 고장나더라도) 영구적으로 반영되어야 한다.

<br/>

</div>
</details>

<details>
<summary>트랜잭션 격리 수준</summary>
<div markdown="1">       

[참고](https://github.com/jaejlf/CS_Study/blob/main/Database/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%20%EA%B2%A9%EB%A6%AC%20%EC%88%98%EC%A4%80(Transaction%20Isolation%20Level)/jihyehann.md)

트랜잭션 격리 수준이란, 동시에 여러 트랜잭션이 처리될 때, 트랜잭션끼리 얼마나 서로 고립되어 있는지를 나타내는 것이다.

- Read Uncommitted: 커밋되지 않은 데이터를 다른 트랜잭션에서 읽을 수 있다.
- Read Committed: 커밋된 데이터만 다른 트랜잭션에서 읽을 수 있다.
- Repeatable Read: 선행 트랜잭션이 읽은 데이터는 트랜잭션이 종료될 때까지 다른 트랜잭션이 수정/삭제할 수 없다.
- Serializable: 선행 트랜잭션이 읽은 데이터를 다른 트랜잭션이 수정/삭제/삽입할 수 없다.

아래로 내려갈수록 트랜잭션 간 고립 정도가 높아지며, 성능이 떨어진다.

</div>
</details>

<br/>

### 트랜잭션을 병행으로 처리하려고 할 때 발생할 수 있는 문제

[참고](https://popo015.tistory.com/40)

- 갱신 내용 손실 (Lost Update) : 동시에 하나의 데이터가 갱신될 때 하나의 갱신이 누락되는 경우
- 현황 파악 오류 (Dirty Read) : 하나의 데이터 갱신이 끝나지 않은 시점에서 다른 트랜잭션이 해당 데이터를 조회하는 경우
- 모순성 (Inconsistency) : 두 트랜잭션이 동시에 실행될 때 데이터베이스가 일관성이 없는 모순된 상태로 남는 문제
- 연쇄 복귀 (Cascading Rollback) : 두 트랜잭션이 하나의 레코드를 갱신할 때 하나의 트랜잭션이 롤백하면 다른 하나의 트랜잭션 마저 롤백이 되는 문제

&rarr; [`동시성 제어 기법`](https://velog.io/@ha0kim/%EB%8F%99%EC%8B%9C%EC%84%B1-%EC%A0%9C%EC%96%B4) 을 통해 이러한 문제를 예방할 수 있다.



<br/>


### 스프링에서 트랜잭션을 다루는 방법

![](https://velog.velcdn.com/images/wisdom-one/post/3e5b9048-c717-463e-aef3-d62cee461d37/image.png)

- 스프링에서는 트랜잭션 추상화 기술을 제공한다.
- 특정 기술에 종속되지 않는 일관된 방식으로 트랜잭션 적용 가능
- PlatformTransactionManager 인터페이스가 트랙잭션 추상화의 핵심  
+) 스프링 5.3부터는 JDBC 트랜잭션을 관리할 때 DataSourceTransactionManager 를 상속받아서 약간의 기능을 확장한 JdbcTransactionManager 를 제공한다. 둘의 기능 차이는 크지 않으므로 같은 것으로 이해하면 된다.

<details>
<summary>PlatformTransactionManager</summary>
<div markdown="1">       

<br/>
  
```java
public interface PlatformTransactionManager extends TransactionManager {

	TransactionStatus getTransaction(@Nullable TransactionDefinition definition)
			throws TransactionException;

	void commit(TransactionStatus status) throws TransactionException;

	void rollback(TransactionStatus status) throws TransactionException;

}
```

- getTransaction: 지정된 propagation behavior에 따라 현재 활성 트랜잭션을 반환하거나 새 트랜잭션을 만든다.
- definition: TransactionDefinition 인스턴스(기본값의 경우 null일 수 있음), 전파 동작, 격리 수준, 시간 초과 등을 설명
- commit: 해당 상태와 관련하여 주어진 트랜잭션을 커밋한다. 트랜잭션이 프로그래밍 방식으로 롤백 전용으로 표시된 경우 롤백을 수행.
- rollback: 지정된 트랜잭션의 롤백을 수행한다.

<br/>
</div>
</details>



<details>
<summary>트랜잭션 매니저의 역할</summary>
<div markdown="1">     

<br/>
  
- 트랜잭션 추상화 : PlatformTransactionManager 인터페이스를 통해 트랜잭션을 추상화한다.
- 트랜잭션 동기화 :  트랜잭션을 시작하기 위한 Connection 객체를 특별한 저장소에 보관해두고 필요할 때 꺼내쓸 수 있도록 하는 기술. 트랜잭션 동기화 매니저(TransactionSynchronizationManager 추상 클래스)를 제공한다. 트랜잭션 동기화 매니저는 ThreadLocal을 사용해서 커넥션을 동기화해준다. ThreadLocal을 사용하기 때문에 멀티스레드 환경에서도 안전하게 커넥션을 동기화할 수 있다. 

![](https://velog.velcdn.com/images/wisdom-one/post/1900087f-2721-445b-858f-3b980052e806/image.png)


동작
1. 트랜잭션을 시작하려면 커넥션이 필요하다. 트랜잭션 매니저는 데이터소스를 통해 커넥션을 만들고 트랜잭션을 시작한다.
2. 트랜잭션 매니저는 트랜잭션이 시작된 커넥션을 트랜잭션 동기화 매니저에 보관한다.
3. 리포지토리는 트랜잭션 동기화 매니저에 보관된 커넥션을 꺼내서 사용한다. (따라서 파라미터로 커넥션을 전달하지 않아도 된다.)
4. 작업이 완료되면 트랜잭션 매니저는 트랜잭션 동기화 매니저에 보관된 커넥션을 통해 트랜잭션을 종료하고, 커넥션도 닫는다.

<br/>
</div>
</details>

<br/>


### `@Transactional` 어노테이션

[공식문서](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Transactional.html)

- 선언적 트랜잭션 방식
- 일련의 작업들을 묶어서 하나의 단위로 처리할 때 사용 (해당 범위 내 메서드가 트랜잭션이 되도록 보장해준다.)
- 개별 메서드 혹은 클래스의 트랜잭션 속성 설정
- PlatformTransactionManager가 관리하는 스레드 바인딩된 트랜잭션에서 작동
- 현재 실행 스레드 내의 모든 데이터 액세스 작업에 트랜잭션을 노출 
- 단, 메서드 내에서 새로 시작한 스레드에는 전파되지 않음!


<details>
<summary>@Transactional을 사용하지 않으면?</summary>
<div markdown="1">  

<br/>
  
![](https://velog.velcdn.com/images/wisdom-one/post/9b0ac4f4-5190-4c43-91bb-4e3e7c0b2ce0/image.png)

![](https://velog.velcdn.com/images/wisdom-one/post/8cf4dca8-c284-437f-b725-de4eb70258ec/image.png)

- 기본적으로 JDBC의 트랜잭션은 하나의 Connection Instance를 생성하고 통신하며 종료하는 흐름과 같이 동작하게 된다. 즉 코드에 존재하는 DAO 로직들은 각각의 트랜잭션 안에서 연산을 진행하게 된다.

<br/>
</div>
</details>


<details>
<summary>@Transactional을 사용하면?</summary>
<div markdown="1">  

<br/>
  
![](https://velog.velcdn.com/images/wisdom-one/post/a6e11c1f-b127-45cc-bba0-a351dd8112a2/image.png)

![](https://velog.velcdn.com/images/wisdom-one/post/0614e5e5-776b-4928-ab0b-3a75e0dac7b3/image.png)

[참고](https://lob-dev.tistory.com/36)

<br/>
</div>
</details>


> 💡 JPA를 사용한다면 단일 작업에 대해서는 @Transactional을 직접 선언할 필요가 없다.    
> JPA의 구현체를 살펴보면 모든 메서드에 이미 @Transactional이 선언되어 있기 때문에 문제가 발생하면 Rollback 처리해주기 때문이다.    
> 따라서, 여러 작업을 하나의 단위로 묶어 Commit or Rollback 처리가 필요할 때 직접 선언해주면 된다!   
 
<br/>

**동작 과정**

@Transactional 어노테이션은 **AOP**로 구현되어 있다!

- 클래스나 개별 메소드에 @Transactional이 선언되면 해당 클래스에 트랜잭션이 적용된 프록시 객체가 생성된다.
- 프록시 객체는 @Transactional이 포함된 메서드가 호출될 경우, 트랜잭션을 시작하고 Commit or Rollback을 수행한다.
- CheckedException or 예외가 없을 때는 Commit을 수행하고, UncheckedException이 발생하면 Rollback을 수행한다.
- 스프링 컨테이너는 트랜잭션 범위의 영속성 컨텍스트 전략을 기본으로 사용한다.
@Transactional 어노테이션이 포함된 메서드를 호출하여 트랜잭션이 시작될 때 영속성 컨텍스트가 생기고, 메서드가 종료되어 트랜잭션을 커밋할 경우, 영속성 컨텍스트가 flush되면서 해당 내용이 반영되고, 이후에 영속성 컨텍스트 역시 종료된다. [[참고]](https://kafcamus.tistory.com/30)


![](https://velog.velcdn.com/images/wisdom-one/post/810ba87c-51d3-49dc-8e5d-549d835ce169/image.png)

이러한 방식으로 영속성 컨텍스트를 관리해 주기 때문에, @Transactional을 쓸 경우 트랜잭션의 원칙을 정확히 지킬 수 있다.

또한, 아래의 원칙 역시 유의해야 한다.

- 만약 같은 트랜잭션 내에서 여러 EntityManager를 쓰더라도, 이는 같은 영속성 컨텍스트를 사용한다.
- 같은 EntityManager를 쓰더라도, 트랜잭션이 다르면 다른 영속성 컨텍스트를 사용한다.

<br/>

**Test 환경에서의 `@Transactional` 동작**

테스트 메서드가 트랜잭션으로 감싸지며, 메서드가 종료될 때 자동으로 롤백된다.

<br/>

<details>
<summary>@Transactional 옵션</summary>
<div markdown="1">   

<br/>
  
```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Transactional {
	@AliasFor("transactionManager")
	String value() default "";

	@AliasFor("value")
	String transactionManager() default "";
    
    String[] label() default {};

	Propagation propagation() default Propagation.REQUIRED;

	Isolation isolation() default Isolation.DEFAULT;

	int timeout() default TransactionDefinition.TIMEOUT_DEFAULT;

	String timeoutString() default "";

	boolean readOnly() default false;
	
	Class<? extends Throwable>[] rollbackFor() default {};

	String[] rollbackForClassName() default {};

	Class<? extends Throwable>[] noRollbackFor() default {};

	String[] noRollbackForClassName() default {};

}
```

readOnly (기본값 false)
- 해당 트랜잭션이 readOnly 인지 나타내는 flag
- readOnly = true로 설정하게 되면, 스프링은 해당 트랜잭션의 FlushMode를 NEVER로 설정한다. 이때 flush가 일어나지 않으므로 비용이 절감되며, 생성/수정/삭제가 일어나지 않으므로 별도의 스냅샷을 만들 필요가 없어 성능상 이점이 생긴다.
- 데이터를 변경하는 Operation이 없으면 readOnly를 true로 주는 것이 성능상 좋다.

propagation (기본값 Propagation.REQUIRE)
- 트랜잭션의 경계에서 이미 진행중인 트랜잭션이 있거나 없을 때 어떻게 동작할 것인가를 결정
- REQUIRED: 현재 트랜잭션이 있는지 확인하고, 있으면 기존 트랜잭션을 사용하고 없으면 새 트랜잭션을 생성한다.
- SUPPORTS: 현재 트랜잭션이 있는지 확인하고, 있으면 기존 트랜잭션을 사용하고, 없으면 트랜잭션 없이 실행한다.
- MANDATORY: 현재 트랜잭션을 사용하고, 없으면 예외가 발생한다.
- REQUIRES_NEW: 새 트랜잭션을 생성한다. 현재 트랜잭션이 존재하는 경우 일시 중단한다.
- NOT_SUPPORTED: 트랜잭션 없이 실행한다. 현재 트랜잭션이 존재하는 경우 일시 중단한다.
- NEVER: 트랜잭션 없이 실행하고, 현재 트랜잭션이 존재하는 경우 예외가 발생한다.
- NESTED: 현재 트랜잭션이 있으면 중첩(자식) 트랜잭션을 시작한다. 현재 트랜잭션이 없다면 REQRUIED처럼 동작한다. NESTED에 의한 중첩 트랜잭션은 먼저 시작된 부모 트랜잭션의 커밋과 롤백에는 영향을 받지만, 자신의 커밋과 롤백은 부모 트랜잭션에게 영향을 주지 않는다.

isolation (기본값 Isolation.DEFAULT)
- Transaction의 Isolation level. 별도로 정의하지 않으면 DB의 Isolation Level을 따른다.
- DEFAULT: RDBMS가 지정한 값으로 설정된다. 
- READ_UNCOMMITTED
- READ_COMMITTED: Postgres와 SQL Server 및 Oracle에서 사용하는 격리 수준.
- REPEATABLE_READ: Mysql에서 사용하는 격리 수준.
- SERIALIZABLE: 동시 호출을 순차적으로 실행한다(성능이 가장 느리다)

timeout (기본값 -1)
- 지정한 시간 내에 해당 메소드 수행이 완료되지 않을 경우 rollback 수행한다.
- `-1` : no time out

rollbackFor
- 정의된 Exception에 대해서 rollback을 수행한다.

rollbackForClassName
- 정의된 이름의 Exception에 대해서 rollback을 수행한다.

noRollBackFor
- 정의된 Exception에 대해서는 rollback을 수행하지 않는다.

transactionManager, value
- 사용할 트랜잭션 관리자를 지정한다.

<br/>
</div>
</details>



