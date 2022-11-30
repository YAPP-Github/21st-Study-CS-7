### Transaction

- 데이터베이스의 상태를 변화시키기 해서 수행하는 작업의 단위
  - 상태를 변화시킴 → SQL 질의어를 통해 DB에 접근
  - 작업 단위 → SQL 명령문들을 사람의 기준에 따라 묶기

**Transaction의 특징**

- **Atomicity - 원자성**
  - 한 트랜잭션 내의 일련의 작업들은 모두 성공하거나 모두 실패해야함
- **Consistency - 일관성**
  - 모든 트랜잭션은 일관성 있는 데이터베이스 상태를 유지
  - ex) 데이터베이스에서 정한 무결성 제약 조건을 항상 만족
- **Isolation - 격리성**
  - 동시에 실행되는 트랜잭션들이 서로에게 영향을 미치지 않아야함
  - ex) 동시에 같은 데이터를 수정하지 못함
  - 동시성과 관련된 성능 이슈로 인해 적절한 격리 수준을 선택해야함
- **Durability - 지속성**
  - 하나의 트랜잭션 성공적으로 수행되었을 경우, 그 결과가 항상 기록되어야함
  - ex) 시스템 문제 발생해도 데이터베이스 로그 등을 통한 트랜잭션 내용 복구

**Transaction 격리 수준**

- **Read Uncommitted**
  - 커밋하지 않은 변경 사항 다른 트랙잭션에서 읽는 것 허용
  - Dirty Read
    - 실행 중인 다른 트랜잭션에서의 완료되지 않은 변경사항 보임
- **Read Committed**
  - 커밋한 변경사항만 다른 트랙잭션 읽을 수 있음
  - 언두 영역의 백업 레코드에서 가져와 데이터 조회
  - 대부분 서비스 default로 사용하는 격리 수준
  - Non-Repeatable Read
    - 한 트랜잭션 같은 쿼리 두번 실행시 결과값 달라질 수 있음
    - 중간에 다른 트랜잭션이 해당 레코드 변경 커밋할 경우
- **Repeatable Read**
  - 트랜잭션이 범위 내에서 조회한 데이터에 대해서 항상 동일함 보장
  - 언두 영역에 백업된 이전 데이터 이용해 동일 트랜잭션 내 동일한 결과 보장
  - MySQL InnoDB 스토리지 엔진에서 default로 사용하는 격리 수준
  - Phantom Read
    - 일정 범위 레코드 읽을 때 처음에는 없던 레코드 생길 수 있음
    - 트랜잭션 도중 새로운 레코드 삽입 허용하기 때문에
- **Serializable**
  - 가장 단순한, 가장 엄격한 격리 수준
  - 문제 발생하지 않지만
  - 동시 처리 성능 다른 트랜잭션 격리 수준보다 떨어짐
  - 읽기 작업도 공유 잠금(읽기 잠금)을 획득
    - 동시에 다른 트랜잭션 변경 불가

### Spring Transaction

**Spring에서의 트랜잭션과 영속성 컨텍스트 범위**

- **OSIV(Open Session In View)**
  - 영속성 컨텍스트의 생존 범위 지정
  - `spring.jpa.open-in-view : true` (기본값) : 커넥션 시작부터 API 응답이 끝날 때까지 영속성 컨텍스트 유지
    > Binds a JPA EntityManager to the thread for the entire processing of the request.
    - Controller에서도 지연 로딩을 사용 할 수 있음
    - DB 커넥션을 오랫동안 사용해 트래픽이 몰리면 커넥션 부족해질 수 있음
  - `spring.jpa.open-in-view : false` : 트랜잭션이 종료될 때 영속성 컨텍스트를 닫고 DB커넥션도 반환
    - 커넥션 리소스를 낭비하지 않음
    - 모든 지연로딩을 트랜잭션 안에서 처리해야함

**`@Transactional`**

- 클래스나 메서드에 설정할 경우, 해당 범위 내 메서드가 트랜잭션이 되도록 보장
- `isolation`
  - 격리 수준 선택
  - `DEFAULT` : 사용하는 DB의 기본 격리 수준 사용
  - `READ_UNCOMMITTED`
  - `READ_COMMITTED`
  - `REPEATABLE_READ`
  - `SERIALIZABLE`
- `propagation`
  - 진행하는 트랜잭션이 존재할 때 새로운 트랜잭션 메서드 호출시의 대응 방식
  - `REQUIRED` : 기본값, 부모 트랜잭션이 존재할 경우 참여하고 없는 경우 새 트랜잭션을 시작
  - 이외 옵션값들
    - `SUPPORTS`: 부모 트랜잭션이 존재할 경우 참여하고 없는 경우 non-transactional 상태로 실행
    - `MANDATORY`: 부모 트랜잭션이 있으면 참여하고 없으면 예외 발생
    - `REQUIRES_NEW`: 부모 트랜잭션을 무시하고 무조건 새로운 트랜잭션이 생성
    - `NOT_SUPPORTED`: non-transactional 상태로 실행하며 부모 트랜잭션이 존재하는 경우 일시 정지시킴
    - `NEVER`: non-transactional 상태로 실행하며 부모 트랜잭션이 존재하는 경우 예외 발생
    - `NESTED`:
      - 부모 트랜잭션과는 별개의 중첩된 트랜잭션을 만듬
      - 부모 트랜잭션의 커밋과 롤백에는 영향을 받지만 자신의 커밋과 롤백은 부모 트랜잭션에게 영향을 주지 않음
      - 부모 트랜잭션이 없는 경우 새로운 트랜잭션을 만듬 (`REQUIRED` 와 동일)
      - DB 가 SAVEPOINT 를 지원해야 사용 가능 (Oracle)
      - `JpaTransactionManager` 에서는 지원하지 않음
- `readOnly`
  - 기본값은 `false` `true` 경우 트랜잭션을 읽기 전용으로 변경
    - flush 일어나지 않음
    - dirty checking(변경 감지) 수행하지 않음
  - 해당 트랜잭션 내에서 읽기 작업만 수행 (DB에 따라서는 readOnly 무시하고 쓰기 연산 실행되기도함)
    - MySQL의 경우
      - SELECT 연산만 지원
      - read-only transaction에 id 부여하지 않음
      - ⇒ 불필요한 ID 설정에 대한 오버헤드가 발생하지 않음
  - 데이터 변경 불가능 로직임을 코드로 표시
- `rollbackFor`
  - 기본적으로 트랜잭션은 종료 시 변경된 데이터를 커밋
  - 특정 Exception 발생 시 데이터를 커밋하지 않고 롤백하도록 변경
    - 기본값 `RuntimeException` `Error`
  - `rollbackFor` 속성으로 다른 Exception 을 추가해도 `RuntimeException`이나 `Error`는 여전히 데이터를 롤백
  - 강제로 데이터 롤백을 막고 싶다면 `noRollbackFor` 옵션으로 지정
- `timeout`
  - 정한 시간 내 해당 메소드 수행이 완료되지 않은 경우 `JpaSystemException` 발생
  - 초 단위로 지정할 수 있으며 기본값인 -1 인 경우엔 timeout 을 지원하지 않음
- 테스트 환경에서 사용할 경우
  - 기본적으로 종료시 롤백 설정 (`@Rollback` true로 설정됨)
  - `WebEnvironment`의 `RANDOM_PORT` `DEFINED_PORT` 사용시 별도의 스레드 내 테스트 수행 ⇒ 트랜잭션 롤백되지 않음
  - AI 설정시 트랜잭션 내 삽입 연산으로 증가된 id 값 감소되지 않음
    - 트랜잭션과 AI 별도로 작동되기 때문
    - 동시에 여러 트랜잭션 실행될 수 있기 때문에
