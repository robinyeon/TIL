*From [Momentum by nomadcoders](https://nomadcoders.co/javascript-for-beginners)*

## Form submission
- input의 유효성 검사를 작동하기 위해선 **form안에 존재**해야만 한다. e.g. `required`, `maxlength`
- input안에 있는 button을 누르거나, type이 submit인 input을 클릭하면 내가 작성한 form이 submit 된다.
  - 즉, form안에서 엔터나 클릭을 누르고 input이 더 존재하지 않는다면 자동으로 submit 되는 것 -> HTML의 규칙
  - 더 이상 'click'이 아닌 'submit'이라는 이벤트에 집중할 필요가 있다.

## Events
- 브라우저는 엔터를 누를때 새로고침을 하고 form을 submit하도록 프로그램 되어있다(디폴트).
  - EventListener 함수의 첫번째 argument는 항상 **지금 막 벌어진 일들에 대한 정보**가 된다.
  - `event.preventDefault()`: 이벤트의 디폴트 행동을 멈춘다.
  - e.g. `a`태그의 새로운 링크로 이동하는 행동을 막기, submit 될 시 페이지 새로고침 되는 것 막기

## Saving Username_Local Storage
- `localStorage.setItem()`
- `localStorage.getItem()`
- `localStorage.removeItem()`
  - `()`안에는 (key, value)

## [D-day 계산기](https://aspdotnet.tistory.com/2396)
- `new Date("2022-03-12T00:00:00")`과 같은 형태로 디데이 날짜를 설정해줄 수 있다.
- `Date()`간의 뺄셈은 자동으로 `.getTime()`을 활용한 밀리세컨드 뺄셈값이 출력된다.
