# JVM
> Java Virtual Machine의 약자로, Java로 작성된 프로그램이 돌아가도록 만들어주는 가상 머신이다.
## JVM 관련 이미지
![image](https://user-images.githubusercontent.com/62461857/201794604-7f9f6389-1bc9-44e2-8dbb-eb7b1fd3c513.png)
![image](https://user-images.githubusercontent.com/62461857/201794714-a8e7ed02-f327-4c8b-840f-afce02805759.png)
![image](https://user-images.githubusercontent.com/62461857/201794725-2e6bf7e0-4e03-4b8a-a674-af3a97d3a168.png)
## Java의 동작 원리와 특장점
1. `자바 컴파일러`(javac)가 `자바 소스 코드`(.java)를 읽어 `자바 바이트 코드`(.class)로 변환시킨다. 
2. `Class Loader`를 통해 class 파일들을 `JVM으로 로딩`한다. 
3. 로딩된 class 파일들은 `Execution Engine` 을 통해 해석된다. 
4. 해석된 바이트 코드는 `Runtime Data Areas`에 배치되어 실질적인 수행이 시작된다.
### 스크립트 언어
PHP, JS, Python 등.
인터프리터라는 방식으로 한 라인씩 기계어로 번영하며 실행합니다. 바로바로 실행하기에는 좋다.
### 컴파일 언어
C, C++, Swift, Go
컴파일 과정을 거쳐 기계어로 번역되기 때문에 사전에 검증을 할 수 있다는 장점이 있다.
### JAVA
Java는 컴파일 시에 자바 바이트 코드로 컴파일 되며, JVM에서 인터프리터 방식으로 동작하지만, JIT Compiler 등의 기술로 네이티브 언어와 유사한 퍼포먼스를 낼 수 있다.
운영체제에 독립적으로 개발할 수 있으며, Garbage Collector가 자동으로 메모리를 관리해주고, 포인터나 다중 상속같은 까다로운 부분을 삭제하여서 개발에 편의성을 준다.
처음부터 객체 지향적 언어로 설계되어서 추상화, 캡슐화, 상속, 다형성 등의 특성을 잘 나타내는 언어이기도 하다.

## JVM, JRE, JDK
JVM 위에서 프로그램이 원활하게 실행을 하기 위해서는 몇 가지 필수적인 요소들이 있다.
이를 제공해주는 것이 JRE(Java Runtime Environment) 이다.

JRE는 크게 JVM, Java Class Libraries, Class Loader 로 구성된다.
Java Class Libraries 는 Java 를 실행시키는데 필수적인 라이브러리로, java.io, java.util, java.thread 등 작동에 필요한 기능들을 가지고 있다. 
Class Loader 는 필요한 클래스들을 JVM 으로 올려주는 역할을 한다.

JRE는 프로그램 개발이 아니라 실제 프로그램을 구동시키는 데 집중을 하고 있기 때문에, 실제 Java 코드가 주어져도 이를 분석하거나 클래스 파일로 변환하는 일은 할 수 없다. 개발을 하기 위해서는 Java Compiler 몇가지 다른 요소들이 더 필요하다.

JDK는 Java Development Kit 의 약자로 Java 를 활용하여 프로그램을 개발할 때 필요한 도구 모음이다. 
실제로 프로그램을 실행시켜보아야 하기 때문에 JRE 가 포함이 되어 있으며, Java로 이루어진 코드를 클래스 파일로 컴파일 하는 javac, 작성된 코드를 디버깅 하는 jdb 등을 가진다.
JDK에서도 개발과 실행에 필요한 환경 및 기능 제공의 폭에 따라 표준형인 SE(Standard Edition)와 여러 기능이 추가된 EE(Enterprise Edition)으로 나뉜다.

## JVM 구성 요소
JVM은 자바 가상 머신의 약자로, 자바 애플리케이션을 클래스 로더를 통해 읽어, JAVA API 와 함께 실행한다.
GC를 통해 메모리 관리를 수행하며 Stack 기반의 가상머신이다.
JVM은 아래와 같은 구조로 구성되어 있다.

- `Class Loader` : JVM 내로 클래스를 로드하고 링크를 통해 배치한다.
- `Runtime Data Areas` : 프로그램 실행 중에 사용되는 다양한 영역이다.
    - **Method Area** : 클래스 멤버 변수, 메소드 정보, Type 정보, Constant Pool, Static, final 변수 등이 생성된다.
    - **Heap Area** : 동적으로 생성된 객체와 배열이 저장되는 곳이다.
    - **Stack Area** : 지연 변수, 파라미터 등이 생성되는 영역으로, 실제 객체는 Heap 에 할당되고, 해당 레퍼런스만 stack에 저장된다.
    - **PC Register** : Thread가 시작될 때 생성되며, 현재 수행중인 JVM 명령 주소를 갖고 있다.
- `Execution Engine` : 바이트 코드를 실행한다.
    - **Interpreter** : 바이트 코드를 한줄 씩 실행한다.
    - **JIT Compiler** : 인터프리터 효율을 높이기 위한 컴파일러로, 반복되는 코드를 인터프리터가 발견하면 해당 코드를 네이티브 코드로 변경해준다.
    - **GC** : 가비지 컬렉터는 힙 영역에서 사용되지 않는 객체들을 제거하는 작업을 한다.
- Java Native Interface : 자바 애플리케이션에서 C, C++, 어셈블리어로 작성된 함수를 사용할 수 있는 방법을 제공해준다. Native 키워드를 통해 메소드를 호출하며, 대표적으로는 Thread의 currentThread()가 여기에 속한다.
- Native Method Library : C, C++ 로 작성된 라이브러리이다.

## GC
Garbage Collector는 JVM 상에서 더 이상 사용되지 않는 데이터에 할당되어있는 메모리를 알아서 해제시켜주는 장치이다. 
Grabage Collection은 보통 Heap 영역에서 이뤄진다. 스택처럼 CPU에 의해 타이트하게 관리되지 않고, 쓸 수 있는 변수크기에 제한이 없기 때문에 가만히 두면 memory leak 발생의 위험이 있기 때문이다. 
<img width="688" alt="image" src="https://user-images.githubusercontent.com/62461857/202705506-d18cb97b-9503-40c9-98c2-6c231b69e046.png">

JVM의 Heap 영역은 처음 설계될 때 아래 2가지 전제를 가졌는데, 이를 약한 세대 가설(Weak Generational Hypothesis)이라고 한다.
1.  대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
2.  오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.
이 전제 조건의 장점을 살리기 위해 JVM의 Heap 영역을 Young Generation과 Old Generation으로 나누었다.
- Young Generation
    - 새로 생성된 객체가 할당 되는 영역이다.
    - 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 생성되었다가 사라진다.
    - Young Generation 에서 일어나는 Garbage Collection 을 Minor GC 라고 한다.
    - Stop The World가 무시될 정도로 짧게 일어난다.
- Old Generation
    - Young Generation 에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역이다. 
    - Young Generation 보다 크게 할당되지만, Garbage 는 적게 발생한다.
    - Old Generation 에서 일어나는 Garbage Collection을 Major GC 또는 Full GC라고 한다.
    - Stop The World가 길게 일어난다.
 