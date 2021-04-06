# HTML

> [참조 : MDN HTML](https://developer.mozilla.org/en-US/docs/Web/HTML)

* 웹 브라우저에서 보여지도록 디자인된 문서
* 브라우저에서 실행 가능한 가장 기본적인 파일
* Markup Language로 태그들을 이용해 구조적으로 짜여짐



## Hypertext Markup Language

* **`Hypertext`** : 참조(링크)를 통해 한 문서에서 다른 문서로 접근할 수 있는 텍스트
* **`Markup Language`** : 일반적인 텍스트와 문법적으로 구분하기 위해 태그를 이용해 문서나 데이터의 구조를 명기하는 언어



## Basic Structure

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width">
  </head>
  <body>
    <!-- Contents -->
  </body>
</html>
```

* `<!DOCTYPE html>`

  > HTML5 이후부터 사용되는 선언

  * 문서의 유형을 정의하기 위해 사용하는 선언문 (**DTD**, Document Type Definition)
  * 선언된 페이지의 HTML 버전이 무엇인지 웹 브라우저에게 알려주는 역할

* `<html>` 

  * HTML 문서의 최상위 루트
  * 웹 페이지의 시작과 끝을 나타내는 태그

* `<head>` 

  * html 내부에서 사용할 외부 파일 연결, 웹 페이지에 대한 상세 문서 정보 보유
    * `<meta>` : 웹 페이지에 대한 메타데이터를 제공
    * `<title>` : 웹 페이지의 제목 정의(브라우저 검색하거나 북마크에 추가하거나 보여지는 제목)

* `<body>` 

  * 사용자에게 제공하는 화면 내용
  * 한 문서에 하나의 `<body>` 요소만 존재



## HTML Tags

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
* `<nav>` : 웹 페이지 내에서 다른 페이지로 이동할 수 있는 링크를 모아놓은 구획
* `<aside>` : 문서의 주요 내용과 간접적으로만 연관된 부분, 사이드바로 표현
* `<section>` : HTML 문서의 독립적인 구획
* `<article>` : 독립적으로 구분해 배포하거나 재사용할 수 있는 구획
* `<main>` : body의 주요 콘텐츠 표시
* `<footer>` : 구획의 작성자, 저작권 정보, 관련 문서 등의 내용 표시