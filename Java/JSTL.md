# JSTL

> JSP Standard Tag Library

* JSP와 HTML을 같이 사용함으로써 가독성이 떨어지는 것을 보완하고자 만들어진 태그 라이브러리

* JSP 페이지에서 자바 코드를 작성하지 않고 태그 사용

* JSP 페이지의 로직을 담당하는 부분인 제어문 및 DB 처리 등을 표준 라이브러리 태그로 제공

* 사용하기 위한 태그

  ```jsp
  <%@ taglib uri="" prefix="">
  ```

<br>

## JSTL Library

> 5개의 라이브러리로 구성

* core
  * `c`
  * 변수, 제어문, url 등 처리
* format 
  * `fmt` 
  * 숫자/날짜/시간 지정, 국제어, 다국어 처리
* database 
  * `sql`
  * 데이터베이스 작업 처리
* xml 
  * `x`
  * XML 문서 처리
* function 
  * `fn`
  * 함수 기능

<br>

## Core

* `URI` : http://java.sun.com/jsp/jstl/core
* `Prefix` : **c**
* 제공 기능
  * 변수의 선언 및 삭제 등 변수와 관련된 작업
  * if, for문 등 제어문 처리
  * url 처리 및 기타 예외처리, 화면 출력 기능

### Core 태그 리스트

* 변수 지원
  * `<c:set>` : 변수 설정
  * `<c:remove>` : 설정된 변수 제거

* 흐름 제어

  * `<c:if>` : 조건문

    ```jsp
    <c:if test="${조건식}" var="변수명" [scope} />
    ```

  * `<c:choose>` : 선택 ( 자바 switch문)

    * 서브 태그
      * `<when>`
      * `<otherwise>`

    ```jsp
    <c:choose>
    	<c:when test="조건식1">본문내용1</c:when>
    	<c:when test="조건식21">본문내용2</c:when>
    	<c:otherwise>본문내</c:otherwise>
    </c:choose>
    ```

  * `<c:forEach>` : 반복문

    ```jsp
    <c:forEach var="변수명" items="반복할 객체면" begin="시작값' end="마지막값" step="증가값">
    </c:forEach>
    ```

  * `<c:forTokens>` : 구분자로 분리된 각 토큰을 처리할 때 사용

- URL 처리

  >  ${pageContext.request.contextPath }를 기본으로 설정

  - `<c:import>` : url을 이용해서 다른 자원 추가
  - `<c:redirect>` : request.sendRedirect() 기능 수행
  - `<c:url>` : 요청 매개변수로부터 URL 생성

- 기타 태그
	- `<c:out>` : JspWriter에 내용을 처리한 한 후 출력
	- `<c:catch>` : 예외 처리

<br>

## Formatting Tag / Library

> 숫자 및 날짜 관련된 formatting tag / library

* `fmt`
  * `<fmt:formatNumber>`
  * `<fmt:formatDate>`

<br>

## 문자열 처리 함수

* JSTL에서 제공하는 함수 이용해 JSP에서 사용 가능
* `<fn:toLower()>`

