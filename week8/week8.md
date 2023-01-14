# Week 8

```
📅 2022.1.14 22:00
```

## 📚 주제

1. redis
2. 자바에서 null을 안전하게 다루는 방법

<br/>

## 📝 질문

### redis

<details>
<summary>레디스의 싱글 스레드 특성으로 주의해야할 점?</summary>

<div markdown="1">
<br/>
 
 레디스는 싱글 스레드로 동작하기 때문에 한 번에 하나의 명렁어만 수행한다.
 
 따라서 데이터를 조회할 때 전체 키를 조회하는 명령어처럼 많은 데이터를 한 번에 조회하는 명령어 사용에는 주의해야한다.
 
 해당 명령어를 사용하게 되면 처리해야하는 명령어들이 딜레이(블로킹)되기 때문.

</div>
</details>
  
### 자바에서 null을 안전하게 다루는 방법

<details>
<summary>계약에 대한 설계?</summary>
<div markdown="1">
<br/>

 API 규약을 지켜야할 계약으로 여기는 설계 방법 (인터페이스)
 
 개방-폐쇄 원칙의 상위 개념
  
</div>
</details>

## 👥 참여

le2sky, jinsim, jihyehann, binimini

- 발표 & 대답 : jihyehann, jinsim
- 서기 : binimini
