# HTML

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