# Week 4

```
📅 2022.12.06 23:00
```

## 📚 주제 
1. Docker
2. Git flow

<br/>

## 📝 질문

### Docker


<details>
<summary>Docker Registry와 Docker Hub</summary>
<div markdown="1">       
<br/>
Docker Registry 내에 Public Registry, Private Registry 가 있다. <br/>
Public Registry의 예시가 Docker Hub, AWS ECR 등.
</div>
</details>

<details>
<summary>Windows나 Mac에서는 Docker가 어떻게 동작하는가</summary>
<div markdown="1">       
<br/>
Docker는 Linux 환경에서만 네이티브로 동작한다. Linux 이외의 환경에서 Docker를 사용할 때는 Docker 자체가 OS에서 지원하는 가상화 환경에서 구동된다. <br/>
이 가상화 기술로는 Mac은 xhyve, Windows는 Hyper-V 를 사용하여 별도의 Linux 환경을 만들 수 있다. <br/>
Docker의 핵심인 Docker Engine은 이 가상화 환경의 Linux 위에서 돌아가게 된다. <br/>
</div>
</details>

<details>
<summary>Docker 이미지 용량 줄이는 법</summary>
<div markdown="1">       
<br/>
가벼운 Base image를 사용 <br/>
불필요한 Layer를 줄인다. <br/>
불필요한 파일을 정리한다. <br/>
멀티-스테이지 빌드 <br/>
</div>
</details>

### Git flow

<details>
<summary>Git Flow를 사용하는 이유</summary>
<div markdown="1">       
<br/>
Github Flow은 Master와 Feature 2개만 존재.<br/>
Gitlab Flow는 Master과 Production 사이에 Pre-Production도 존재.<br/>
요새는 CICD 도 잘 되어 있기에 간단한 위 두 개를 사용하는 경우도 많다.
</div>
</details>

<details>
<summary>CI/CD</summary>
<div markdown="1">       
<br/>
</div>
</details>

<details>
<summary>reset vs revert</summary>
<div markdown="1">       
<br/>
reset : 시간을 돌린다. Local 에서 사용 <br/>
revert : 강제로 해당 커밋을 삭제한다. 이력이 남는다. Remote 에서 사용
</div>
</details>

<details>
<summary>merge vs rebase</summary>
<div markdown="1">       
<br/>
merge : 쉽고 간단하지만, history가 지저분해진다.<br/>
rebase : 어렵고 conflict 날 확률이 높지만, history가 깔끔해진다.
</div>
</details>

<br/>

## 👥 참여

le2sky, jinsim, jihyehann, binimini

- 발표 & 대답 : 
- 질문 : 
- 서기 : 
