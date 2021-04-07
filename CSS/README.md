# CSS TIL

## Selectors

> Selector 중첩 사용 가능

* Universal : `*`
* Type : `tag`
* ID : `#id`
* Class : `.class`
* State : `:`
* Attribute : `[]`



## Basic Structure

```css
selector {
  property: value;
}
```



## Color

> 색 참조하기 좋은 곳 [Color tool](https://material.io/resources/color/#!/?view.left=0&view.right=0)



## Length units

### Relative length units

* `vw` : viewport 너비의 1%
* `vh` : viewport 높이의 1%

> `%` 는 부모 요소의 길이를 기준으로 값을 정하는 백분율 단위
>
> 즉, `vw` 의 부모 요소는 viewport이고 100vw일 경우 viewport 너비 전체를 쓴다는 뜻



## Flexbox

> main axis와 cross axis를 정하고 이를 기준으로 생각해서 지정

### Container attribute

* `display`

  * flex

* `flex-direction`

  > 중심축 정하는 속성

  * row (**default**) : left ➔ right
  * row-reverse : right ➔ left
  * column : top ➔ bottom
  * column-reverse : bottom ➔ top

* `align-items`

  > 반대축 아이템 속성

  * baseline : baseline을 기준으로 텍스트를 균등하게 표시

* `flex-wrap`

  * nowrap (**default**)
  * wrap
    * left ➔ right
    * top ➔ bottom
    * 아이템들이 한 줄에 꽉 차게 되면 다음 줄로 넘어감
  * wrap-reverse
    * left ➔ right
    * bottom ➔ top

* `flex-flow`

  * `flex-direction` 과 `flex-wrap` 을 한번에 작성

  * ```css
    flex-flow: column nowrap;
    ```

* `justify-content`

  > 중심축 아이템 배치 속성

  * flex-start (default)
  * flex-end
  * center : 가운데 정렬
  * space-around : 박스 주변으로 공간 생성해 간격 주기
  * space-evenly : 똑같은 간격 주기
  * space-between : 시작과 마지막 아이템은 양쪽 끝에 붙이고 나머지 아이템들 사이에 일정한 간격 주기

* `align-content`

  > 반대축 아이템 배치 속성

  * `justify-content` 의 속성 모두 사용 가능

### Item attribute

* `order`
  * 아이템별로 순서 지정
  * 0 (default)
* `flex-grow`
  * 아이템의 크기를 늘어난 화면 크기에 맞추는 속성
  * 0 (default)
* `flex-shrink`
  * 아이템 크기를 줄어든 화면 크기에 맞추는 속성
  * 0 (default)
    * 숫자가 클수록 그만큼 더 줄어듦
* `flex-basis`
  * 아이템들이 공간을 얼만큼 차지해야 하는지 정해주는 속성
  * `flex-grow` 와 `flex-shrink` 의 효과를 한번에 누릴 수 있음
  * auto (default)
    * `%` 로 지정
* `align-self`
  * 아이템별로 아이템 정렬하게 해주는 속성