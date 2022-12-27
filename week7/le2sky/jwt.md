# jwt

## JWT와 JWT 기반 인증

- `JWT(JSON Web Token)`란 인증에 필요한 정보들을 암호화시킨 JSON 토큰을 의미

- JWT 기반 인증은 JWT 토큰(Access Token)을 HTTP 헤더에 실어 서버가 클라이언트를 식별하는 방식이다

## 구조

JWT는 .을 구분자로 세 문자열의 조합이다.

- `Header` : JWT에서 사용할 타입, 해시 알고리즘 종류
- `Payload` : 서버에서 첨부한 사용자 권한 정보, 데이터
- `Signature` : Header, Payload를 Base64 URL-safe Encode 를 한 이후 Header 에 명시된 해시함수를 적용하고, 개인키(Private Key)로 서명한 전자서명이 담겨있다.

## Refresh Token과 Sliding Sessions를 활용한 JWT의 보안 전략

### 1. AccessToken

**짧은 만료 시간의 경우**

- 장점 : 기기나 AccessToken이 탈취되더라도 빠르게 만료
- 단점 : 자주 로그인해야함

**긴 만료 시간의 경우**

- 장점 : 로그인 자주할 필요가 없음
- 단점 : 기기, AccessToken이 탈취되면 오랫동안 사용가능

---

### 2. Sliding Sessions 전략과 함께 AccessToken 사용

세션을 지속적으로 이용하는 유저에게 자동으로 만료 기한을 늘려주는 방법이다. 주로 유효한 AccessToken을 가진 클라이언트의 요청에 대해 서버가 새로운 AccessToken을 발급해주는 방법을 사용한다.

글 작성을 시작할 때 발급해준다거나, 쇼핑몰에서 장바구니에 아이템을 담는 경우에 발급해주는 등의 전략을 사용하는 것도 괜찮은 방법이다.
또한, 클라이언트가 토큰의 iat(토큰 발급 시간)속성을 참조해서 갱신 요청을 하는 방법도 있다.

장점 :

- 로그인을 자주할 필요가 없음
- 글을 작성하거나 결제를 하는 등의 세션 유지가 필요한 순간에 세션이 만료되는 문제를 방지

단점 :

- 접속이 주로 단발성으로 이루어지는 서비스의 경우 Sliding Sessions 전략의 효과가 크지 않음

- 긴 만료 시간을 갖는 AccessToken을 사용하는 경우 로그인을 전혀 하지 않아도 되는 경우가 발생

---

### 3. AccessToken과 RefreshToken

AccessToken과 함께 그에 비해 긴 만료 시간을 갖는 RefreshToken을 클라이언트에 함께 발급한다. 주로 AccessToken은 30분 내외, RefreshToken은 2주에서 한달 정도의 만료 기간을 부여한다.
(access 만료시 refresh로 재발급, 만료된 RefreshToken으로 요청이 들어오면 오류를 반환해, 사용자에게 로그인을 요구)

장점 :

- 짧은 만료 기간을 사용 할 수 있기 때문에 AccessToken이 탈취되더라도 제한된 기간만 접근이 가능
- 로그인 자주할 필요가 없음
- RefreshToken에 대한 만료를 강제로 설정 가능

단점 :

- 클라이언트는 AccessToken의 만료에 대한 연장 요청을 구현
- 인증 만료 기간의 자동 연장이 불가능
- 서버에 별도의 storage 를 만들어야함

---

### 4. Sliding Sessions 전략과 함께 AccessToken과 RefreshToken을 사용

AccessToken의 Sliding Sessions 전략이 AccessToken 자체의 만료 기간을 늘려주었다면, 이 전략은 RefreshToken의 만료 기간을 늘려준다.

장점 :

- RefreshToken의 만료 기간에 대한 제약을 받지 않는다.
- 글을 작성하거나 결제를 하는 등의 세션 유지가 필요한 순간에 세션이 만료되는 문제를 방지 할 수 있다.

단점 :

- 서버에서 강제로 refreshToken을 만료하지 않는 한 지속적으로 사용 가능
- 인증이 추가로 요구되는 경우에 대한 보안 강화가 필요

---
