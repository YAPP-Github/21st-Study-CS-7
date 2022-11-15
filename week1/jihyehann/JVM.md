# JVM (Java Virtual Machine)

## 개념

JVM은 자바를 실행하기 위한 가상 기계로써, OS에 종속받지 않고 java를 실행할 수 있도록 해준다.  
Java compiler가 .java 파일을 .class 라는 자바 바이트코드로 변환하는데, JVM에서는 이 바이트코드를 OS에 특화된 코드로 변환하여 실행한다.

### JRE (Java Runtime Environment)

JRE는 JVM 위에서 프로그램이 원활하게 실행을 하기 위해 필요한 필수적인 요소들을 제공하며 크게 JVM , Java Class Libraries 그리고 Class Loader 로 구성된다.   
Java Class Libraries 는 Java 를 실행시키는데 필수적인 라이브러리로, java.io, java.util, java.thread 등 작동에 필수적인 요소들을 가지고 있다.

### JDK (Java Development Kit)

Java 를 활용하여 프로그램을 개발할 때 필요한 도구 모음이다.  
실제로 프로그램을 실행시켜보아야 하기 때문에 `JRE` 가 포함되어 있다.

<br/>

## 구조

![img_4.png](https://user-images.githubusercontent.com/75151848/201724974-805db83b-4a0f-4987-b24a-32e4bdb83d09.png)

JVM은 크게 아래와 같이 이루어져 있다.

- [클래스 로더(Class Loader)](#1-class-loader)
- 런타임 데이터 영역 (Runtime Data Area)
  - Method Area
  - Heap
  - Java Stack
  - Native Method Stack
  - PC(Program Counter) Register
- 실행 엔진(Execution Engine)
  - 인터프리터(Interpreter)
  - JIT 컴파일러(Just-in-Time)
  - 가비지 콜렉터(Garbage collector)

<br/>

### 1. Class Loader

class file(.class 확장자를 가진 파일)을 runtime data area에 적재하는 역할을 한다.

#### 1) Loading : 클래스를 JVM에 탑재한다.
<img width="500" src="https://user-images.githubusercontent.com/75151848/201725046-e7f19f30-768e-4c01-b501-a09cc130ada7.png"/>

- Bootstrap class loader : JVM을 구동시키기 위한 가장 필수적인 라이브러리의 클래스들을 로딩한다. (JVM 내부)
- Extension class loader : 추가적인 기능들을 위한 Java Class의 라이브러리들을 로딩한다. (JRE)
- Application class loader : classpath에 있는 클래스를 로딩한다. 개발자가 작성한 클래스 파일은 이 클래스 로더에 의해 JVM에 탑재된다. (JRE)

#### 2) Linking : 로드된 클래스 파일들을 검증하고, 사용할 수 있게 준비한다.
- Verification : 클래스 파일이 유효한지를 확인한다. 클래스 파일이 JVM 의 구동 조건 대로 구현되지 않았을 경우에는 VerifyError 를 던진다.
- Preparation : 클래스 및 인터페이스에 필요한 static field 메모리를 할당하고, 기본값으로 초기화한다. 기본값으로 초기화된 static field 값들은 뒤의 Initialization 과정에서 코드에 작성한 초기값으로 변경된다.
- Resolution : Symbolic Reference 값을 JVM 의 메모리 구성 요소인 Method Area 의 런타임 환경 풀을 통하여 Direct Reference 라는 메모리 주소 값으로 바꿔준다.

#### 3) Initialization : static 값을 초기화하고 변수를 할당한다.
- class 와 interface 의 값들을 지정한 값들로 초기화 및 초기화 메서드를 실행시켜준다. 이때, JVM 은 멀티 쓰레딩으로 작동을 하며, 같은 시간에 한 번에 초기화를 하는 경우가 있기 때문에 초기화 단계에서도 동시성을 고려해주어야 한다.

<br/>

### 2. Runtime Data Area

<img width="500" src="https://user-images.githubusercontent.com/75151848/201724512-fbf79b85-effe-462e-ab69-7e783e2c4df9.png"/>

JVM 의 Run-Time Data Area는 크게 Method Area, Heap, Java Stack, PC registers, Native Method Stack 으로 구성된다.

#### 1) Method Area

- 인스턴스 생성을 위한 객체 구조, 생성자, 필드 등이 저장된다.
- Runtime Constant Pool 과 static 변수, 그리고 메소드 데이터와 같은 Class 데이터들도 이곳에서 관리된다.
- 이 영역은 JVM 당 하나만 생성되며, JVM 의 모든 Thread 들이 Method Area 을 공유하게 된다.
- JVM 의 다른 메모리 영역에서 해당 정보에 대한 요청이 오면 실제 물리 메모리 주소로 변환해서 전달해준다.
- JVM 구동 시작 시에 생성되며, 종료 시까지 유지되는 공통 영역이다.

#### 2) Heap

- 코드 실행을 위한 Java 로 구성된 객체 및 JRE 클래스들이 탑재된다.
- 문자열에 대한 정보를 가진 String Pool 뿐만이 아니라 실제 데이터를 가진 인스턴스, 배열 등이 저장된다.
- JVM 당 하나만 생성되고, 해당 영역이 가진 데이터는 모든 Java Stack 영역에서 참조되어, Thread 간에 공유된다.
- Heap 영역이 가득 차게 되면 `OutOfMemoryError` 가 발생한다.


#### 3) Java Stack

- 각 Thread 별로 따로 할당되는 영역이다.
- Heap 메모리 영역보다 비교적 빠르고, 동시성 문제에서 자유롭다는 장점이 있다.
- 각 Thread 들은 메소드를 호출할 때마다 `Frame` 이라는 단위를 추가(push)하고, 메소드가 마무리되어 결과를 반환하면 해당 Frame 은 Stack 으로부터 제거(pop)된다.
  - Frame : 메소드에 대한 정보를 가지고 있는 Local Variable, Operand Stack, Constant Pool Reference 로 구성되어 있다.
  - Local Variable : 메서드 안의 지역 변수
  - Operand Stack : 메서드 내 연산을 위한 바이트 코드 명령문들이 있는 공간
  - Constant Pool Reference : Constant Pool 참조를 위한 공간
- Java Stack 영역이 가득 차게 되면 `StackOverflowError` 를 발생한다.

#### 4) Native Method Stack

- Native Method : 다른 프로그래밍 언어로 작성된 메소드
- Native Method Stack 은 Java로 작성되지 않은 메소드들을 다루는 영역이다.
- Java Stack과 마찬가지로, Native Method가 호출되면 Stack에 해당 메서드가 쌓인다.

#### 5) PC(Program Counter) Register

- 현재 실행되고 있는 명령어의 주소를 저장하고 있는 영역이다.
- 각각의 스레드는 자신만의 PC Register를 가지고 있다.
- 멀티스레드 환경에서 한 스레드가 작업을 하다가 다른 쓰레드로 잠시 CPU 점유를 넘겨주고, 다시 돌아왔을 때 이전에 어떤 명령어를 수행하고 있었는지 기억하고 있어야 하기 때문에 필요하다.
- 만약, 실행했던 메소드가 native method라면 undefined가 기록되고, java 메소드라면 JVM에서 사용된 명령의 주소값을 저장한다.

<br/>

### 3. Execution Engine

- Load 된 바이트코드를 실행하는 Runtime Module이다.
- 명령어 단위로 읽어서 실행하는데, 인터프리터 방식과 JIT(Just In Time) 컴파일 방식을 혼합하여 사용한다.

#### 1) Interpreter

- 바이트코드 명령을 읽고 순차적으로 실행한다.
- 같은 메소드라도 매번 기계어로 변환하여, 성능이 느리다는 단점이 있다.

#### 2) JIT(Just-In-Time) 컴파일러

<img width="500" src="https://user-images.githubusercontent.com/75151848/201724640-a592af07-65a5-4ed5-9eaf-47250b5bc95e.png" /> 

- JIT 컴파일러는 프로그램을 실제 실행하는 시점에 기계어로 번역하는 컴파일러를 말한다.
- 인터프리터 방식의 단점을 보완하기 위해 도입되었다.
- 인터프리터 방식으로 실행하다가 적절한 시점(반복되는 코드를 발견하면)에 바이트코드를 컴파일하여 기계어로 변환한다. 이렇게 되면 그 이후에는 인터프리터가 매번 해당 코드를 기계어로 변환하지 않고도 바로 사용할 수 있게 된다.

#### 3) Garbage Collector

- 참조되지 않은 객체를 수집하고 제거하여 메모리를 확보한다.
- Garbage Collection 참고

#### +) Java Native Interface(JNI)

- 자바 메서드 호출과 해당 native library 사이의 브리지(중개자) 역할을 한다.

<img width="500" src="https://user-images.githubusercontent.com/75151848/201724720-d76b96fa-62ad-448a-9a69-4d0c0584659b.png" />

<br/>

## 꼬리 질문

<details id="gc">
<summary>가비지 컬렉션 (Garbage Collection, GC) 이란?</summary>
<div markdown="1">  
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

</div>
</details>

<br/><br/>

> 🔖 참고
> - [JVM 구조](https://m.blog.naver.com/whdgml1996/221999082469)
> - [JVM](https://doozi0316.tistory.com/entry/1%EC%A3%BC%EC%B0%A8-JVM%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%93%9C%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%8B%A4%ED%96%89%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%B8%EA%B0%80)
> - [jvm jre jdk](https://tecoble.techcourse.co.kr/post/2021-07-12-jvm-jre-jdk/)
> - [classloader](https://tecoble.techcourse.co.kr/post/2021-07-15-jvm-classloader/)
> - [Run-Time Data Area](https://tecoble.techcourse.co.kr/post/2021-08-09-jvm-memory/)  