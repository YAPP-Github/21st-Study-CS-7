![image](https://user-images.githubusercontent.com/75151848/209769451-e047877c-2df9-4134-b975-b86fbd1df869.png)

## Web Server

- 웹 브라우저 클라이언트로부터 HTTP 요청을 받아 **정적인 컨텐츠(.html .jpeg .css 등)** 를 제공하는 컴퓨터 프로그램
- 동적 컨텐츠를 요청받으면, 클라이언트 요청을 WAS에 보내고 결과를 클라이언트에게 전달한다.
- ex) Apache, Nginx, IIS 등

### Apache

- 쓰레드 / 프로세스 기반 구조로 요청 하나당 쓰레드 하나가 처리하는 구조
- 사용자가 많으면 많은 쓰레드 생성, 메모리 및 CPU 낭비가 심함
- 하나의 쓰레드 : 하나의 클라이언트 구조

### Nginx

- 비동기 Event-Driven 기반 구조.
- 다수의 연결을 효과적으로 처리 가능.
- 대부분의 코어 모듈이 Apache보다 적은 리소스로 더 빠르게 동작 가능
- 더 작은 쓰레드로 클라이언트의 요청들을 처리 가능

<br>

## WAS

- Web Application Server 약자
- DB 조회나 다양한 로직 처리를 요구하는 **동적인 컨텐츠** 를 제공하기 위해 만들어진 Application Server
- **웹 컨테이너(Web Container)** 혹은 **서블릿 컨테이너(Servlet Container)** 라고도 불린다.
    - Container란 JSP, Servlet을 실행시킬 수 있는 소프트웨어를 말한다.
    - WAS는 JSP, Servlet 구동 환경을 제공한다.
- WAS = Web Server + Web Container
- 주요 기능
    - 프로그램 실행 환경과 DB 접속 기능 제공
    - 여러 개의 트랜잭션(논리적인 작업 단위) 관리 기능
    - 업무를 처리하는 비즈니스 로직 수행
- ex) Tomcat, JBoss, Jeus, Web Sphere 등

<br>

## 질문

<details>
<summary>Web server와 WAS를 분리하는 이유는?</summary>
<div markdown="1">
 
<br/>

자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성을 위해 Web Server와 WAS를 분리한다.
    
1. 기능을 분리하여 서버 부하 방지
    - WAS는 DB 조회나 다양한 로직을 처리하느라 바쁘기 때문에 단순한 정적 컨텐츠는 Web Server에서 빠르게 클라이언트에 제공하는 것이 좋다.
    - 만약 정적 컨텐츠 요청까지 WAS가 처리한다면 정적 데이터 처리로 인해 부하가 커지게 되고, 동적 컨텐츠의 처리가 지연됨에 따라 수행 속도가 느려진다.
2. 물리적으로 분리하여 보안 강화
    - SSL에 대한 암복호화 처리에 Web Server를 사용한다.
3. 여러 대의 WAS를 연결 가능
    - Load Balancing을 위해서 Web Server를 사용한다.
    - fail over(장애 극복), fail back 처리에 유리
    - 특히 대용량 웹 어플리케이션의 경우(여러 개의 서버 사용) Web Server와 WAS를 분리하여 무중단 운영을 위한 장애 극복에 쉽게 대응할 수 있다. 예를 들어, 앞단의 Web Server에서 오류가 발생한 WAS를 이용하지 못하도록 한 후 WAS를 재시작함으로써 사용자는 오류를 느끼지 못하고 이용할 수 있다.
4. 여러 웹 애플리케이션 서비스 가능
    - 예를 들어, 하나의 서버에서 PHP Application과 Java Application을 함께 사용하는 경우
5. 접근 허용 IP 관리, 2대 이상의 서버에서의 세션 관리 등도 Web Server에서 처리하면 효율적이다.
  
</div>
</details>  

<details>
<summary>http 요청을 받았을 때의 Tomcat, Spring MVC 동작 과정을 설명해주세요.</summary>
<div markdown="1">
 
<br/>

[참고](https://taes-k.github.io/2020/02/16/servlet-container-spring-container/)
    
![image](https://user-images.githubusercontent.com/75151848/209769536-12d2544b-81f0-491d-8b74-e6b296dcd5ed.png)

![image](https://user-images.githubusercontent.com/75151848/209769556-1a9dbc9c-3639-4460-a589-71ff4db2b495.png)


Server start 단계

1. Web server init
2. Root WebApplicationContext 로딩
3. Web server start

Client 호출 단계

1. Client -> Web server 으로 request 보냄
2. 동적 Web server -> Servlet container로 전달
3. Servlet container에서 쓰레드 생성
4. DispatcherServlet init (서블릿 생성 안되어 있을 경우)
5. 생성된 쓰레드에서 DispatcherServlet service() 메서드 호출
6. HandlerMapping을 통해 매핑 컨트롤러 조회
7. HandlerAdapter를 통해 매핑 컨트롤러에 request 전달
8. 개발자가 구현한 Controller -> Service -> Repository … 동작
  
</div>
</details>  

<details>
<summary>DispatcherServlet 동작 과정을 설명해주세요.</summary>
<div markdown="1">
 
<br/>

![image](https://user-images.githubusercontent.com/75151848/209769604-28f768fc-6808-4483-8857-7ee48f1e0eea.png)

    
DispatcherServlet은 front-controller을 함

1. HandlerMapping: 클라이언트 요청을 분석하여 매핑된 Controller가 있는지 확인하고
2. HandlerAdapter: 매핑 대상 Controller에게 Request 처리 요청을 보내고,
3. ViewResolver: Controller에서 view를 return 했을 경우, 해당하는 view를 찾아 client에게 return한다.
HttpMessageConverter: @ResponseBody가 있다면 Converter를 사용하여 응답 본문을 만들어 client에게 return한다.
  
</div>
</details>  
