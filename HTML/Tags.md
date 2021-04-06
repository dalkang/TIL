# HTML

## Tags

> 유효한 태그인지 확인하고 싶을 땐 [Click me](https://validator.w3.org/)

* `Tag`는 `Opening tag`와 `Closing tag`로 이루어져 있음

  * ex)
  * `Opening tag` : `<h1>`
  * `Closing tag` : `</h1>`

* `Opening tag`와 `Closing tag` 안에는 `Content` 영역

  * ex)

  * ```html
    <h1>
      <!-- Content -->
      Hello, World!
    </h1>
    ```

* `Opening tag` `Closing tag` `Content` 를 Element 혹은 Node 라고 부름



### Content sectioning

> **구획 요소를 사용해 문서의 콘텐츠를 논리적인 구조로 체계화**

* `<header>` : 탐색에 도움을 주는 콘텐츠 표시 (제목, 로고, 검색 등의 요소 포함)
* `<nav>` : 웹페이지 내에서 다른 페이지로 이동할 수 있는 링크를 모아놓은 구획
* `<aside>` : 문서의 주요 내용과 간접적으로만 연관된 부분, 사이드바로 표현
* `<section>` : HTML 문서의 독립적인 구획
* `<article>` : 독립적으로 구분해 배포하거나 재사용할 수 있는 구획
* `<main>` : body의 주요 콘텐츠 표시
* `<footer>` : 구획의 작성자, 저작권 정보, 관련 문서 등의 내용 표시



### Forms

> 웹페이지에서 사용자로부터 입력을 받아 데이터를 전송할 수 있는 양식

* `<form>` : 모든 HTML 폼은 `<form></form>` 태그로 시작, 정보 제출을 위한 문서 구획 나타내는 태그
* `<button>` : 클릭 가능한 버튼
* `<input>` : 웹페이지에서 사용자의 데이터를 받을 수 있는 인터랙티브 컨트롤 생성
  * 한 페이지 내에 많은 `<input>` 태그가 있을 수 있기에 주로 `id` 를 부여해서 사용
  * `type` 을 통해 input의 양식 지정
* `<label>` : 사용자 인터페이스 항목 설명
* `<select>` : 옵션 메뉴를 제공하는 양식
* `<option>` : 옵션 항목을 정의
* `<textarea>` : 일반 텍스트 양식

