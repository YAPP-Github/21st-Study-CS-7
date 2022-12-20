# Week 6

```
📅 2022.12.20 23:30
```

## 📚 주제

1. Scale-up Scale-out
2. HTTP HTTPS

<br/>

## 📝 질문

### Scale-up Scale-out

<details>
<summary>scale-out 에서 세션 불일치 문제가 발생하게 될 때 어떻게 해결할 수 있나요?</summary>
<div markdown="1">
  <br/>
  
- Session Clustering
- Sticky Session
- 외부 Storage 사용 (Session storage를 분리)
  
</div>
</details>

<details>
<summary>코어가 늘어남에 따라 마냥 성능이 증가하지는 않고, 코어 증가에 따라 대역폭은 증가해 지연이 발행할 가능성이 있는 이유는?</summary>
<div markdown="1">
</div>
</details>

<details>
<summary>scale-up, scale-out은 각각 언제 적합할까?</summary>
<div markdown="1">
  <br/>
  
- scale-up은 데이터 update가 빈번하게 일어날 때
- scale-out 데이터 변화가 적고, 병렬적인 처리를 할 때
  
</div>
</details>

<details>
<summary>로드 밸런싱에 대해 설명해주세요.</summary>
<div markdown="1">
  <br/>
  
- 서버에 가해지는 부하를 적절하게 분산시켜주는 기술
- 라운드로빈 방식, IP 해시 방식, 최소 연결 방식 등
  
</div>
</details>

  
### HTTP HTTPS

<details>
<summary>HTTP/1.0 과 비교했을 때 HTTP/1.1 에서 추가된 부분은?</summary>
<div markdown="1">
  <br/>
  
- 커넥션 유지 (Persistent Connection), 파이프라이닝 등
  
</div>
</details>

<details>
<summary>HTTP/2.0에서 추가된 기능은?</summary>
<div markdown="1">
  <br/>
  
- binary 프레임
- Multiplexing : 하나의 TCP 연결을 통해 여러 데이터 요청을 병렬로 전송 → HOL Blocking 해소
- 헤더 압축 등
  
</div>
</details>

## 👥 참여

le2sky, jinsim, jihyehann, binimini

- 발표 & 대답 : jinsim, binimini
- 서기 : jihyehann
