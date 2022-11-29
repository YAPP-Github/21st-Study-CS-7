# Week 3

```
📅 2022.11.29 09:30
```

## 📚 주제 
1. 프로세스 동기화
2. Context Switching (문맥 교환)

<br/>

## 📝 질문

### 프로세스 동기화

<details>
<summary>모니터에 대해서 설명해주세요.</summary>
<div markdown="1">       
<br/>
  
- 공유자원과 큐를 인터페이스로 제공하여 상호배제를 한다.
- Java에서의 `synchronized` , `wait` , `notify` 는 모니터를 사용한다.
  
</div>
</details>
    
<details>
<summary>뮤텍스와 이진 세마포어의 차이는?</summary>
<div markdown="1">       
<br/>
  
- 뮤텍스는 lock을 건 프로세스만이 lock을 해제할 수 있다.
- 세마포어는 lock을 걸지 않은 프로세스도 Signal을 보내 lock을 해제할 수 있다.

</div>
</details>

<details>
<summary>spinlock 방식은 왜 사용하나? (어떤 상황에서 사용하는지)</summary>
<div markdown="1">       
<br/>
  
- busy wait를 사용하면 컨텍스트 스위칭이 발생하지 않기 때문에 점유 시간이 길지 않은 경우 오버헤드가 줄어들 수 있다. 
  
</div>
</details>

### Context Switching (문맥 교환)

<details>
<summary>모드 스위칭과 프로세스 스위칭 간의 차이점은?</summary>
<div markdown="1">       
<br/>
  
- 모드 스위칭이란 사용자 모드와 커널 모드 간의 전환을 말하며, 외부 인터럽트, 내부 트랩, 시스템콜의 경우 발생한다. 이때 현재 프로세스 상태를 저장한다. 
- 프로세스 스위칭은 모드 스위칭이 일어난 후에 경우에 따라 발생할 수도 있고 발생하지 않을 수도 있다. 모드 스위칭만 하고 프로세스 스위칭을 하지 않는 경우 process context의 저장과 간단한 복원만 일어나기 때문에 overhead가 적다.
  
</div>
</details>

<br/>

## 👥 참여

- 발표 & 대답 : le2sky, jinsim
- 질문 : jihyehann, binimini
- 서기 : jihyehann
