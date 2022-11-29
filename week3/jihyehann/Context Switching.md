# Context Switching (문맥 교환)

Context Switching이란 CPU(중앙 처리 장치)를 하나의 프로세스나 스레드에서 다른 프로세스나 스레드로 전환하는 것을 말한다.

![image](https://user-images.githubusercontent.com/75151848/204244404-6e17333b-ef67-4e04-b8a7-84d360bb780a.png)

이때, 하나의 Task(Process, Thread)가 CPU를 사용 중인 상태에서 다른 Task가 CPU를 사용하도록 하기 위해, 이전의 Task의 상태(문맥)를 보관하고 새로운 Task의 정보를 Register에 적재한다.

Task의 문맥은 그 프로세스의 `프로세스 제어 블록(PCB)` 에 저장된다.

<br/>

## 발생 시점

- 멀티 태스킹: 스케줄링에 따라 CPU를 사용하고 있는 프로세스가 다른 프로세스로 전환되어야 할 때가 있다.
- 인터럽트 핸들링: 인터럽트가 발생하면 자동으로 컨텍스트의 일부를 전환한다. 인터럽트를 처리하는 데 소요되는 시간을 최소화하기 위해 컨텍스트의 최소 부분만 변경하고, 인터럽트 서비스가 완료되면 인터럽트가 발생하기 전의 유효한 컨텍스트가 복원된다.
- 사용자 모드와 커널 모드 간 전환 (mode switching): 경우에 따라 mode switching 시에 context swithcing이 발생할 수 있다.

<br/>

## 오버헤드

- 문맥을 교환하는 동안에는 CPU가 유용한 작업을 수행할 수 없기 때문에, 문맥 교환 시간은 일종의 오버헤드라고 할 수 있다.

<br/>

## Process vs Thread
![](https://user-images.githubusercontent.com/75151848/183282320-b2d5eda1-2ecc-4fdf-aadb-ebe29e2bca64.png)

스레드는 스택(Stack)을 제외한 코드(Code), 데이터(Data), 힙(Heap) 영역을 공유하므로, 문맥 교환 시 스택 및 간단한 정보만 저장하면 되기 때문에 프로세스의 문맥 교환보다 빠르다.