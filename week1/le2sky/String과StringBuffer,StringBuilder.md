# String, StringBuffer, StringBuilder의 차이

## 불변한 String

- 자바의 String 클래스는 불변하다.
- String 클래스는 기존 문자열에 새로운 문자열을 더해 기존의 것을 변경하는 것이 아니다.
- 기존의 문자열에 새로운 문자열을 더해서 새로운 문자열을 만든다.
- 이후에 변수는 새로운 메모리 영역을 가리키게 변경되고, 기존 문자열은 Garbage로 남아있다가 시간이 지나면 GC(Garbage Collection)에 의해서 사라지게 된다.

> 문자열 연산을 수행할 때마다, 새로운 클래스 인스턴스를 만드는 것과 같다. 이는 곧 힙 메모리에 많은 임시 가비지가 생성되어 어플리케이션의 성능에 치명적인 영향을 끼치게 된다.

## 가변한 String Buffer와 String Builder

- 위와 같은 String 클래스의 한계를 보완하고자, 가변적인 String Buffer와 String Builder 클래스가 도입됐다.

- String과는 다르게 .append() .delete() 등의 API를 이용해 같은 동일 객체의 문자열을 변경하는 것이 가능하다.

- `StringBuffer`는 동기화 키워드를 지원해 thread-safe하다.
- `StringBuilder`는 동기화를 지원하지 않지만, 성능에 있어서 비교적 가장 빠르다.
