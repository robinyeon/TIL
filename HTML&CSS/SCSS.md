*From [CSS Layout by nomadcoders](https://nomadcoders.co/css-layout-masterclass)*

# SCSS
- 기본적으로 css preprocessor: compile돼서 normal css 코드가 되는 것.
- css를 programming language처럼 만들어주는 등, css 사용을 편리하게 해줌 e.g. 콘솔을 통해 에러 알림
- Sass가 먼저 만들어졌으나 후에 Sass를 위한 공식 문법으로 릴리즈 됨: 따라서 보통 사람들이 말하는 sass와 scss는 거의 같은 것을(아예 같은 것 일수도) 의미한다.
- 점점 업계의 표준이 되어가고 있다.

<br/>

## gulp
- 브라우저는 scss를 이해하지 못한다. 따라서: **styles.scss** -gulp-> **styles.css** -link href-> **index.html**
- 오래된 언어에서 신문법으로, scss를 css로, 혹은 이미지를 새롭게 압축하는 등의 업무 자동화에 유용

<br/>

## `_`로 시작하는 폴더
- e.g. `_varaibles.scss` or `_reset.scss`
- scss만을 위한 파일
- normal css로 컴파일 되지 않았으면 하는 내용들을 넣는 폴더

<br/>

## Variable
- `_variables.scss`폴더 내에 `$bg: #e7473c;`와 같이 지정
- `styels.scss`로 들어와서
  1. 임포트 한 뒤: `@import "_variables;";`
  2. `body { background-color: $bg; }`와 같이 변수를 사용하면 된다.
  3. 그러면 `styles.css`내에서 자동으로 변환되는 것이 눈에 보인다. (gulp덕)

<br/>

## Nesting
- father-child 간의 css를 적을때 조금 더 쉽고 명확하게 적을 수 있게 해준다.
```
<h2>HI</h2>
<div class="box">
  <h2>Title</h2>
  <button>Button</button>
</div>
<button>BYE</button>
```
- 기존에는 `.box`를 통해 내부 h2와 button에 공통으로 적용 시킬 속성을 적고, 내부 h2에만 적용할 속성을 또 `.box h2 {}`와 같이 적었다면,
- Nesting을 통해 아래와 같이 적을 수 있다:
```
.box {
  margin-top: 20px;
  h2 {
    color: blue;
   }
   button {
    color: red;
   }
}
```
- Nest안에 nest를 만들기도 가능하다. 또한 아래와 같이 `&`을 활용하면 반복해서 적지 않아도 된다.
  ```
  button {
    &:hover {
    }
  }
  ```

<br/>

## Mixins
- 인자로 받는것이 무엇이냐에 따라 css의 결과값이 바뀐다. 따라서 반복해서 비슷한 다른 css를 작성할 때 유용하다. (후에 나오는 extends와의 차이점)
- `_mixins.scss`내에 아래와 같이 작성:
  ```
  @mixin nameOfTheMixin {
    color: blue;
    font-size: 30px;
    margin-bottom: 12px;
  }
  ```
- `styles.scss`에 `_mixins.scss`임포트 한 후 적용하고 싶은 element에 아래와 같이 작성한다:
  ```
  h1 {
    @include nameOfTheMixin();
  }
  ```
- 앞서 `_varaibles`에서 만든 변수를 활용하고 싶다면
  ```
  @mixin nameOfTheMixin($color) {
     color: $color;
    font-size: 30px;
    margin-bottom: 12px;
  }
  ```
  ```
  h1 {
    @include nameOfTheMixin(yellow);
  }
  ```
- if-else 구문 활용도 가능하다.
  ```
  @mixin nameOfTheMixin($word) {
    @if $word == "odd" {
      color: blue;
    } @else {
      color: red;
    }
  }
  ```
- 반응형을 위해 media query 등과 함께 사용하면 더욱 유용
- 'awesome scss mixins'과 같이 검색하면 더 유용한 정보 찾을 수 있다. e.g. libraries such as Bourbon, Sass MediaQuery.

<br/>

## Extend
- 같은 코드를 중복해서 쓰고 싶지 않을 때 사용한다.
- 다른 코드를 확장하거나 재사용할 때 사용
- extend를 사용할 수 있는 scss를 만드는 방법: `%` 활용
  1. `_button.scss`에 아래와 같이 작성
    ```
    %button {
      border-radius: 7px;
      font-size: 12px;
      text-transform: uppercase;
      padding: 5px 10px;
    }
    ```
  2. `styles.scss`에 `_button.scss`임포트 한 후, 아래와 같이 적용시킨다:
    ```
    a {
      @extend %button;
      text-decoration: none;
    }
    ```






