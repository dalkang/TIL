# Servlet DB

* 비즈니스 로직 처리
* 웹 프로그램이 클라이언트의 요청에 대해 비즈니스 로직 처리 기능 이용
* 데이터 저장소에서 데이터 조회한 후 서블릿의 응답 기능을 이용해 클라이언트에게 전송

<br>

## VO(DTO)

> Servlet에서는 VO 사용

|                 | VO   | DTO  |
| --------------- | ---- | ---- |
| 데이터 저장     | O    | O    |
| Transfer 기능   | X    | O    |
| 데이터 가변성   | X    | O    |
| Setters/Getters | X    | O    |

> #### VO (Value Object)
>
> 데이터 저장 담당 클래스
>
> Transfer 기능 수행 X
>
> 값을 조회하기 위한 용도로 사용되는 객체
>
> Getters/Setters 사용하지 않음
>
> 값이 변하지 않는 불변성의 특징 가짐 (**read only**)
>
> #### DTO(Data Transfer Object)
>
> 데이터 저장 담당 클래스
>
> Transfer 기능 수행
>
> Controller, Service, View 등 계층 간에서 **데이터를 교환**하기 위해 사용되는 객체
>
> 즉, 비즈니스 로직을 갖지 않는 순수한 데이터 객체
>
> Getters/Setters 메소드 사용
>
> 데이터 변경 (가변의 성격) : Setters

## DAO

* **비즈니스 로직** 처리
* DB에 데이터 저장
* DB에서 데이터 조회

## Servlet

* Controller 역할
* 클라이언트 요청을 받아 DAO에게 전송
* DAO가 처리한 결과를 받아 클라이언트에게 전송

## Client

* 결과 값을 받아 웹 브라우저에 출력

<hr>

<br>

## DBCP (Database Connection Pool)

* 데이터베이스에 연결시켜 연결 상태를 유지

  > 클라이언트에서 다수의 요청이 발생될 경우 요청마다 DB Connection 객체를 생성하면 데이터베이스에 부하가 생김

  * 매번 커넥션을 생성하고 해제하는 데에 시간이 소요되지 않아 애플리케이션 실행 속도 빨라짐

* 일정량의 DB Connection 객체를 Pool에 저장

  * 생성되는 커넥션 수를 제어하기 때문에 동시 접속자 수가 증가해도 애플리케이션이 쉽게 다운되지 않음
  * 접속 시 사용할 커넥션이 없으면 대기 상태로 전환, 커넥션 반환되면 대기 순서대로 커넥션 제공

* 클라이언트 요청이 있을 때 사용하고 반환

  * 커넥션을 계속해서 사용하고 반환하기 때문에 재사용 가능
  * 많은 커넥션을 만들지 않아도 됨

## DBCP 연동

* Server의 context.xml에 `<Resource>` 정보 추가
* Tomcat 컨테이너가 데이터베이스 인증 작업 수행

## JNDI(Java Naming and Directory Interface)

* 디렉토리 서비스에 제공하는 데이터 또는 객체를 discover하고 loopup 하기 위한 API
* 실제 웹 애플리케이션에서 커넥션 풀 객체를 구현할 때 Java SE에서 제공하는 javax.sql.Datasourse