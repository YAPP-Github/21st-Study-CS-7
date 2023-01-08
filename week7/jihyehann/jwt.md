## JSON Web Token

- JSON 포맷을 이용하여 사용자에 대한 속성을 저장하는 Claim 기반의 Web Token
- 토큰 자체에 정보가 저장되어 있다. (Self-Contained)
- [RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519)

### 사용 용도

- Authorization (인가): 사용자가 로그인한 후, 모든 요청에 JWT를 포함하여 허용된 경로, 서비스, 리소스에 접근할 수 있다.
- 정보 교환: 안전하게 정보를 교환할 수 있다. 헤더와 페이로드를 가지고 signature를 계산하므로 데이터가 위변조되었는지 검증할 수 있다.

## 구조

```
header.payload.signiture
```

### Header

2가지로 구성: 토큰의 타입(typ), 서명 알고리즘(alg - RSA, HMAC SHA256 등)

Base64Url로 인코딩된다.

예시

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### Payload

Claim을 포함한다.

[Claim](https://www.rfc-editor.org/rfc/rfc7519#section-4.1) : 엔티티(주로 사용자)와 추가 데이터에 대한 설명

Claim의 3가지 타입: registered, public, private

**Registered Claim**

- **iss** (Issuer) : 토큰 발급자
- **sub** (Subject) : 토큰 제목 - 토큰에서 사용자에 대한 식별 값이 됨
- **aud** (Audience) : 토큰 대상자
- **exp** (Expiration Time) : 토큰 만료 시간
- **nbf** (Not Before) : 토큰 활성 날짜 (이 날짜 이전의 토큰은 활성화되지 않음을 보장)
- **iat** (Issued At) : 토큰 발급 시간
- **jti** (JWT Id) : JWT 토큰 식별자 (issuer가 여러 명일 때 이를 구분하기 위한 값)

Base64Url로 인코딩된다.

예시

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

### Signature(서명)

인코딩된 헤더, 인코딩된 페이로드, secret을 가지고 헤더에서 지정한 알고리즘으로 암호화하여 서명한다.

HMAC SHA256 알고리즘으로 암호화한 예시

```
HMACSHA256(
	base64UrlEncode(header) + "." +
	base64UrlEncode(payload),
	secret
)
```

<br>


## 장단점

### 장점

- JSON 포맷으로 생성된 토큰은 용량이 작아(XML과 비교할 때) 전송 속도가 빠르다.
- 별도의 인증 저장소를 필요로 하지 않는다. (self-contatined)
- 중앙 집중식 인증 서버와 데이터베이스에 의존하지 않는 쉬운 인가 방법을 제공한다. → 확장성
- 다른 로그인 시스템에 접근 및 권한 공유가 가능하다.
- 모바일 환경에서도 동작한다. (세션은 불가능)

### 단점

- 단일 키를 사용하므로, 키가 유출되면 시스템 전체가 위험에 노출된다.
- payload에 저장하는 정보가 많아질수록 토큰의 길이가 늘어나 네트워크에 부하를 줄 수 있다.
- payload 자체는 암호화하지 않으므로, 중요 데이터는 넣을 수 없다.
- 토큰이 한 번 만들어지면 삭제가 불가능하다. 토큰 자체를 탈취당하면 대처하기가 어렵다. (만료 시간을 꼭 넣어야한다)

<br>

## 질문

<details>
<summary>인증(Authentication)과 인가(Authorization)의 차이를 설명해주세요.</summary>
<div markdown="1">
 
인증(Authentication)

- 어떤 개체(사용자 또는 장치)의 신원을 확인하는 것
    
인가(Authorization)
- 어떤 리소스에 접근할 수 있는지, 어떤 동작을 수행할 수 있는지 검증하는 것
- 접근 권한을 얻는 일

<br/>
  
</div>
</details>

<details>
<summary>JWT를 이용한 토큰 인증 과정을 설명해주세요.</summary>
<div markdown="1">
 
<br/>

![image](https://user-images.githubusercontent.com/75151848/209769393-4f82ffa3-3de2-4d83-adbe-7953a40edf09.png)
    
1. 사용자가 ID, Password로 로그인 요청을 한다.
2. 서버에서 계정 정보를 읽어 사용자를 확인한 후, 사용자의 고유 ID 값을 부여한 후 기타 정보와 함께 Payload 에 집어넣습니다.
3. JWT 토큰의 유효기간을 설정합니다.
4. 암호화할 Secret key 를 이용해 Access Token 을 발급한다.
5. 사용자는 Access Token 을 받아 저장 후, 인증이 필요한 요청마다 토큰을 헤더에 실어 보낸다.
6. 서버에서는 해당 토큰의 Signature 를 Secret key 로 복호화한 후, 조작 여부, 유효기간을 확인한다.
7. 검증이 완료되었을 경우, Payload 를 디코딩하여 사용자의 ID 에 맞는 데이터를 가져온다.
    
</div>
</details>  
    