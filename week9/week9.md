# Week 9

```
📅 2023.1.28 11:00
```

## 📚 주제

1. TDD
2. index

<br/>

## 📝 질문

### TDD

<details>
<summary></summary>

<div markdown="1">
<br/>

</div>
</details>
  
### index

<details>
<summary>속도 향상을 위해 인덱스를 무조건 많이 만드는 것이 좋을까요?</summary>
<div markdown="1">

  인덱스를 관리하기 위해서는 추가적인 저장 공간이 필요하다. 
  <br/>
  DDL 수행 시 인덱스 정렬을 위한 추가적인 부하가 발생하므로 데이터베이스의 전반적인 성능에 영향을 미칠 수 있다.
  <br/>
  인덱스 생성을 남발하기보다는 효율적인 SQL 쿼리를 짜는 것이 더 중요하다.
</div>
</details>

<details>
<summary>데이터 중복도가 높은 경우, 인덱스 효율이 높지 않은 이유는?</summary>

<div markdown="1">
  
중복도가 높은 경우, 인덱스를 사용하는 것이 효율이 없지는 않지만 어차피 데이터를 읽기 위해 많은 페이지를 읽어야 하는 것은 마찬가지이기 때문에 피해야 한다.
예시로, 성별은 카디널리티가 2다. 따라서 중복도는 높고 분포도는 낮다. => 풀 테이블 스캔이 나을지도 모른다.  
 
<br/>
</div>
</details>

<details>
<summary>인덱스 손익분기점에 대해서</summary>

<div markdown="1">
  
테이블이 가지고 있는 전체 데이터양의 10-15퍼센트 이내의 데이터가 출력될때 사용하는 것이 효율적이다. <br/>
MySQL의 경우 테이블 전체 레코드 중 20-25퍼센트를 넘어가면 아예 풀 테이블 스캔을 실행한다.
  
</div>
</details>


## 👥 참여

le2sky, jinsim, jihyehann, binimini

- 발표 & 대답 : jihyehann, binimini
- 서기 : le2sky
