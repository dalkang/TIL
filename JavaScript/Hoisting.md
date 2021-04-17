# Hoisting

## ES5의 실행 

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

    이를 호이스팅이라고 함

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

      이로 인해 호이스팅 불가

  * 변수가 값을 가질 때 비로소 값을 가진 변수로 인식



