*From [ReactJS Masterclass by nomadcoders](https://nomadcoders.co/react-masterclass)*

## Styled components
- React.js 스타일 적용에 최고
- 생산성을 높여준다. e.g. 다크모드 등을 만들기 쉽게 해준다.

### 다양한 CSS 적용방법과 단점
1. Global CSS 
	- 모든 어플리케이션에 적용된다는 점
2. Inline CSS
	- JavaScript 코드를 적어야한다는 점
	- hover 등을 적용하지 못한다는 점
3. Moduler CSS
	- className들을 계속해서 복붙해줘야는 귀찮음
- styled-components는 이 모든것을 웃도는 스타일 적용 방법

### styled-components
- CSS를 살펴보지 않더라도 각 컴포넌트가 맡은 일을 파악할 수 있다.
- 브라우저 검사탭 들어가서 보면 className이 자동으로 생성되어 있는걸 확인 할 수 있다.
#### Adapting
- **props를 사용**해서 설정 변경(configurable)이 가능한 스타일을 제공해준다.
  - e.g. BoxOne, BoxTwo가 아닌 Box 컴포넌트 하나에 props을 다르게 적용하기.
  ```javascript
  const Box = styled.div`
    background-color: ${(props) => props.bgColor};
    width: 100px;
    height: 100px;
  `;

  function App() {
    return (
      <Father>
        <Box bgColor="teal" />
        <Box bgColor="tomato" />
      </Father>
    );
  }
  ```
#### Extending
- 교집합이 되는 부분을 함수로써 불러온 후 **확장**하여 사용 가능하다.
```javascript
const Box = styled.div`
  background-color: ${(props) => props.bgColor};
  width: 100px;
  height: 100px;
`;

const Circle = styled(Box)`
  border-radius: 50%;
`;
```

#### As
- 기존에 존재하는 styled-components의 스타일은 그대로 사용하고 싶은데 html태그는 바꾸고 싶을때 사용하면 좋다.
	- e.g. button에 있는 css를 a(link)로 사용하고 싶을때
  ```javascript
  const Btn = styled.button`
    color: white;
    background-color: tomato;
    border: none;
    border-radius: 10px;
  `;

  function App() {
    return (
      <Father>
        <Btn>Log In</Btn>
        <Btn as="a" href="/">
          Log In
        </Btn>
      </Father>
    );
  }
  ```

#### Attributes
- styled-components 내에 attribute를 부여할 수 있다. 
```javascript
const Input = styled.input`
  background-color: tomato;
`;

function App() {
  return (
    <Father>
      <Input required />
      <Input required />
      <Input required />
      <Input required />
      <Input required />
      <Input required />
      <Input required />
    </Father>
  );
}
```
위와 같이 반복적으로 작성하는 대신, 아래와 같이 반복없이 작성 가능하다.
```javascript
const Input = styled.input.attrs({ required: true, minLength: 10 })`
  background-color: tomato;
`;

function App() {
  return (
    <Father>
      <Input />
      <Input />
      <Input />
      <Input />
      <Input />
      <Input />
    </Father>
  );
}
```
