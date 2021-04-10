# JSP(Java Server Page)

* HTML 문서 내에 자바코드를 삽입해서 웹 서버에서 동적으로 웹페이지 생성해 클라이언트(웹 브라우저)에게 반환해주는 서버 사이드 스크립트 언어
* JSP를 통해 HTML과 동적으로 생성된 콘텐츠를 혼합해 사용 가능
* Servlet을 보완한 스크립트 방식 표준(Servlet 기능 + 추가 기능)
* JSP는 실행되면서 Servlet으로 변환되어 .class 파일로 만들어 실행
* View를 담당하는 페이지로 사용

## JSP와 Servlet의 차이점

### `JSP`

* HTML 내부에 Java 소스코드가 들어 있는 형식

### `Servlet`

* Java 코드 내에 HTML 코드가 있는 형식

## JSP Tags

* `<%` 로 시작 `%>`로 종료
* @, !, -, -- 를 추가하여 태그 의미 부여

| 구분        | 태그 표기   | 설명                           |
| ----------- | ----------- | ------------------------------ |
| 지시어      | <%@ %>      | JSP 페이지의 속성 지정         |
| 선언부      | <%! %>      | 변수 선언 및 메소드 정의       |
| 표현식      | <%= %>      | 계산식, 함수 호출 결과, 변수값 |
| 스크립트 릿 | <% %>       | 자바 코드 기술                 |
| 주석        | <%--%>      | JSP 페이지 주석                |
| 액션 태그   | \<jsp:액션> |                                |

## JSP Page 기본 구성 요소

* #### JSP Page 내용

  * HTML 문서 내용
  * JSP 태그
  * 자바 코드

* #### JSP Page 구성

  * 지시어 
    * page
    * include
    * taglib
  * 스크립트 요소 : 선언문, 표현식, 스크립트릿
  * 액션 태그

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

## JSP 내장 객체

* 클라이언트에서 웹서버에 JSP 페이지 요청하면 자동으로 생성
* 객체를 별도로 생성하는 과정 없이 바로 사용 가능

### 내장 객체 종류

#### `입출력`

* request
* response
* out

#### `서블릿`

* page
* config

#### `컨텍스트`

* session
* application
* pageContext

#### `예외처리`

* exception