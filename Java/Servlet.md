# Servlet

> Server + Applet

* Java 언어로 이루어진 웹 프로그래밍 문서 (.java)
* 서버 쪽에서 실행되면서 클라이언트의 요청에 따라 동적으로 서비스를 제공하는 자바 클래스
* Java의 일반적인 특징 모두 포함
* 독자적으로는 실행할 수 없고 웹컨테이너(톰캣)에서 실행
* 클라이언트 요청에 동적으로 작동(동적 웹 애플리케이션 컴포넌트)
* 멀티 스레드 기반
* MVC 패턴에서 로직에 해당하는 Model(DAO/DTO)과 화면에 결과를 표시하는 View 사이에서 제어를 하는 **Controller**로 사용
* Java 파일이기 때문에 Java Resources의 src에 위치
* 같은 Servlet class에 대한 요청을 처리하는 모든 thread는 같은 Servlet 객체 공유
  * 동시성 문제 발생
  * 동시성 문제를 해결하기 위해 지역 변수 사용
  * 지역 변수는 각 요청 스레드마다 각각의 스택 영역에 저장되기 때문에 동시성 문제를 발생시키지 않음



## Servlet Life Cycle

1. Servlet 객체 생성 (처음 한번)
2. 초기화 : init() 메소드 호출 (처음 한번)
3. 요청 작업 처리 : service() 메소드가 호출되고 이후 request에 따라 doGet(), doPost(), doPut(), doHead(), doDelete(), doTrace() 등등 요청할 때마다 매번 수행 (재호출되는 메소드)
   * 서블릿은 디폴트로 get 방식으로 요청
     * 따라서 Form 태그에서 method 지정하지 않아도 디폴트로 get 방식 사용
     * 클래스에 doGet() 메소드가 없으면 오류 발생
       * destroy() 메소드 호출하며 서블릿 소멸
4. 자원 해제 : destroy() 메소드 호출 (마지막 한번)



## Servlet 장점

* 빠른 응답 속도 
  * 서블릿은 최초 요청시 객체가 만들어져 메모리에 로드되고 이후 요청 시에는 기존의 객체 재활용
  * 그 결과 동작 속도가 빠름
* 신뢰성 (Reliablity)
* 확장성 (Scalability)
  * 기능 확장 용이
* 플랫폼과 서버에 독립적 (Java 기반)
* 한번 개발된 애플리케이션은 다양한 서버 환경에서 실행 가능
* Java에서 제공되는 다른 기술 같이 사용 가능 (ex: Servlet + JDBC)



## Servlet Class

### Servlet Package

* javax.servlet.* , 서블릿 작성을 위한 인터페이스와 클래스 제공

* javax.servlet.http.* : HTTP 프로토콜을 이용한 서블릿 작성에 필요한 인터페이스 제공 (GET / POST)

### Servlet class

* Servlet 인터페이스
* GenericServlet 추상 클래스
* HTTPServlet 클래스
  * 서블릿을 만들 때 HTTPServer 클래스 상속받아서 만듦



### Servlet mapping

>  웹 브라우저에서 서블릿 요청 위해서는 서블릿 매핑 필요

1. Xml 기반

* web.xml 파일에 설정
* `<servlet-name>` : 임의의 이름 설정
* `<servlet-class>` : 맵핑할 클래스 파일명 (패키지명 포함)
* `<url-pattern>` : url에 사용할 임의의 이름 입력(/로 시작)

* 서블릿 경로 연결

  ```xml
  <servlet>
  	<servlet-name>Servlet 이름</servlet-name>
    <servlet-class>사용할 서블릿 위치와 이름</servlet-class>
  </servlet>
  ```

* 서블릿 맵핑

* 서블릿 파일 경로 노출로 인한 보안 문제 없애고 url 간단하게 줄일 수 있음

  ```xml
  <servlet-mapping>
    <servlet-name>Servlet 이름</servlet-name>
    <url-pattern>사용할 url 이름</url-pattern>
  </servlet-mapping>
  ```

2. 어노테이션 기반

* 이클립스에서 자동 지정 (변경 가능)
* 클래스가 아닌 서블릿으로 생성
* **@WebServlet** 이용
* 어노테이션이 적용되는 클래스는 반드시 HttpServlet 클래스를 상속 받아야 함



### 요청 / 응답 객체

`doGet(HttpServletRequest request, HttpServletResponse response)`

* 요청 처리 객체 - `HttpServletRequest request`
  * 클라이언트의 요청 정보를 전달 받는 객체
  * 클라이언트에서 입력한 데이터가 `request` 객체에 담겨져서 서버로 전송
  * 클라이언트 >> 서버
* 응답 처리 객체 - `HttpServletResponse response`
  * 요청 처리 결과를 클라이언트에게 전달할 때 사용하는 객체
  * 서버 측에서 처리한 결과를 `response` 객체에 담아서 클라이언트로 전달
  * 서버 >> 클라이언트



### Path

| 프로토콜 | IP         | 포트번호 | 프로젝트명 | 패키지명 + 파일명 |
| -------- | ---------- | -------- | ---------- | ----------------- |
| http     | localhost: | 8081     | Servlet    | third             |

#### 1. URL : 전체 주소

* http://localhost:8081/Servlet/third

#### 2. URI : ContextPath + ServletPath

* /Servlet/third

#### 3. ContextPath : 프로젝트명

* /Servlet

#### 4. ServletPath : 패키지명 + 파일명

* /third



### Context Path

* WAS에서 웹 애플리케이션을 구분하기 위한 path
  * 1개의 웹 어플리케이션 : 자바 프로젝트 1개
* 이클립스에서 프로젝트 생성하면 자동으로 server.xml에 추가
  * `<Context docBase=*"Servlet"* path=*"/Servlet"* ...>`



## 요청(request)

### Form 태그로 서블릿에 요청

* form 예시

  > Login form

  * ```html
    <form name="loginForm" method="get" action="login">
    ```

    * `method` : 데이터 가져오는 방식 지정
    * `action` : 사용할 서블릿의 이름 지정



### request 객체 사용

* request 객체의 getParameter() 메소드를 사용해 전달되는 값 받아옴
* request.getParameter("태그의 name 속성값");
* 서버로 전달되는 데이터 유형은 `String`



### `<form>` 태그로 전송된 데이터를 받아오는 메소드

1. `getParameter()` : 1개의 값 받을 때 사용

2. `getParameterValues()` : 동일한 name으로 여러 개의 값 받아올 때 사용 (결과 - 배열)

   * String[] 변수 = request.getParameterValues()

3. `request.getparameterNames()` : 이름 모를 때 사용

   > Enumberation 객체 생성해 사용



## 응답(response)

* 클라이언트로부터 온 요청 처리 메소드 
  * doGet() 또는 doPost() 메소드 안에서 처리
* `HttpServletResponse` 객체 이용
* `setContentType()` 메소드 이용해 클라이언트에게 전송되는 데이터 유형 지정
  * **MIME-TYPE**
  * HTML로 전송 타입 : text/html
  * 일반 텍스트로 전송 타입 : text/plain
  * XML 전송 타입 : application/xml
* 클라이언트(웹 브라우저)와 서블릿의 통신은 자바 I/O 스트림 이용
  * PrintWriter의 print() 또는 println() 메소드를 이용해 데이터 출력
  * HTML 태그 문자열을 웹 브라우저로 출력



### Get / Post

* `Get`

  * 서버로 데이터 전송 시 URL 뒤에 name=value 형태로 전송
  * 여러 개의 값을 전송할 때는 &으로 구분해서 전송
  * 데이터가 노출되어 보안에 취약
  * 전송할 수 있는 데이터는 최대 255자
  * doGet() 이용해 데이터 처리
  * 간단하고 처리 방식이 POST 보다는 빠름

* `Post`

  * 데이터를 TCP/IP 프로토콜 데이터의 HEAD 영역에 숨겨 전송
  * 노출이 없어 보안 유리
  * 전송되는 데이터 용량 무제한
  * GET보다 느림
  * doPost() 이용

* `Get` 과 `Post` 방식 둘 다 처리하는 방법

  * 새로운 메소드를 만들어 doPost()와 doGet()을 호출하게 함

  ```java
  doGet() {
    doProcess();
  }
  
  doPost() {
    doProcess();
  }
  
  doProcess() {
  // 처리할 내용
  }
  ```