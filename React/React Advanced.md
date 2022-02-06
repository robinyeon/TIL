*From [ReactJS Masterclass by nomadcoders](https://nomadcoders.co/react-masterclass)*

## 다양한 CSS 적용방법과 단점
1. Global CSS 
	- 모든 어플리케이션에 적용된다는 점
2. Inline CSS
	- JavaScript 코드를 적어야한다는 점
	- hover 등을 적용하지 못한다는 점
3. Moduler CSS
	- className들을 계속해서 복붙해줘야는 귀찮음

- styled-components는 이 모든것을 웃도는 스타일 적용 방법이다.
- React.js 스타일 적용에 최적화 되어있다.
- 생산성을 높여준다. e.g. 다크모드 등을 만들기 쉽게 해준다.


## Styled-components
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

### Animation in styled-components
- 보통의 CSS 코드와 사용법이 대부분 비슷하지만,
1. `{keyframes}`를 import하여 사용한다.
2. String interpolation`${}`을 사용해서 적용한다.
```javascript
import styled, { keyframes } from "styled-components";

const rotateAnimation = keyframes`
0% {
transform: rotate(0deg);
border-radius: 0px;
}
50% {
border-radius: 100px;
}
100% {
  transform: rotate(360deg);
  border-radius: 0px;
  }
`;


const Box = styled.div`
  width: 200px;
  height: 200px;
  background-color: tomato;
  animation: ${rotateAnimation} 1s linear infinite;
`;
```

### Pseudo selector
- styled-components로 작성되지 않은 다른 element를 타겟하여 작성할 수 있다.
```javascript
const Box = styled.div`
  width: 200px;
  height: 200px;
  background-color: tomato;
  animation: ${rotateAnimation} 1s linear infinite;
  display: flex;
  justify-content: center;
  align-items: center;
  span {
    font-size: 36px;
    &:hover {
      font-size: 50px;
    }
  }
`;
```

- 태그명에 의존하고 싶지 않다면?: 아래의 방법과 같이 **styled-components 자체를 타겟팅** 할 수 있다.
```javascript
const Box = styled.div`
  width: 200px;
  height: 200px;
  background-color: tomato;
  animation: ${rotateAnimation} 1s linear infinite;
  display: flex;
  justify-content: center;
  align-items: center;
  ${Emoji} {
    &:hover {
      font-size: 50px;
    }
  }
`;

function App() {
  return (
    <Wrapper>
      <Box>
        <Emoji>🤓</Emoji>
      </Box>
      <Emoji>🌊</Emoji>
    </Wrapper>
  );
}
```
- 위의 코드의 경우, `<Box>` 안에 들어있는 `<Emoji>`(🤓)에만 적용된다. `<Box>` 외부의 `<Emoji>`(🌊)에는 적용되지 않는다.

## Themes
- 모든 색상을 하나의 object안에 넣어 놓은 것: property를 가진 object를 준비해놓으면 끝
- 다크모드 구현의 50%는 Theme이다. (나머지 50%는 추후에 학습할 Local estate management)
### 1. `index.js` 에서 설정
- **다크모드와 라이트모드 내의 property 이름이 같도록 작성해준다.**
- 그래야만 이후에 그저 `theme`만 listening하여 자유자재로 다크-라이트 변경 가능해진다.
```javascript
import { ThemeProvider } from "styled-components";

const darkTheme = {
  textColor: "whitesmoke",
  backgroundColor: "#111",
};

const lightTheme = {
  textColor: "#111",
  backgroundColor: "whitesmoke",
};

ReactDOM.render(
  <React.StrictMode>
    <ThemeProvider theme={darkTheme}>
      <App />
    </ThemeProvider>
  </React.StrictMode>,
  document.getElementById("root")
);
```
### 2. `App.js`내에서 props로 받아 사용
```javascript
const Title = styled.h1`
  color: ${(props) => props.theme.textColor};
`;

function App() {
  return (
    <Wrapper>
      <Title>HELLO</Title>
    </Wrapper>
  );
}
```




<hr/>

## Etc.
- "vscode-styled-components Extension" 다운받으면 styled-components 내에서도 자동완성 적용된다!

