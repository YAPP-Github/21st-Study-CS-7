# **Context Switching**

## Process

실행 중인 프로그램이다. HDD나 SSD 등의 저장 장치에 저장되어 있는 프로그램이 메모리에 로드되면서 프로세스가 된다.

CPU는 한 번에 하나의 일만 할 수 있다. 

**주기억장치에 적재되어 커널 영역에 PCB를 부여받은 프로그램**

프로세스는 아래와 같은 Process Address Space라는 구조 따라 메모리에 적재된다. 

<img width="384" alt="image" src="https://user-images.githubusercontent.com/62461857/210219778-2afc6be1-9c59-4923-a582-c13599b4e82f.png">

<img width="408" alt="image" src="https://user-images.githubusercontent.com/62461857/210219794-00f2a314-fbcb-4615-9279-5db3d8676bb0.png">

**Code Segment**

코드가 저장되어 있는 부분. 프로그램의 코드는 바뀌어서는 안되므로 읽기만 가능하다.
프로그램의 코드는 컴파일 이후에 변경되지 않으므로 프로세스끼리 공유할 수 있도록 Read Only를 시켜 분리해두었다.

**Stack Segment**

함수나 함수 안에 있는 지역변수와 매개변수를 저장하는 부분.
함수의 실행은 스택 구조를 따라가므로 Stack 영역을 분리해두었고, 함수 실행이 완료되면 메모리가 제거된다.
일정 수 이상의 재귀 함수 호출 시 프로세스가 강제 종료되는 이유는, 호출된 함수가 차지하는 메모리가 stack을 넘어 heap 을 침범할 수 있기 때문이다.

**Heap Segment**

필요에 따라 동적으로 할당된 메모리를 저장하는 부분. 
프로그램이 실행되는 동안 메모리의 크기를 정하므로 효율적이다.

**Data Segment**

전역 변수 같은 정적인 데이터를 관리하는 부분. 
함수의 바깥에 있는 데이터(전역 변수)를 저장해 놓으며, 프로그램이 완전히 종료되어야 없어진다.

## Process State (프로세스 상태)

<img width="432" alt="image" src="https://user-images.githubusercontent.com/62461857/210219819-b20fd2f3-169f-4416-bbbf-fc50a209f095.png">

- **New (생성 상태)**
    - 프로세스가 새로 생성되어 메모리에 올라와 실행 준비를 완료한 상태.
    - 프로그램이 커널 영역에 PCB를 부여받은 상태이다. (PCB 생성 완료)
    - OS가 프로세스를 생성한 후 주기억장치 공간이 여유로운지 확인하고, 공간이 충분하다면 프로세스 주소 공간을 할당한 후 Ready 상태로 변경시킨다. 공간이 부족한 경우에는 Suspend-ready 상태로 변경시킨다.
        - 주기억장치 = 메인메모리 = **RAM (Random Access Memory)**
        컴퓨터의 CPU가 현재 처리중인 데이터나 명령만을 일시적으로 저장하는 휘발성 메모리
        - 보조기억장치 = 하드디스크 SSD
        전원을 끄더라도 저장된 데이터나 정보가 날아가지 않는 비휘발성 메모리
    - 보통은 바로 Suspend-ready 나 Ready로 변경되므로 잠시 거쳐간다.
- **Ready (준비 상태)**
    - 생성된 프로세스들이 실행 가능한 상태에서 CPU를 할당 받을 때까지 대기하는 상태.
    - 여러 프로세스가 주기억장치에 적재되는 다중 프로그래밍 환경에서는, 큐와 리스트로 프로세스를 관리한다.
    - OS가 Ready 상태의 여러 프로세스 중 CPU를 할당하는 순서를 정하는 것이 **CPU 스케줄링**이다.
- **Running (실행 상태)**
    - CPU에서 프로세스가 실행 중인 상태.
    - Ready 상태의 프로세스가 CPU를 할당 받으면 Running 상태가 되고, CPU는 해당 프로세스를 실행한다.
    - CPU 스케줄링에 따라 CPU를 뺏길 수 있고, 뺏긴 프로세스는 Ready 상태가 된다.
    - I/O 작업을 요청을 하게 되면 I/O 작업이 종료될 때까지 Waiting 상태가 된다. 
    (Ready 상태인 다른 프로세스가 CPU를 할당 받는다.)
- **Waiting(block) (대기 상태)**
    - I/O 작업을 요청한 후, 해당 I/O 작업이 끝날때까지 대기하는 상태.
    - Running 상태인 프로세스가 I/O 작업 등 바로 확보될 수 없는 자원을 요청하면, CPU를 반납하고 해당 작업이 완료될 때까지 대기 상태가 된다.
        - Ready와 마찬가지로 큐나 리스트로 관리된다.
    - 요청한 작업이 완료되면 Ready 상태가 된다.
- **End(terminated) (종료 상태)**
    - 프로세스가 종료되어 사용했던 데이터와 PCB를 삭제하는 상태.
    - 할당된 모든 자원들이 회수되고 PCB만 커널에 남아있다가, OS가 해당 프로세스의 흔적을 정리한 후 PCB를 삭제하면 프로세스가 완전히 없어지게 된다.
    - 이때까지는 아직 프로세스가 메모리에 올라가 있긴 하다.
    - 프로세스가 사라지는 과정에서 잠깐 거쳐간다.

### 활성 상태와 보류 상태

보통 상태 지속 시간이 짧은 **New**, **End**를 제외하고, **Ready**, **Running**, **Waiting**을 **활성 상태**라고 한다.

활성 상태는 **메모리 공간을 부여**받고, **주기억장치에 적재되어있는 프로세스**들을 말한다.

만약 메모리가 부족하거나 혹은 다른 이유에 의해 프로세스의 **메모리를 회수**하고 **보조기억장치로 내보내는** 경우 = (Swapped Out)가 있는데, 이 상태를 **보류 상태**라고 한다.

활성 상태와 달리 보류 상태에 있는 프로세스는 **메모리에 적재되어 있지 않으며**, Swap 영역에 보관된다.

- **Suspend-Ready (보류 준비 상태)**
    - 생성된 프로세스가 바로 메모리를 받지 못하거나, Ready, Running 상태의 프로세스가 메모리를 잃게 될 때 변경됩니다.
    - 주기억장치에 여유가 생기거나 Ready 상태인 프로세스가 전혀 없을 때, Waiting 상태의 프로세스를 Suspend-Waiting 상태로 만들고 Ready 상태로 변경됩니다.
- S**uspend-Waiting (보류 대기 상태)**
    - Waiting 상태에서 메모리 공간을 잃은 상태이다. 보류 대기 상태의 프로세스는 입출력 등의 작업 종료 시 Suspend-Ready 상태로 변경됩니다.

## Context Switching

- 멀티프로세스 환경에서 CPU가 어떤 하나의 프로세스를 실행하고 있는 상태에서
- 인터럽트 요청에 의해 다음 우선 순위의 프로세스가 실행되어야 할 때
- 기존의 프로세스의 상태 또는 레지스터 값(Context)을 저장하고
- CPU가 다음 프로세스를 수행하도록 새로운 프로세스의 상태 또는 레지스터 값(Context)를 **교체하는 작업을**
- **Context Switching** 이라고 한다.

### Context

**Context** : 사용자와 다른 사용자, 사용자와 시스템 또는 디바이스간의 상호작용에 영향을 미치는 사람, 장소, 개체등의 현재 상황(상태)을 규정하는 정보들

OS에서의 **Context** : CPU가 해당 프로세스를 실행하기 위한 해당 프로세스의 정보들이다. **프로세스의 PCB(Process Control Block)**에 저장된다. 
프로그램 카운터, CPU 레지스터들의 값, 메모리 관리 상태 등이 포함된다.

그래서 Context Switching 때 PCB의 정보를 읽어서 CPU가 이전에 프로세스가 하던 일에 이어서 수행이 가능한 것이다.

### Context Switching 과정
<img width="412" alt="image" src="https://user-images.githubusercontent.com/62461857/210219876-ad49eaf5-5cd4-4d93-ab38-2247a14c63fb.png">

- P0를 실행 중 interrupt가 발생하거나 system call이 발생하면 P0의 상태를 PCB0에 저장한다.
- PCB1에 저장된 P1의 상태를 불러와 복구한다.
- P1를 실행한다.
- 이때 다시 interrupt가 발생하거나 system call이 발생하면 P1의 상태를 PCB1에 저장한다.
- PCB0에 저장된 P0의 상태를 불러와 복구한다.
- P0를 실행한다.

즉, ready, running, waiting이 반복돼서 실행한다.
이걸 위해서는, 현재 실행중인 프로세스의 정보를 저장하는 것이 필요하다.
이때 사용하는 자료구조가 바로 PCB이다.

### Interrupt

인터럽트는 CPU가 프로그램을 실행하고 있을 때 실행중인 프로그램 밖에서 예외 상황이 발생하여 처리가 필요한 경우 CPU에게 알려 예외 상황을 처리할 수 있도록 하는 것을 말한다.

1. I/O request (입출력 요청할 때)
2. time slice expired (CPU 사용시간이 만료 되었을 때)
3. fork a child (자식 프로세스를 만들 때)
4. wait for an interrupt (인터럽트 처리를 기다릴 때)

## Process Control Block (프로세스 제어 블록)

프로그램이 실행되면, 메모리에 적재하면서 프로세스가 생성되고, 프로세스 메타 데이터를 생성해서 PCB에 저장한다. 
PCB는 프로세스가 생성될 때 삽입되고, 프로세스가 완료되면 삭제된다. (LinkedList 방식으로 관리되어 삽입과 삭제에 용이하다)

Context Switching이란, 수행 중인 Task(Process/Thread)가 변경될 때, CPU의 레지스터 정보가 변경되는  것을 의미한다. 즉, 이전 프로세스의 상태를 PCB에 보관하고, 다른 프로세스의 정보를 PCB에서 읽어 와서 CPU 레지스터에 적재하는 것이다.

PCB에는 현재 CPU에 의해 실행 중인 프로세스에 관해, CPU 레지스터가 갖고 있는 정보들을 저장한다.
PCB에 프로세스 정보를 저장해두고, 다른 프로세스의 PCB에 있는 정보를 CPU의 레지스터에 로드하여 다른 프로세스를 실행하게 된다.

- Process Id : 프로세스에 시스템이 할당해주는 고유 번호
- Process State : 프로세스 실행 상태 (ready, wait, running, blocked, end, suspend-wait, suspend-ready)
- Program Counter(PC) : 다음 실행될 명령어의 위치
- CPU registers : Context Switching이 발생하면 레지스터 정보를 기억하고, 프로세스가 CPU를 할당 받으면 다시 사용한다.
- CPU scheduling information : CPU 스케줄링 정보
- Memory-management information : 메모리 관리 및 보호 정보 보관
- Process Accounting information : CPU 사용 시간, CPU 할당 시간 등
- Process I/O status information : 프로세스 실행 중에 할당을 요구한 I/O 장치에 대한 정보

## CPU 스케줄링

- 멀티프로세스 환경에서는 여러 프로세스가 동시에 실행되는 것처럼 보이기 위해, 프로세스간 **CPU 자원 할당 이동(**Context Switching) 을 한다.
    - CPU는 **기존 할당된 프로세스의 Context를 저장하고**, **새로 자원을 할당할 프로세스의 Context로 교체하는 과정**
- Context Switching이 진행 중일 때는, CPU는 아무 일도 하지 못 한다. 따라서 너무 자주 발생하게 되면 CPU의 성능이 떨어진다.
- 필요한 순간에 적절하게 Context Switching을 발생시키는 알고리즘을 OS 스케줄러가 정하고 사용한다.
    - 우선순위가 높은 프로세스에 CPU 자원을 먼저 할당해준다.

### 비선점 스케줄링

어떤 프로세스가 CPU를 점유하고 있다면 해당 프로세스의 작업이 완료 될 때까지 다른 프로세스는 CPU를 사용할 수 없다.

**FCFS (First Come First Service) , SJF (Shortest Job First) , HRN (Highest Response Ratio Next)**

### 선점 스케줄링

어떤 프로세스가 CPU를 점유하고 있을 때 우선순위가 높은 다른 프로세스가 점유를 빼앗아 CPU를 점유할 수 있다. 
긴급히 처리되어야 할 프로세스가 먼저 처리 될 수 있지만, Context Switching이 자주 일어날 수 있다.

**SRT (Shortest Remaining Time) , 라운드로빈 (Round-Robin) , 다단계 큐 (Multi-Level Queue)**

## 멀티 프로세스, 멀티 스레드, 멀티 코어

<img width="423" alt="image" src="https://user-images.githubusercontent.com/62461857/210219908-aff0ed42-dc9e-4f5b-b652-d8be05974fb8.png">

<img width="421" alt="image" src="https://user-images.githubusercontent.com/62461857/210219935-91e4e9db-3e44-4e5b-a549-2c7eb5a0edc7.png">

### 멀티 프로세스

하나의 프로그램을 여러 개의 프로세스로 구성하여, 각 프로세스가 병렬적으로 작업을 수행하는 것

- 장점
    - 메모리 침범 문제를 OS 차원에서 해결
    - 각 프로세스는 독립적이기에, 여러 프로세스 중 하나에 문제가 생겨도 확산되지 않는다.
    - 동기화 작업이 필요하지 않는다.
- 단점
    - 각 프로세스가 독립된 메모리 영역을 가지고 있기 때문에 Context Switching 비용이 커서, 작업량이 많아지면 오버헤드가 발생한다.
        - PCB와 TCB를 모두 주고받아야 한다.
    - 프로세스 간의 복잡한 통신(IPC)이 필요하다.

### 멀티 스레드

하나의 프로세스에서 여러 스레드를 구성해, 각 스레드가 작업을 처리하는 것

- 장점
    - 메모리 공간과 시스템 자원의 효율성이 증가한다.
    - Data, Heap, Code 공유 영역을 이용해 데이터를 주고받으므로 Context Switching 비용이 작아, 시스템의 처리량과 프로그램의 응답 시간이 단축된다.
        - TCB만 주고받으면 된다.
    - 스레드 간의 통신이 간단하다.
- 단점
    - 서로 다른 스레드가 Stack을 제외한 메모리 공간을 공유하기 때문에, 동기화 문제가 발생할 수 있다.
    - 하나의 스레드에 문제가 생기면 전체 프로세스가 영향을 받는다.
    - 공유 자원 관리 등 주의 깊은 설계가 필요하며, 디버깅이 까다롭다.
- 예) 인터넷 익스플로러에서 각 탭이 멀티 스레드로 이뤄져 있으면, 한 탭이 에러나면 다 꺼진다. 크롬은 멀티 프로세서를 이용하기 때문에 조금 비효율적이어도 탭간의 영향이 덜하다.

### 멀티 코어

앞서 2개는 **처리 방식**의 일종이기에 SW 분야에 가깝고, 멀티 코어는 하드웨어의 측면에 가깝다.

- 싱글 코어를 가진 CPU가 실행단위를 처리할 때는, 동시에 여러 작업을 하기 위해서 Context Switching 되면서 처리된다. = 동시성
- 멀티 코어는, 둘 이상의 코어에서 동시에 하나의 프로세스나 스레드가 한꺼번에 진행되는 것 = 병렬성

### Reference
[https://velog.io/@kbpark9898/프로세스-컨텍스트-스위칭](https://velog.io/@kbpark9898/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8-%EC%8A%A4%EC%9C%84%EC%B9%AD)
[https://rainbow97.tistory.com/entry/운영체제-3장-Process-Concept](https://rainbow97.tistory.com/entry/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-3%EC%9E%A5-Process-Concept)
[https://whereisusb.tistory.com/10](https://whereisusb.tistory.com/10)
[https://code-lab1.tistory.com/41](https://code-lab1.tistory.com/41)
[https://jeong-pro.tistory.com/93](https://jeong-pro.tistory.com/93)
[https://steady-coding.tistory.com/503](https://steady-coding.tistory.com/503)
[https://jutudy.tistory.com/m/20](https://jutudy.tistory.com/m/20)
[https://lotuslee.tistory.com/85](https://lotuslee.tistory.com/85)
[https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=skddms&logNo=110176736594](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=skddms&logNo=110176736594)
