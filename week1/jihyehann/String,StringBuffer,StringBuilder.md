# String & StringBuffer & StringBuilder

String, StringBuffer, StringBuilder 모두 문자열을 다루는 클래스라는 점에서 동일하지만, 몇 가지 차이점이 있다.

<br/>

## 불변성
### String - 불변

<img width="260" src="https://user-images.githubusercontent.com/75151848/201724248-3d92bd36-5bd5-4931-88dd-13c329092922.png"/>

- String 클래스는 불변이다(immutable).
- 즉, 한 번 값이 할당되면 수정할 수 없다.
- 문자열을 변경하기 위해서 String Pool에 메모리를 할당받아 새로운 문자열 객체를 만들어야 한다.
- 그리고 기존 문자열 객체는 더이상 참조되지 않기 때문에 GC 대상이 된다.
- 따라서, 변하지 않는 문자열을 자주 읽어들이는 경우에는 String을 사용하는 것이 좋지만, 문자열에 추가/수정/삭제 등의 연산이 빈번하게 발생한다면 메모리 낭비와 불필요한 작업으로 인해 성능에 영향을 끼칠 수 있다.
  - 기존 문자열이 GC에 의해 제거되어야 한다.
  - 새로운 문자열 객체를 계속해서 만들어야 한다.
- 특히, 문자열 연결은 매우 느리다.  <br/><br/>
  <img width="400" src="https://user-images.githubusercontent.com/75151848/201724364-1adf7624-0bd1-435c-9a92-822c59a9fa86.png"/>

  > 🔖 [Effective Java](https://velog.io/@wisdom-one/%EC%95%84%EC%9D%B4%ED%85%9C-63.-%EB%AC%B8%EC%9E%90%EC%97%B4-%EC%97%B0%EA%B2%B0%EC%9D%80-%EB%8A%90%EB%A6%AC%EB%8B%88-%EC%A3%BC%EC%9D%98%ED%95%98%EB%9D%BC)에서도 문자열 연결 시에는 StringBuilder의 append 메서드를 사용하라고 권장하고 있다.

<br/>

### StringBuilder, String Buffer - 가변

<img width="300" src="https://user-images.githubusercontent.com/75151848/201725250-67f094da-0dcb-4678-b35c-83f3422a5a7d.png"/>

- 반면, StringBuilder와 StringBuffer는 모두 가변적(mutable)이다.
- 두 클래스의 경우, 내부 버퍼에 문자열을 저장해두고 그 안에서 추가, 수정, 삭제 작업을 할 수 있다.


<br/>

## Thread-safe

### String, StringBuffer - 쓰레드 안전

- String은 불변성을 가지고 있기 때문에, 멀티스레드 환경에서도 당연히 안전하다.
- StringBuffer의 경우, 동기화를 지원하기 때문에 멀티스레드 환경에서 안전하다.


### StringBuilder - 쓰레드 안전하지 않음

- StringBuffer는 단일 스레드 환경에서만 사용하도록 설계되어, 동기화를 지원하지 않는다.
- 즉, 멀티스레드 환경에서는 적합하지 않다.
- 그러나 단일스레드에서는 StringBuffer보다 성능이 우수하다는 장점이 있다.

<br/>

## 정리

- `String` :  문자열 연산이 적고 멀티쓰레드 환경일 경우
- `StringBuffer` :  문자열 연산이 많고 멀티쓰레드 환경일 경우
- `StringBuilder` :  문자열 연산이 많고 단일쓰레드이거나 동기화를 고려하지 않아도 되는 경우

<br/>

&nbsp; | String | StringBuffer | StringBuilder  
--|:--:|:--:|:--:  
Storage | String pool (Heap 영역 내) | Heap | Heap  
Modifiable | No (immutable) | Yes (mutable) | Yes (mutable)  
Thread safe | Yes | Yes | No  
Synchronized | Yes | Yes | No
Performance | Fast | Slow | Fast

<br/>

## 꼬리 질문

<details>
<summary>String Constants Pool이란? (String Interning)</summary>
<div markdown="1">       
<br/>
String 객체를 new 키워드를 통해 생성하지 않고, 리터럴로 선언하게 되면, Heap 내의 특별한 공간인 String Pool에 문자열이 저장된다.  
String Pool에 저장하고 사용하는 것을 `String Interning` 이라고 한다.  

String Constants Pool은 일종의 HashMap으로, 리터럴 선언을 통해 문자열 객체를 생성할 때 다음의 과정을 거친다.
1. string constant pool에 동일한 값이 있는지 확인한다.
2. 동일한 값이 이미 존재하는 경우, 참조만 반환한다.
   없는 경우, 새로운 문자열 객체를 생성하고 string pool에 추가한 다음 참조를 반환한다.

이 방식은 문자열을 생성할 때 시간이 더 걸리지만 공간 절약을 할 수 있다.   
단, new 키워드를 사용할 경우, 새로운 String 객체를 생성하도록 강제한다.

<br/>
</div>
</details>

<details>
<summary>String은 왜 불변으로 설계되었을까?</summary>
<div markdown="1">  
<br/>

- String Pool을 사용할 수 있다.
- 해커가 값을 변경할 수 없으므로 보안이 강화된다. 데이터베이스의 중요한 정보(회원 비밀번호 등)를 저장할 경우 사용하기 좋다.
- 멀티스레드 환경에서 스레드 안전하고, 동기화할 필요가 없다.

<br/>
</div>
</details>


<details>
<summary>가비지 컬렉션 (Garbage Collection, GC) 이란?</summary>
<div markdown="1">  
<br/>

JVM의 Heap 영역에서 동적으로 할당했던 메모리 영역 중 필요 없게 된 메모리 영역을 주기적으로 삭제하는 프로세스를 말한다. <br/>
Java는 JVM에 탑재되어 있는 가비지 컬렉터가 메모리 관리를 대행해주기 때문에 개발자 입장에서 메모리 관리, 메모리 누수(Memory Leak) 문제에서 대해 완벽하게 관리하지 않아도 되어 오롯이 개발에만 집중할 수 있다는 장점이 있다. <br/>

Heap 영역은 물리적으로 2가지 영역으로 나뉜다.
- Young 영역(Yong Generation 영역): 새롭게 생성한 객체의 대부분이 여기에 위치한다. 대부분의 객체가 금방 접근 불가능 상태가 되기 때문에 매우 많은 객체가 Young 영역에 생성되었다가 사라진다. 이 영역에서 객체가 사라질때 `Minor GC` 가 발생한다고 말한다.
- Old 영역(Old Generation 영역): 접근 불가능 상태로 되지 않아 Young 영역에서 살아남은 객체가 여기로 복사된다. 대부분 Young 영역보다 크게 할당하며, 크기가 큰 만큼 Young 영역보다 GC는 적게 발생한다. 이 영역에서 객체가 사라질 때 `Major GC` (혹은 `Full GC` )가 발생한다고 말한다.

![img_7.png](https://user-images.githubusercontent.com/75151848/201724451-b6c036e9-b76e-44ad-9e38-8783ba73f19e.png)
- Permanent Generation 영역(이하 Perm 영역) : Method Area라고도 한다.
  객체나 억류(intern)된 문자열 정보를 저장하는 곳으로, Old 영역에서 살아남은 객체가 영원히 남아 있는 곳은 절대 아니다.
  이 영역에서 GC가 발생할 수도 있는데, 여기서 GC가 발생해도 Major GC의 횟수에 포함된다.  
  Java 8 버전 이후로 Native 영역에 존재하는 `Metaspace` 라는 영역으로 대체되었다.


**Young 영역**

3개의 영역으로 나뉜다.
- Eden 영역
- Survivor 영역(2개)

처리 절차
- 새로 생성한 대부분의 객체는 Eden 영역에 위치한다.
- Eden 영역에서 GC가 한 번 발생한 후 살아남은 객체는 Survivor 영역 중 하나로 이동된다.
- Eden 영역에서 GC가 발생하면 이미 살아남은 객체가 존재하는 Survivor 영역으로 객체가 계속 쌓인다. 하나의 Survivor 영역이 가득 차게 되면 그 중에서 살아남은 객체를 다른 Survivor 영역으로 이동한다. 그리고 가득 찬 Survivor 영역은 아무 데이터도 없는 상태로 된다.
- 이 과정을 반복하다가 계속해서 살아남아 있는 객체는 Old 영역으로 이동하게 된다.

"Eden 영역에 최초로 객체가 만들어지고, Survivor 영역을 통해서 Old 영역으로 오래 살아남은 객체가 이동한다."

**Old 영역**

Old 영역은 기본적으로 데이터가 가득 차면 GC를 실행한다.  
시간이 지나 Old영역에 할당된 메모리가 허용치를 넘게 되면, Old 영역에 있는 모든 객체들을 검사하여 참조되지 않는 객체들을 한꺼번에 삭제하는 GC가 실행된다.
이러한 Major GC는 시간이 오래 걸리는 작업이고 이때 GC를 실행하는 스레드를 제외한 모든 스레드는 작업을 멈추게된다(stop-the-world).

<br/>
</div>
</details>



<br/>

[참고](https://github.com/devham76/tech-interview-study/blob/master/contents/java.md#stringstringbufferstringbuilder)   
[참고](https://ifuwanna.tistory.com/221)   
[가비지 컬렉션](https://d2.naver.com/helloworld/1329)   
