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

## [grid](https://cssgridgarden.com/#ko)
- 격자무늬(grid)를 만들기 힘든 flexbox에서 일어나는 문제를 해결하기 위해 등장      
  - flexbox 문제점 예시      
    ![flexbox의 문제점_예시](https://user-images.githubusercontent.com/85475577/147576422-f5114acd-65c9-4c95-9ef4-14a0a0edee37.png) 

- grid 역시 대부분 '부모'에게 적용시킨다.
```
.father {
  display: grid;
}
```

<br/>

### columns & rows 
- e.g. `grid-template-columns: 10px 20px 30px;`
  - column 3개를 만들건데, 첫번째 column의 너비는 10px, 두번째는 20px, 세번째는 30px
- e.g. `grid-template-rows: 100px 200px 300px;`
  -  row 3개를 만들건데, 첫번째 row 높이는 10px, 두번째는 20px, 세번째는 30px
- **auto** 통해서 화면 꽉 채울 수 있음
  - e.g. `grid-template-columns: auto 200px;`: 가장 끝의 column만 너비 200px이고 나머지 앞쪽의 너비는 auto.

<br/>

### gap
- `column-gap`: column들 사이의 공간 크기
- `row-gap`: row들 사이의 공간 크기
- `gap`: column과 row에 공통적으로 적용할 공간 크기

<br/>

### repeat
- e.g. `grid-template-columns: 200px 200px 200px 200px;`
  - === `grid-template-columns: repeat(4, 200px)`
-  e.g. `grid-template-rows: 100px repeat(2, 300px) 100px;` 이렇게도 활용 가능

<br/>

### grid-template-areas
- allows **visually** design layout
- grid-area(하단 참고) 통해서 각 child에 이름 적용 후 써먹기
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


#### grid-area
- 각각(header, content 등)이 무엇을 지칭하는지는 class를 통해서가 아닌 지정된 `grid-area`로 판별한다.
```
.header {
  grid-area: header;
}
```
- grid-column와 grid-row 모두를 입력하는게 너무 많은 경우, 다른 속성을 이용하여 줄일 수 있다.
- grid-area은 /(슬래쉬)로 구분지어 grid-row-start, grid-column-start, grid-row-end, grid-column-end순으로 입력
  - e.g. `grid-area: 1 / 1 / 3 / 6;`

<br/>

### start & end
- child에 부여한다.
- 예를 들어, `grid-column-start: 3;`와 같이 입력시 그리드 세번째 세로**선**에서 시작
  1) grid-column-start
  2) grid-column-end
  3) grid-row-start
  4) grid-row-end
- 해당 속성은 정수인 숫자가 들어가며, 1부터 column(row)의 최대갯수 + 1까지 사용 가능하다.(왜냐하면 '선'이니까)

#### Shortcuts
- 위처럼 start, end를 하나하나 다 명시해서 쓰는건 귀찮기도 하기 때문에 다음 강의에 이를 해결하기위한 방법인 shortCuts이 나옴.
- grid-column: (start) / (end);
- grid-row: (start) / (end);
`grid-column: 1 / 4;`
- -1, -2, -3, ··· : 끝에서부터 line 세기 e.g. `grid-column: 1 / -2;`
- span: 차지하는 구역의 개수를 지정할 수 있다.
  - e.g. 4개의 기둥 있고 한줄로 다 먹을 때 `grid-column: span 4;`
  - `grid-row: (start) / span (cell 수)` : start로부터 지정한 span의 너비만큼 차지한다. e.g. `grid-row: 2 / span 2;`

<br/>

### Line Naming
- 각각의 선에 이름 부여 가능
- 선부터 시작해야지 기존의 계산법과 같이 사용하기 유용: **선** **너비** 선 너비 ... 너비 선
  - e.g. `grid-template-columns: [first-line] 100px [second-line] 100px [third-line] 100px [fourth-line] 100px [fifth-line];`
```
.grid {
  display: grid;
  grid-template-columns: [first-line] 100px [second-line] 100px [third-line] 100px [fourth-line] 100px [fifth-line];
  grid-template-rows: repeat(4, [sexy-line] 100px);
  gap: 10px;
}
```
- grid-template-areas, grid-column(row), span, line-naming 필요에 따라 골라 쓰면 됨

<br/>

### fr
- fr(fraction): fraction은 grid에서 사용 가능한 공간을 뜻한다.
- 더 이상 px 등의 절대단위를 사용하지 않게끔 해주는 좋은 단위
- **비율**로 공간을 나눈다.
- fr은 전역으로 결정되는 것이 아닌 grid container에서 결정된다. **따라서 fr을 쓰기 위해서는 grid container에 height을 명시해야 한다**.
  - block은 width와 height 주어져 있지 않은 경우, 항상 width는 가능한 한 최댓값, height은 0이다.

<br/>

### grid-template:
```
"(이름)" (row크기)
"(이름)" (row크기)
"(이름)" (row크기)/ (각 column의 크기);
```
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
- grid-templete 에서는 repeat이 적용되지 않는다.

<br/>

### Place-items
- `items`는 **각각의 아이템들(child)에 공통**으로 적용할때 사용한다
  - justify-items 수평 (flexbox의 justify-contents와 동일)      
  - align-items 수직 (flexbox의 align-items와 동일)     
  - place-items: (수직) (수평); // shortcut 동시에 2개 다 지정가능
- stretch : grid를 늘려서 grid를 채우게 한다. (default) // 대부분 stretch 사용          
- start : item을 cell 시작에 배치한다. (flexbox의 flex-start와 동일)    
- center : item을 cell 중앙에 배치한다. (flexbox의 center와 동일)     
- end : item을 cell 끝에 배치한다. (flexbox의 flex-end와 동일)     

<br/>

### Place-content
- `content`는 **전체 그리드 박스**를 위치시키는데에 사용된다.
  - justify-content: 수평으로 그리드가 놓이는 위치
  - align-content: 수직으로 그리드를 움직이는 것
  - place-content: place items와 마찬가지로 place-content를 통해 수직 수평으로 그리드 이동가능
- 컨테이너의 height가 그리드를 담을 만큼 충분해야한다.(높이 지정)
- **place-items는 각각의 child셀안에서 항목이 이동하는 것이며, place-content는 그리드가 이동하는 것**

<br/>

### Place-self
- child에게 직접 명시하여 부여한다.
- **하나의 child에만** 적용돠는 property
  - justify-self
  - align-self
  - place-self

<br/>

### Auto columns and rows
- 예상한 column, row보다 더 많은 데이터를 가져오게 되는경우
e.g.      
```
grid-template-columns: repeat(4, 100px);
grid-template-rows: repeat(4, 100px);
```     
![auto-rows auto-columns가 필요한 경우](https://user-images.githubusercontent.com/85475577/147895007-051d601e-ccbe-413f-b949-30c6b0d2a85f.png)      

- `grid-auto-rows: (원하는 row의 높이);`: 만들어놓은 row보다 더 많은 content가 있으면, 자동으로 row를 만들어라.
- `grid-auto-flow: (방향);`
  - default는 row
  - flex-direction과 비슷
  - row가 끝날 때 새로운 row를 만들지, 새로운 column을 만들지 결정
- `grid-auto-columns: (원하는 column의 너비);`: `grid-auto-flow: column;`일때 작동한다.

<br/>

### minmax
- column과 row의 최소최대값을 지정할 수 있다.
  - e.g. `grid-template-columns: repeat(10, minmax(100px, 1fr));`: 최대 1fr로 하되 최소 100px너비

<br/>

### auto-fill & auto-fit
-반응형 디자인의 기본
- `grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));` //창 너비가 늘어나면 빈 column들로 row를 채움     
  ![auto-fill](https://user-images.githubusercontent.com/85475577/147896111-591f94bf-03f5-4f70-a8d8-62d1bd4dc704.png)   
  - 보다 **정확한** 사이즈를 위해서   
- `grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));` // 창 너비가 늘어나면 element를 늘려서 row에 맞게 해줌      
  ![auto-fit](https://user-images.githubusercontent.com/85475577/147896119-06c3a8af-5834-4fd9-ac8f-6ec49e77fa0e.png)      
  - 보다 **유연한** 사이즈를 위해서     

<br/>

### min-content & max-content
- 더 이상 사이즈가 아닌 **내부에 담긴 '콘텐츠'** 에 신경 쓸 수 있음     
  ![max-content min-content example](https://user-images.githubusercontent.com/85475577/147896379-56579cdb-525c-4532-88de-19bc9a986912.png)
- min-content: 글자가 깨지거나 튀어나오는 것 없이 너비를 최소한으로 줄여줌
- max-content: 글자가 깨지거나 튀어나오는 것 없이 콘텐츠의 최대 너비에 맞춰줌
- 반응형의 기본. 자주 섞여 쓰임
  - e.g. `grid-template-columns: repeat(5, minmax(max-content, 1fr));`
  - e.g. `grid-template-columns: repeat(auto-fit, minmax(20px, max-content));`
