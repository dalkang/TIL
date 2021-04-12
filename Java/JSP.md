# JSP(Java Server Page)

* HTML 문서 내에 자바코드를 삽입해서 웹 서버에서 동적으로 웹페이지 생성해 클라이언트(웹 브라우저)에게 반환해주는 서버 사이드 스크립트 언어
* JSP를 통해 HTML과 동적으로 생성된 콘텐츠를 혼합해 사용 가능
* Servlet을 보완한 스크립트 방식 표준(Servlet 기능 + 추가 기능)
* JSP는 실행되면서 Servlet으로 변환되어 .class 파일로 만들어 실행
* View를 담당하는 페이지로 사용

<br>

## JSP와 Servlet의 차이점

### `JSP`

* HTML 내부에 Java 소스코드가 들어 있는 형식

### `Servlet`

* Java 코드 내에 HTML 코드가 있는 형식

<br>

## JSP 발전 과정

* HTML 태그를 중심으로 자바 이용해 화면 구현
* 화면 요구 사항이 복잡해지면서 자바 코드로 대체하는 액션 태그 등장
* 복잡한 자바 코드를 제거하는 방향으로 발전
  * 복잡한 자바 코드로 인해 화면 작업 어려움
* **현재 JSP 페이지 스크립트 요소보다 EL이나 JSTL 사용해 화면 구현**

<br>

## JSP Tags

* `<%` 로 시작 `%>`로 종료
* `@`, `!`, `-`, `--` 를 추가하여 태그 의미 부여

| 구분        | 태그 표기   | 설명                                  |
| ----------- | ----------- | ------------------------------------- |
| 지시어      | <%@ %>      | JSP 페이지의 속성 지정                |
| 선언부      | <%! %>      | 변수 선언 및 메소드 정의              |
| 표현식      | <%= %>      | 계산식, 함수 호출 결과, 변수값        |
| 스크립트 릿 | <% %>       | 자바 코드 기술                        |
| 주석        | <%--%>      | JSP 페이지 주석                       |
| 액션 태그   | \<jsp:액션> | 자바 빈, include / forward / param 등 |

<br>

## JSP Page 기본 구성 요소

* #### JSP Page 내용

  * HTML 문서 내용
  * JSP 태그
  * 자바 코드

* #### JSP Page 구성

  * 지시어 
    * `page`
    * `include`
    * `taglib`
  * 스크립트 요소
    * 선언문
    * 표현식
    * 스크립트릿
  * 액션 태그

<br>

## 지시어

### `page`

* JSP 페이지의 전체적인 속성을 지정할 때 사용

* JSP 컨테이너에게 전달하는 JSP 페이지 관련 페이지

* <%@ 지시어 속성1=값, 속성2=값, ... %>

  * JSP 페이지 속성

    ```jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    ```

### `include` 

* ```jsp
  <%@ include file="포함될 파일의 url"%>
  ```

* 공통적으로 포함될 내용을 가진 파일을 해당 JSP 페이지 내에 삽입하는 기능

<br>

## JSP 내장 객체

* 클라이언트에서 웹서버에 JSP 페이지 요청하면 자동으로 생성
* 객체를 별도로 생성하는 과정 없이 바로 사용 가능

### 내장 객체 종류

#### `입출력`

* `request` : 사용자의 요청을 받을 때 사용하는 객체
  * `getParameter()` : 1개의 값을 받을 때
  * `getParameterValues()` : 동일한 name으로 여러 개의 값을 받을 때 (배열로 받음)
  * `getParameterNames()` : name 속성 모를 때 사용
* `response` : 서버로부터 사용자에게 응답할 때 사용하는 객체
  * JSP 처리 결과를 웹 브라우저에게 전송
  * `addCookie()` : 쿠키 설정
  * `setContentType()` : 페이지의 contentType 설정
  * `sendRedirect()` : 페이지 이동 (페이지 포워딩) 
* `out` : 웹 서버에서 웹 브라우저에게 출력 스트림으로 응답할 때 사용

#### `서블릿`

* page
* config

#### `컨텍스트`

* session
* application
* pageContext

#### `예외처리`

* exception

<br>

## Action Tag

* JSP 페이지 내에서 어떤 동작을 지시하는 태그
* 어떤 동작(액션)이 일어나는 시점에 페이지와 페이지 사이에서 이동 기능 제공
* 다른 페이지의 실행 결과를 현재 페이지에 포함하는 기능 제공

### Action Tag 종류

* #### `include`

  * ##### `<jsp:include>`

  *  다른 페이지의 실행 결과를 현재 페이지에 포함시킬 때 사용

  * | 구분        | \<jsp:include>                  | \<include>                                           |
    | ----------- | ------------------------------- | ---------------------------------------------------- |
    | 형식        | \<jsp:include page=""/>         | <%@ include file="" %>                               |
    | 처리 시점   | 실행 시                         | 자바 소스로 변환 시                                  |
    | 기능        | 별도의 파일로 처리              | 현재 파일에 삽입                                     |
    |             | 제어권이 이동했다가 다시 돌아옴 | 합쳐서 하나의 java 파일 생성                         |
    | 데이터 구성 | 동적 데이터로 구성              | 정적 데이터로 구성                                   |
    | 용도        | 화면 레이아웃 모듈화 할 때      | 여러 페이지에서 사용하는 변수를 지정하고 포함시킬 때 |

* #### `forward`

  * ##### `<jsp:forward>`

  * 현재 페이지에서 다른 특정 페이지로 전환, 웹페이지 간 제어 이동할 때 사용

    * `<jsp:param>`

      * `forward` 및 `include` 액션 태그에서 데이터 전달하기 위해 사용

      * name과 value로 구성 

        ```jsp
        <jsp:param name="candy" value="lollipop" />
        ```

      * param으로 전달된 값 받을 때

        * ```jsp
          request.getParameter("candy");
          ```

* #### `useBean`

  * ##### `<jsp:useBean>` 

  * 자바빈을 JSP 페이지에서 사용할 때 

* #### `setProperty`

  * ##### `<jsp:setProperty>`

  * 프로퍼티 값을 설정할 때 사용

* #### `getProperty`

  * ##### `<jsp:getProperty>`

  * 프로퍼티의 값을 가져올 때 사용

* #### `plug-in`

<br>

## JavaBeans

* 데이터를 다루기 위해 자바로 작성되는 소프트웨어 컴포넌트로 재사용 가능
* DTO / VO와 같은 개념
* 입력폼의 데이터와 데이터베이스의 데이터 처리 부분에서 활용
* 클래스로 만들어짐
  * 멤버 필드로 속성(property)이 있고, 멤버 메소드로 Getters/Setters 포함
    * `setXXX()` : 프로퍼티에 값 저장
    * `getXXX()` : 프로퍼티 값 반환
* Action Tag를 이용해 빈 사용
* 속성 접근 제어자 : private (Getters/Setters 사용)

### 자바 빈 액션 태그

* useBean 액션 태그 : \<jsp:useBean>

  * 자바 빈을 JSP 페이지에서 사용할 때

    ```jsp
    <jsp:useBean id="자바빈 이름"  class="패키명을 포함한 클래스명" scope="" />
    ```

* setProperty 액션 태그 : \<jsp:setProperty>

  * 프로퍼티의 값을 설정할 때 사용 (데이터 설정)

* getProperty 액션 태그 : <jsp:getProperty>

  * 프로퍼티의 값을 가져올 때 사용