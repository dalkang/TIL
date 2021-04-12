# EL (Expression Language)

* 표현 언어
* 자바 코드가 들어가는 표현식을 좀 더 편리하게 사용하기 위해 JSP2.0부터 도입된 데이터 출력 기능
* 표현식 도는 액션 태그 대신 값을 표현
  * `<%= 값 %>` ➔ `${값}`
* `attribute` 또는 `parameter` 등을 JSP 파일에서 출력할 용도로 사용
* `attribute` : ${attribute 이름}
* `parameter` : ${param.이름} 또는 ${paramValue[인덱스]}

<br>

## EL 연산자

* 산술연산자 : +, -, *, /, %, (div, mod)
* 관계 연산자 : >, >=, ... (gt, ge, lt, eq, ne)
* 논리 연산자 : &&, ||, !, (and, or, not)
* 조건 연산자: 수식 ? 참일 때 값 : 거짓일 때 값
* empty 연산자 : 값이 null이거나 길이가 0이면 true
  * ${empty 변수} : 변수가 null이거나 0이면 true
* 문자열 연산 불가

<br>

## EL 내장 객체

* `scope`

  > scope 우선순위 높은 순서대로 작성해둠
  >
  > * 각 내장 객체에서 바인딩하는 속성 이름이 같은 경우,
  >
  >   각 내장 객체에서 지정된 출력 우선순위에 따라 순서대로 속성에 접근

  * `pageScope` : 현재 페이지 영역의 변수
  * `requestScope` : 이전 페이지에서 전달된 영역의 변수
  * `sessionScope` : 세션 영역의 변수
  * `applicationScope` : 애플리케이션 영역의 변수 

* 요청 파라미터

  * `param`
  * `pramaValues`

* 초기 파라미터

  * initParam : web.xml에 context 초기화 파라미터 참조

* `Cookies` : 쿠키값 참조

* `pageContext` : 페이지 정보 나타내는 컨텍스트 페이지 참조