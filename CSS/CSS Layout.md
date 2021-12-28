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

## grid
- 격자무늬(grid)를 만들기 힘든 flexbox에서 일어나는 문제를 해결하기 위해 등장      
  - flexbox 문제점 예시      
    ![flexbox의 문제점_예시](https://user-images.githubusercontent.com/85475577/147576422-f5114acd-65c9-4c95-9ef4-14a0a0edee37.png) 
    
- most of the time, you're gonna talk with father element.





























