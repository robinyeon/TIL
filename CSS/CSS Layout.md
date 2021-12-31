***WIP***

## block

<br/>

## inline
- in line: 같은 줄에 있음
- not a box
- fluid element
- width 없음, height 없음

<br/>

## inline-block
- block 속성 지녀서 width와 height 먹이기 가능
- inline 속성 지녀서 나란히 옆에 존재
- 많이 사용 안함: 구식이기도 하고 문제가 많음
  - problem(1) 박스 사이에 의도하지 않는 공간이 자동으로 생김
  - problem(2) 특정한 폼을 가지고 있지 않음
  - problem(3) 반응형을 고려하지 못함

<br/>

## [flexbox](https://flexboxfroggy.com/#ko)

### '직속'부모에게 `display: flex` 먹이기
- 정작 해당 '자식'element에는 아무것도 먹이지 않음

<br/>

### flex-direction
- 기본값은 `flex-direction: row`(가로)(수평)
  - main axis는 가로
  - cross axis는 세로
- `flex-direction: column`
  - main axis는 세로
  - cross axis는 가로
- `flex-direction: row-reverse`
- `flex-direction: column-reverse`

<br/>

### justify-content
- **justify-content를 사용하면 'main axis'(wherever it is)에서 아이템을 움직일 수 있다.**
### align-items
- **align-items을 사용하면 'cross axis'(wherever it is)에서 아이템을 움직일 수 있다.**
- 자주 하는 실수: **직속부모에게 height 주는 것을 잊지 말기**

<br/>

### '자식'의 스타일에 property 먹이는 흔치 않은 경우
#### 1. align-self
  - align-items(cross axis)와 같은 역할을 하지만 **오직 한 '자식'** 에게만 먹이는 경우
  - e.g. `.child:nth-child(2) { align-self: center; };`
#### 2. order
  - '자식'의 순서를 바꾸고 싶을때
  - HTML을 건드리기 곤란한 경우에 사용
  - e.g. `.child:nth-child(2) { order: 1 };`      
     ![order: 1](https://user-images.githubusercontent.com/85475577/147569431-7a66fbb9-7317-4d2d-a459-1bf1d5a45a3a.png)
  - 위의 그림처럼 나온 이유: order의 디폴트값은 0이기 때문에 1을 먹인 두번째 박스가 가장 끝으로 가게 됨
#### 3. flex-shrink
  - `flex-wrap: wrap`일때는 한 줄에 모든걸 유지하려고 설정된 width가 무시됨(밑의 flex-wrap파트 참고)
  - 이 상황에서 어떤 박스가 더 꾸겨질지를 결정할 수 있음.
  - 디폴트 값은 `flex-shrink: 1`
  - `flex-shrink: 2` 2배로 더 꾸겨짐      
    ![flex-shrink: 2](https://user-images.githubusercontent.com/85475577/147571231-2c4e8c98-ff0d-4ae4-8def-767d30e1d3e6.png)
#### 4. flex-grow
  - flex-shrink와 비슷한데, flex-grow의 경우에는 얼마나 더 늘어날지를 결정할 수 있음.
  - 여분의 공간이 생기는 경우 flex-grow가 그 공간을 차지함
  - 디폴트 값은 `flex-grow: 0`     
      ![스크린샷 2021-12-28 오후 10 38 58](https://user-images.githubusercontent.com/85475577/147572035-6f5e5f3b-b042-4fc8-8670-5c7b85cc7bad.png)
  - n빵하는 느낌 e.g. 
      ```
      .child:nth-child(2) { 
      flex-grow: 2
      };
      
      .child:nth-child(3) { 
      flex-grow: 1
      };
      
      /* 2번째 child는 빈공간의 2/3을 차지하고,
         3번째 child는 빈공간의 1/3을 차지하게 된다.*/
      ```
*flex-shrink와 flex-grow는 반응형 디자인을 할 때 매우 유용하다.*

#### 5. flex-basis
  - element에게 찌그러지거나 늘어나기 전의 **main axis 방향**의 초기 사이즈(initial size)를 지정해줌.
    - `flex-direction: row`일때는 width가 되고
    - `flex-direction: column`일때는 height가 되겠지
  - flex-shrink나 flex-grow에 의해 후에는 변경될 수도 있는 값

<br/>

### flex-wrap
#### 1. 기본값 `flex-wrap: nowrap`
  - 설정된 width를 바꾸는 한이 있더라도 모든걸 한줄에 꾸겨넣으려함
  - width는 신경 안쓰고, 한줄에 꾸겨넣는걸 더 신경씀
#### 2. `flex-wrap: wrap`
  - 설정된 width를 유지. 다음줄에 넘기는 경우도 존재
#### 3. `flex-wrap: wrap-reverse`
  ![flex-wrap: wrap-reverse](https://user-images.githubusercontent.com/85475577/147570313-b74076ab-641a-487d-b34b-280a847dd74c.png)
  
<br/>

### flex-flow      
- flex-direction과 flex-wrap는 자주 같이 쓰임. 이를 위한 지름길 `flex-flow: flex-direction flex-wrap;`          
![flex-flow](https://user-images.githubusercontent.com/85475577/147574659-9dea08cc-93d9-4bc8-a0b7-d5911d40da0d.png)       
![flex-wrap과 flex-direction](https://user-images.githubusercontent.com/85475577/147574482-7b792499-afd4-446a-9a4f-28de48b06d9b.png)     

<br/>

### align-content
![align-content](https://user-images.githubusercontent.com/85475577/147570615-5156a5d6-ffc7-4875-a69e-568d055f6792.png)
- align-content를 사용해서 저 사이의 공간을 수정할 수 있다.
- cross axis에 있는 'line(빈공간)'을 컨트롤 할 수 있다.
- align-content는 **여러 줄들 사이의 간격**을 지정하며, align-items는 컨테이너 안에서 어떻게 모든 요소들이 정렬하는지를 지정
- 한 줄만 있는 경우, align-content는 효과를 보이지 않음

<br/>

<hr/>

***WIP***

## grid
- 격자무늬(grid)를 만들기 힘든 flexbox에서 일어나는 문제를 해결하기 위해 등장      
  - flexbox 문제점 예시      
    ![flexbox의 문제점_예시](https://user-images.githubusercontent.com/85475577/147576422-f5114acd-65c9-4c95-9ef4-14a0a0edee37.png) 

- grid 역시 대부분 '부모'에게 적용시킨다.
```
.father {
  display: grid;
}
```

### columns & rows 
- e.g. `grid-template-columns: 10px 20px 30px;`
  - column 3개를 만들건데, 첫번째 column의 너비는 10px, 두번째는 20px, 세번째는 30px
- e.g. `grid-template-rows: 100px 200px 300px;`
  -  row 3개를 만들건데, 첫번째 row 높이는 10px, 두번째는 20px, 세번째는 30px
- auto 통해서 화면 꽉 채울 수 있음
  - e.g. `grid-template-columns: auto 200px;`


### gap
- `column-gap`: column들 사이의 공간 크기
- `row-gap`: row들 사이의 공간 크기
- `gap`: column과 row에 공통적으로 적용할 공간 크기


### repeat
- e.g. `grid-template-columns: 200px 200px 200px 200px;`
  - === `grid-template-columns: repeat(4, 200px)`
-  e.g. `grid-template-rows: 100px repeat(2, 300px) 100px;` 이렇게도 활용 가능


### grid-template-areas
- allows visually design your layout
```
  grid-template-areas: 
    "header header header header"
    "content content content nav"
    "content content content nav"
    "footer footer footer footer";
```
  <img width="402" alt="스크린샷 2021-12-31 오후 9 10 30" src="https://user-images.githubusercontent.com/85475577/147822666-1e607f54-1bf2-459b-abeb-9461baa5ea38.png">

- 이름 적는 대신 `.` 적어 넣으면 해당 영역은 비워진다.
  <img width="398" alt="스크린샷 2021-12-31 오후 9 14 09" src="https://user-images.githubusercontent.com/85475577/147822795-bd18d418-eee4-4ea3-880c-9426e26f4adc.png">


#### gride-area
각각(header, content 등)이 무엇을 지칭하는지는 class를 통해서가 아닌 지정된 `grid-area`로 판별한다.
```
.header {
  grid-area: header;
}
```

### start & end
[grid-template-areas와 비슷한 기능을 하는 속성, 자식 grid에 명시한다.]
1)grid-column-start
2)grid-column-end
3)grid-row-start
4)grid-row-end
해당 속성은 정수인 숫자가 들어가며, 1부터 column(row)의 최대갯수 + 1까지 사용 가능하다.
범위를 초과하게 되면 css가 망가져서 생각한 대로 동작하지 않게 되니 꼭 명심하자.. 참고로 상대단위(%, auto)등은 안 먹히는 듯 하다.

이렇게 start, end를 하나하나 다 명시해서 쓰는건 귀찮기도 하기 때문에 다음 강의에 이를 해결하기위한 방법인 shortCuts이 나온다.

#### Shortcuts
- grid-column: (start) / (end);
- grid-row: (start) / (end);
`grid-column: 1 / 4;`

- -1, -2, -3, ··· ▷ 끝에서부터 line 세기
`grid-column: 1 / -2;`

- (start) / span (cell 수) ▷ 시작점과 끝점을 적는걸 대신한다.
`grid-row: 2 / span 2;`
예시 4개의 기둥 있고 한줄로 다 먹을 때 `grid-column: span 4;`

### Line Naming
선 너비 선 너비 ... 너비 선
`grid-template-columns: [first-line] 100px [second-line] 100px [third-line] 100px [fourth-line] 100px [fifth-line];`
```
.grid {
  display: grid;
  grid-template-columns: [first-line] 100px [second-line] 100px [third-line] 100px [fourth-line] 100px [fifth-line];
  grid-template-rows: repeat(4, [sexy-line] 100px);
  gap: 10px;
}
```
- grid-template-areas, grid-column(row), span, line-naming 상황맞춰 골라쓰면 되는겨

### fr
fr은 전역으로 결정되는 것이 아닌 grid container에서 결정된다(여기서는 .grid)
따라서 fr을 쓰기 위해서는 grid container에 height을 명시해야 한다.

fr-fraction(부분)
fraction은 grid에서 사용 가능한 공간을 뜻한다.
이는 더이상 px등의 절대단위를 사용하지 않게끔 해주는 좋은 단위이다.
fr값 비율로 공간을 나눈다.

항상 block은 width와 height 주어져 있지 않은 경우에는 width는 가능한 한 최댓값, height은 0이다.
=> fr을 쓰려면 grid container에 height만 써도 동작하지만, width만 쓰면 동작하지 않는 이유가 이것이다.

● grid-template:
"(이름)" (row크기)
"(이름)" (row크기)
"(이름)" (row크기)/ (각 column의 크기);

grid-templete 에서는 repeat이 적용되지 않는다.
```
.grid {
  height: 50vh;
  display: grid;
  gap: 10px;
  grid-template:
    "header header header header" 1fr
    "content content content nav" 2fr
    "footer footer footer footer" 1fr /
    1fr 1fr 1fr 1fr;
}
```
















