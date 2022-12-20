## HTTP (Hyper Text Transfer Protocol)

- `서버/클라이언트` 모델을 따라 데이터를 주고 받기 위한 프로토콜.
- 인터넷에서 하이퍼텍스트를 교환하기 위한 통신 규약으로, `80번` 포트를 사용한다.
- 1989년 팀 버너스 리(Tim Berners Lee)에 의해 처음 설계되었다.
- 암호화되지 않은 평문 데이터를 전송하는 프로토콜로, 보안 문제가 발생할 수 있다.

### 구조

- HTTP는 애플리케이션 레벨의 프로토콜로 `TCP/IP` 위에서 작동한다.
- HTTP는 상태를 가지고 있지 않는 Stateless 프로토콜이며 `Method`, `Path`, `Version`, `Headers`, `Body` 등으로 구성된다.

![https://user-images.githubusercontent.com/75151848/190839781-aaf20685-35de-4e61-90b8-476420f3188b.png](https://user-images.githubusercontent.com/75151848/190839781-aaf20685-35de-4e61-90b8-476420f3188b.png)

### Connectionless (비연결성)

- 클라이언트가 서버에 요청을 하고 응답을 받으면 바로 TCP/IP 연결을 끊어 연결을 유지 하지 않는다.
- 서버의 자원을 효율적으로 관리하고, 수많은 클라이언트의 요청에도 대응할 수 있다.
- 연결이 끊어짐에 따라 새로 연결될 때 TCP/IP 연결을 새로 맺어야 하므로 3-way handshake에 따른 시간이 추가된다.
    
    → HTTP/1.1 부터는 persistent connection 을 제공한다. HTTP/1.1 에서는 모든 요청/응답이 Connection을 재사용하도록 설계되어 있으며, 필요없는 경우에만 TCP 연결을 종료하는 방식으로 변경되었다.
    

### Stateless (무상태성)

- 서버가 클라이언트의 이전 상태를 보존하지 않는다.
- stateless는 상태를 보관하지 않으므로 클라이언트의 요청에 어느 서버가 응답해도 상관없다. → 클라이언트의 요청이 대폭 증가하는 경우, 서버를 증설해 해결할 수 있다.

## HTTPS (Hyper Text Transfer Protocol Secure)

- HTTP에 데이터 `암호화`가 추가된 프로토콜.
- `443번` 포트를 사용하며, 네트워크 상에서 중간에 제3자가 정보를 볼 수 없도록 암호화를 지원한다.
- `SSL/TLS` 프로토콜을 통해 데이터를 암호화한다.

### SSL (Secure Scokets Layer)

- 암호화 기반 인터넷 보안 프로토콜
- 전달되는 모든 데이터를 암호화하고 특정한 유형의 사이버 공격도 차단한다.
- SSL은 클라이언트와 서버간에 **핸드셰이크**를 통해 인증이 이루어진다.
- SSL은 `TLS`(Transport Layer Security) 암호화의 전신. TLS는 SSL의 업데이트 버전이며 명칭만 다르다고 볼 수 있다.