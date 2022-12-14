## String Builder, String Buffer

### String
- 불변 객체임
- 변경이 불가능하고, 변경(문자열 연산)을 하게 되면 매번 새로운 String 객체가 생성되기 때문에 메모리 문제 발생할 수 있음 (`+` 연산 `concat()` 등)
- 하지만 불변 객체이므로 `Thread-safe`하게 동작


불변 객체란?
- 객체 생성 이후 내부의 상태가 변하지 않는 객체
- 재할당은 가능하지만 데이터 변경은 불가능
- read-only 메소드만 제공
- 객체의 내부 상태를 제공하는 메소드를 제공하지 않거나 방어적 복사를 통해 제공

불변 객체의 장단점?
- 장점
    - Side Effect 없애 오류 가능성 줄일 수 있음
    - 코드 예측 가능하며 안전하게 사용 가능
    - 필드로 사용할 경우 방어적 복사 필요하지 않음
    - 동기화 처리 필요하지 않음
        - 동기화 동시에 여러 스레드 공유 자원에 대해서 write 작업 수행할 때 필요하므로
- 단점
    - 매번 새로운 객체 생성하므로 비용 소모될 수 있음


방어적 복사란?
- 생성자를 통해 초기화하거나 내부의 객체를 반환할 때, 새로운 객체로 감싸서 복사
- 주소값 공유 끊어주기 위해서
- 깊은 복사와의 차이?
    - 인스턴스로 초기화 하는 복사는 진행
    - 원본에서 사용한 객체 내부 요소들은 그대로 사용
- 깊은 복사
    - 객체 내부 요소들도 새로운 요소로 할당(초기화)
- 얕은 복사
    - 같은 메모리 주소를 참조
    - 값 변경시 원본 객체도 변경됨
### StringBuffer
  - 가변 객체
  - 문자열 연산시 기존의 버퍼 크기를 늘리며 유연하게 동작할 수 있으므로 수정 시 매번 새로운 객체가 생성되는 String의 단점을 보완
  - 또한 `Synchronized` 키워드를 통해 멀티 스레드 환경에서도 동기화를 지원
  - 하지만 단일 스레드 환경에서 좋지 않은 성능을 보임
    - 단일 스레드에서 좋지 않은 성능인 이유?
    - 멀티 스레드 환경에서 동기화를 지원하기 위한 처리가 되어있음
    - `Synchronized` 키워드 - 자원 사용하려고 할때 lock 소유하고 있어야함
    - 여러개의 스레드가 한개의 자원을 사용하고자 할 때, 한 스레드를 제외한 나머지 스레드들은 데이터에 접근 할 수 없도록 막음
### StringBuilder
  - 가변 객체
  - Lock을 걸지 않기 때문에 단일 스레드 환경에서 좋은 성능
  - `Synchronized` 키워드 사용하지 않음
