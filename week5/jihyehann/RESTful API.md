# REST API

## REST 란

- REpresentational State Transfer
- 2000년 로이 필딩 (Roy T. Fielding)의 박사 논문에서 소개
- 분산 하이퍼미디어 시스템(ex.웹)을 위한 아키텍처 스타일
- `REST API` : REST 아키텍처 스타일에 부합하는 API

<br/>

## REST 제약조건

1. Client-Server
2. Stateless 
3. Cacheable 
4. Uniform Interface  
5. Layered System
6. Code-On-Demand (optional)

HTTP를 잘 따르기만 하면 위의 제약조건들은 대부분 만족한다.

잘 지켜지지 않는 것은 `Uniform Interface` 제약조건이다.

<br/>

## Uniform Interface

URL로 지정된 리소스에 대한 조작을 통일하고 한정된 인터페이스로 수행하는 아키텍쳐 스타일

1. 자원의 식별 (identifiaction of resources) : URI를 통해 자원을 식별할 수 있어야 한다.
2. 표현을 통한 자원에 대한 조작 (manupulation of resources through representations)
3. 자기 서술적인 메세지 (self-descriptive messages) : 메세지 스스로 설명 가능해야 한다.
4. HATEOAS (hypermedia as the engine of application state) : 하이퍼미디어를 통해 애플리케이션의 상태가 전이되어야 한다.