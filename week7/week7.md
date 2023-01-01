# Week 7

```
📅 2022.12.27 23:30
```

## 📚 주제

1. Web Server
2. JWT

<br/>

## 📝 질문

### Web Server

<details>
<summary>웹서버가 필요한 이유라고 말하면, 처음으로 나오는게, 분리함으로써 
was에 대한 부하를 줄여준다고 하는데, 톰캣 같은 경우 5.5 부터는 오히려 웹서버를 분리하면
성능이 줄어든다고 한다. 정말인가?</summary>
<div markdown="1">
   
<br/>
톰캣은 웹서버 역할을 할 수 있지만(기능 자체는 가능하지만),
웹 서버만의 장점을 제대로 살릴 수 없게 된다. (역방향 프록시 등)
  
</div>
</details>

<details>
<summary>nginx를 사용한 이유는 무엇인가?</summary>
<div markdown="1">
  <br/> 
  nginx가 성능이 좋았고, 리버스 프록시 기능만 사용했기 때문에 다른 웹서버를 사용할 이점이 없었다.
</div>
</details>

  
### JWT

<details>
<summary>액세스 토큰 + 리프레시 토큰과 함께 사용하는 다른 방식이 있는가?</summary>
<div markdown="1">
 
<br/>
슬라이딩 세션은 서비스를 자주 사용하는 사용자의 경우 만료 시간을 늘려주는 방식이다.
  
</div>
</details>

<details>
<summary>외부 스토리지에 리프레시 토큰을 관리할 경우에는 추가적인 I/O 작업이 생겨 기존 JWT의 장점이 깨진다. 이에 대해서 어떻게 생각하는가?</summary>
<div markdown="1">
 
<br/>

  
</div>
</details>


## 👥 참여

le2sky, jinsim, jihyehann, binimini

- 발표 & 대답 : jinsim, binimini
- 서기 : le2sky
