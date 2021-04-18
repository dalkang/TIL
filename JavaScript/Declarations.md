# Declarations

## let

* **블록 스코프**를 갖는 변수

  > 블록 스코프의 기준
  >
  > 1. 중괄호 {코드}
  > 2. function name() {코드}
  > 3. if (조건식) {코드}

  * ```javascript
    let word = "test";
    if (word) {
        let word = "JavaScript";
      	console.log("inner : ", word);
        }
    console.log("outer : ", word);
    
    /*
    inner : JavaScript
    outer : test
    */
    ```

  * 블록 스코프 내부에서 `let` 을 사용한다면 같은 이름 사용 가능

    * 같은 스코프 내에서 같은 이름 사용 불가

  * 만약 `if문` 스코프 내부에서 `let`을 사용하지 않고 `word`에 값을 할당했다면, 블록 안에서 바깥의 변수를 찾아 값을 변경했을 것

  * 그러나 블록 바깥에서 내부의 변수에 접근할 수는 없음

* 값을 할당하지 않고 변수만 선언 가능

  * 초기값으로 `undefined` 할당

* 콤마로 구분하여 변수 생성 가능

<br>

## var / let

### function

```html
<ul class="fruits">
  <li>apple</li>
  <li>banana</li>
  <li>peach</li>
</ul>
```

```javascript
// var
const node = document.querySelector(".fruits");
for (var k = 0; k < node.children.length; k++) {
  node.children[k].onclick = function(event) {
    event.target.style.backgroundColor = "orange";
    console.log(k);
  }
}

/*
3
3
3
*/
```

* var는 함수 스코프 내부에서 유효
* 어떤 \<li> 태그를 클릭하더라도 `for문` 이 끝났을 때의 k 값인 3 출력

```javascript
// let
const node = document.querySelector(".fruits");
for (let k = 0; k < node.children.length; k++) {
  node.children[k].onclick = function(event) {
    event.target.style.backgroundColor = "orange";
    console.log(k);
  }
}

/*
0
1
2
*/
```

* let은 블록 스코프 내부에서 유효
* 이벤트를 설정할 때의 k 값 출력

### Global Object

> 전역 객체는 전역 범위에 항상 존재하는 객체
>
> 브라우저에서의 전역 객체는 `window`
>
> 따라서 어느 전역 객체나 함수는 `window` 객체의 프로퍼티로 접근 가능
>
> `this` 는 `window` 객체 참조
>
> **글로벌 오브젝트에서 let 변수로 this로 참조 불가**

```javascript
var candy = "Lollipop";
let flower = "Wallflower";
console.log(this.candy, this.flower);
/*
Lollipop, undefined
*/
```

* `candy` 는 `window` 객체에 설정
  * `window` 객체 내부에 값이 할당되어 `this` 를 통해 조회 가능
  * 모든 js 파일에서 변수 공유
* `flower` 는 `window` 객체에 설정되지 않음
  * `window` 객체에 설정되지 않아 `this` 로 조회하는 건 불가능
  * 블록 내부에서 `let` 이 설정되지 않은 경우 **자바스크립트 엔진이 블록을 만들어 스코프로 사용 (\<script>에 설정)**
  * 모든 js 파일에서 변수 공유

> `Global Object`에서 `var`와 `let`은 저장되는 영역이 다름
>
> 모든 js 파일에서 변수를 공유하는 점에서는 같음

```javascript
var globalVar = "var";
let globalLet = "let";
{
  let globalBlock = "block";
}
```

* Global Object에서 작성할 때
  * `var` : `window` 에 설정, 공유
  * `let` : `script` 에 설정, 공유
  * `{let}` : `block` 에 설정, 공유 불가
* 함수에서 작성할 때
  * `var` : `Local` 에 설정
  * `let` : `Local` 에 설정
  * `{let}` : `block` 에 설정

> 설정되는 위치에 따라 접근 권한이 달라짐

<br>

<hr>

## Hoisting

### var

```javascript
console.log(cup);
var cup;

// undefined
```

* 변수를 선언하기도 전에 미리 불러서 console.log()를 실행하더라도 식별자를 해결

* 그러나 해당 위치에서의 `cup` 의 **값**은 `undefined`

  * 코드를 실행하지 않았기 때문

  * 아래서 선언된 걸 위에서 끌어다 쓰는 것

  * Window 객체에 값을 갖고 있는 변수로 인식됨

    ↳ 이를 호이스팅이라고 함

<br>

### let

```javascript
console.log(tea)
let tea;

// Uncaught error
```

* let은 호이스팅 불가

### let 인식 시점

* 첫 번째 줄 : 변수로 인식되지 않음, error

* 두 번째 줄 : Window 객체에 변수로 표시됨

  * 값을 할당하지 않고 변수만 선언하면 엔진이 `undefined` 할당

    * **값을 갖고 있지 않다는 표시를 하는 것과 같음**

      ↳ 이로 인해 호이스팅 불가

  * 변수가 값을 가질 때 비로소 값을 가진 변수로 인식

<br>

<hr>

## const

* **값을 바꿀 수 없는 변수 선언 (상수)**
  * `Object`나 `Array`일 경우 값 변경 가능
    * `const` 변수 전체는 불가능
    * 그러나 `Object`의 프로퍼티 값, `Array`의 엘리먼트 값은 가능
* 반드시 값을 작성
* 변수 선언만 불가
* JavaScript에서 상수는 대문자 사용 관례

* `let` 보다는 `const` 를 먼저 고려해보는 것이 좋음 