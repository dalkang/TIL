## HTTP 프로토콜

* 서버-클라이언트 통신 시 stateless 방식으로 통신
  * 브라우저에서 새 웹페이지를 열면 기존의 웹페이지나 서블릿에 관한 어떤 연결 정보도 유지되지 않음
  * 페이지가 서로 독립적이어서 서로의 상태를 알 수 없는 것

<br>

## Cookie

* 서버 측에서 클라이언트 측에 상태 정보를 저장하고 추출하기 위한 매커니즘
* 서버에서 생성해서 클라이언트 측에 저장됨
* 서버에 요청할 때마다 쿠키의 속성값을 참조하거나 변경
* 크기는 4kb로 용량 제한
  * 256문자 이하의 text 데이터로만 저장
* 클라이언트에 저장되기 때문에 보안상 문제 발생
* 따라서 민감한 정보는 쿠키 내에 저장하지 않음

### Cookie 생성 밎 저장 과정

1. 서버에서 쿠키 생성

   * Cookie 클래스로부터 쿠키 객체 생성

   ```java
   Cookie cookie = new Cookie(이름, 값);
   ```

2. 속성 설정

   * setter로 쿠키 객체의 유효기간 설정

   ```java
   Cookie.setMaxAge(초단위_유효기간);
   ```

3. 클라이언트에 전송되어 클라이언트 PC에 저장

   * response 내장 객체의 addCookie() 메소드로 전송

   ```java
   response.addCookie(쿠키객체);
   ```

### Client/Server 쿠키 동작 방식

1. 클라이언트가 서버에게 페이지 요청
2. 서버에서 쿠키 생성 / HTTP 헤더에 쿠키를 포함시켜서 응답
   * 브라우저가 종료되어도 쿠키 만료 기간이 남아 있다면 클라이언트에서 보관
3. 다시 서버에 요청할 경우 HTTP 헤더에 쿠키 함께 보냄
   * 서버에서 쿠키를 읽어 이전 상태 정보를 변경할 필요가 있다면 쿠키 업데이트
   * 변경된 쿠키를 HTTP 헤더에 포함시켜 응답
4. 이후 요청받은 페이지 응답

### Cookie 관련 주요 메소드

* `setMaxAge()` : 쿠키 유효기간 설정

* `setValue()` : 쿠키의 값 설정

* `setVersion()` : 쿠키 버전 설정

* `setPath()` : 쿠키 사용의 유효 디렉토리 설정

  ```java
  protected void doHandle(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    request.setCharacterEncoding("utf-8");
    response.setContentType("text/html;charset=utf-8");
    
    PrintWriter out = response.getWriter();
  
    Date d = new Date();
    Cookie c = new Cookie("cookieTest", URLEncoder.encode("JSP 프로그래밍입니다", "utf-8"));
    c.setMaxAge(24 * 60 * 60);
    response.addCookie(c);
    out.println("현재시간 : " + d);
    out.println("<br> 문자열을 Cookie에 저장합니다.");
  }
  ```

* `getMaxAge()` : 쿠키 유효기간 반환

* `getValue()` : 쿠키의 값 반환

* `getVersion()` : 쿠키 버전 반환

* `getPath()` : 쿠키 사용의 유효 디렉토리 반환

  ```java
  protected void doHandle(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    response.setContentType("text/html;charset=utf-8");
    PrintWriter out = response.getWriter();
  
    Cookie[] allValues = request.getCookies();
  
    for (int i = 0; i < allValues.length; i++) {
      if (allValues[i].getName().equals("cookie")) {
        out.println("<h2>Cookie 값 가져오기 : </h2>" + URLDecoder.decode(allValues[i].getValue(), "utf-8"));
      }
    }
  }
  ```

<br>

## Session

* 클라이언트와 웹서버 가넹 네트워크로 연결이 지속적으로 유지되고 있는 상태
* 쿠키와 마찬가지로 서버와의 관계를 유지하기 위한 수단
* 쿠키와 달리 클라이언트에 저장되는 것이 아니라 서버 상에 객체로 존재
* 따라서 세션은 서버에서만 접근이 가능하며 보안이 좋음
* 서버에서 사용자의 정보를 유지/관리
* 사용자 인증 후 여러 페이지 걸쳐 정보를 공유할 수 있게 해줌
* 객체형을 포함한 어떠한 형태의 데이터 저장 가능 (저장할 수 있는 데이터 제한 없음)

* Session은 서버측에서만 설정 가능
* 브라우저당 한 개씩 생성

### Session 생성

1. 클라이언트가 서버에 페이지 요청

2. Session 자동 생성, 속성 설정

   * **`session 내장 객체`**의 메소드 사용

     ```java
     // session : 내장 객체, 생성하지 않고 사용
     session.setAttribute("SID", "name");
     ```

### Session ID

* 클라이언트가 처음 접속하면 서버(컨테이너)로부터 유일한 ID(Session ID)를 부여받음
* 클라이언트가 재접속했을 때 클라이언트를 구분하기 위한 수단으로 사용

### Session 관련 주요 메소드

* `setAttribute(이름, 값)` 

  * 세션 이름과 값 설정

* `getAttribute(이름)` 

  * 이름에 해당되는 값 반환

  ```java
  Object obj = session.getAttribute("SID");
  // Object 타입 반환 (사용시 변환)
  ```

* `getId()` 

  * 세션 ID 반환

* `invalidate()` 

  * 실행 중인 세션 종료, 모든 데이터 삭제

  ```java
  session.invalidate();
  ```

* `세션 무효화`

  * invalidate() 사용해 바로 무효시킬 수 있음

  * web.xml 파일에 session-timeout 설정해서 시간 정할 수 있음

  * setMaxInactiveInterval() 메소드를 사용해 시간 지정 가능

  * setMaxInactiveInterval(-1) 설정으로 session 객체 소멸 방지할 수 있음

  * 세션 유효 시간을 따로 설정하지 않으면 톰캣에서 설정한 기본 유효 시간 30분 적용

    * 기본 세션은 마지막 요청으로부터 30분 경과 후 자동 소멸됨

    * web.xml에 세션 유효 시간 설정되어 있음

      ```xml
      <session-config>
        <session-timeout>30</session-timeout>
      </session-config>
      ```

      

    