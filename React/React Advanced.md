*From [ReactJS Masterclass by nomadcoders](https://nomadcoders.co/react-masterclass)*

## 다양한 CSS 적용방법과 단점
1. Global CSS 
	- 모든 어플리케이션에 적용된다는 점
2. Inline CSS
	- JavaScript 코드를 적어야한다는 점
	- hover 등을 적용하지 못한다는 점
3. Moduler CSS
	- className들을 계속해서 복붙해줘야한다는 귀찮음

- styled-components는 이 모든것을 웃도는 스타일 적용 방법이다.
- React.js 스타일 적용에 최적화 되어있다.
- 생산성을 높여준다. e.g. 다크모드 등을 만들기 쉽게 해준다.


## Styled-components
- CSS를 살펴보지 않더라도 각 컴포넌트가 맡은 일을 파악할 수 있다.
- 브라우저 검사탭 들어가서 보면 className이 자동으로 생성되어 있는걸 확인 할 수 있다.

<br/>

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

<br/>

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

<br/>

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

<br/>

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

<br/>

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

<br/>

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

<br/>

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

<br/>

## TypeScript
- 실수를 줄여줌으로써 더욱 생산적이고 윤택한 개발 환경을 구축해준다.
- JS를 기반으로 한 프로그래밍 언어: 문법도 JS와 거의 비슷하지만 새로운 기능이 추가된 것.
- Strongly-typed 언어: 프로그램이 실행되기 전에 확인이 가능하다.
- 브라우저는 JS만 이해하고 TS는 이해하지 못한다. 컴파일의 과정이 자동으로 진행됨.

### Prop Types과 비교
- Prop Types의 경우, prop의 존재여부를 확인해주긴하나 코드를 실행한 **후**에 **브라우저 콘솔**에서나 확인이 가능하다.
- TS는 코드 실행 **전**에 확인이 가능하다.

### TypeScript가 뭐하는 애인지 대략적인 예시
```typescript
const plus = (a:number, b:number) => a + b;
```

<br/>

## 설치
1. Create React App을 타입스크립트로 시작하는 방법
`npx create-react-app my-app --template typescript`

2. 기존 Create React App으로 만든 프로젝트에 타입스크립트 설치하는 방법
`npm install --save typescript @types/node @types/react @types/react-dom @types/jest`

### 확장자명
- 파일 이름은 `.ts`이며, TypeScript와 React에선 `.tsx`를 사용한다.

<br/>

## [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)
- 기존의 라이브러리나 패키지들은 처음부터 TS을 타겟팅하여 만들어진게 만들어진게 아니다. e.g. styled-components
- 이에 새롭게 라이브러리를 서치해주어야 한다. e.g. `npm i --save-dev @types/styled-components`
### `@types`(type definition)는 무엇인가?
- DefinitelyTyped: 기존 JS에서 사용하던 라이브러리를 TS에서도 사용하기 위해 만들어진 거대한 규모의 깃헙 레포
- TS용 설명이 적혀있다.
- e.g. TS에게 styled-components가 뭔지 설명함으로써 TS에서도 styled-components를 사용할 수 있게끔 해준다.

#### 🚫`Module not found: Error: Can't resolve 'styled-components' in ...`
- 기존의 `npm install styled-components`가 설치된 상태에서 `npm i --save-dev @types/styled-components`설치가 동반되어야 에러가 해결되더라.
- 왜냐하면 `@types`는 TS에게 styled-components가 무엇인지 설명해주는 아이이기 때문에 설명이 될 본체가 일단 존재해야지!

<br/> 

## How to Type
- "Component에 type를 추가한다"의 의미는 "TS에게 설명한다"는 뜻이다.

### Interface
- “Props를 넘겨줄때, 혹은 styled-components에 넘겨줄때 interface 해준다”: **Interface는 object의 모양을 설명해준다.**
- 아래의 코드 예시 참고
1. Component의 props에 type을 주는 방법 예시: `CircleProps`
2. styled-components에도 넘겨주는 방법 예시: `ContainerProps`
```typescript
interface ContainerProps {
  bgColor: string;
}

const Container = styled.div<ContainerProps>`
  width: 200px;
  height: 200px;
  background-color: ${(props) => props.bgColor};
`;

interface CircleProps {
  bgColor: string;
}

const Circle = ({ bgColor }: CircleProps) => {
  return <Container bgColor={bgColor} />;
};
```

<br/>

## Default & Optional Props
### 1. Prop값이 무조건 required가 아닌 optional일수도 있게 만드는 법
```typescript
interface CircleProps {
  bgColor: string;
  borderColor?: string;
}
```
- 위의 코드 예시 속 `borderColor?: string;`처럼, `?`을 통해 prop값을 `string`또는 `undefined`로 줄 수 있다. (=== `borderColor: string | undefined`

### 2. 값이 내려오지 않았을 때 Default값을 부여하는 방법
```typescript
interface CircleProps {
  bgColor: string;
  borderColor?: string;
  text?: string;
}

const Circle = ({
  bgColor,
  borderColor,
  text = "I'm a default text!",
}: CircleProps) => {
  return (
    <Container bgColor={bgColor} borderColor={borderColor ?? bgColor}>
      {text}
    </Container>
  );
};
```
2-1. TS문법 `borderColor={borderColor ?? bgColor}`
- borderColor가 있으면 물려받은 borderColor를 사용하고,
- borderColor가 없으면 디폴트값으로 bgColor를 사용한다.
- 아니면 e.g. `borderColor={borderColor ?? "white"}`와 같이 직접 적는 것도 가능하다.

2-2. JS문법 `text = "I'm a default text!"`
- props를 내려받으며 디폴트값 적기

<br/>

## State
- TS는 똑똑해서 디폴트값으로 어떤 타입을 쓸지 미리 예측한다. e.g. 아래 코드 예시 속 `useState(0)` 디폴트 값이 `0` number로 주어졌기 때문에 예측 가능하다.
-  useState의 value값이 string 또는 number 타입이 되길 원한다면 `useState<string | number>(0)`와 같이 작성 가능하지만, 보통 기존의 타입을 그대로 쓰는 경우가 많아 그렇게 자주 쓰이지는 않는다.
```typescript
const Circle = ({
  bgColor,
  borderColor,
  text = "I'm a default text!",
}: CircleProps) => {
  const [value, setValue] = useState<string | number>(0);
  setValue(0);   //✅
  setValue("hello");   //✅
  setValue(true);   //❌

  return (
    <Container bgColor={bgColor} borderColor={borderColor ?? bgColor}>
      {text}
    </Container>
  );
};
```

<br/>

## Forms, Events
<img width="441" alt="스크린샷 2022-02-07 오후 5 15 05" src="https://user-images.githubusercontent.com/85475577/152749867-e1661a77-39b5-40f3-9587-fb5eb26c5ba8.png"/>

- `any`는 아무 타입이나 될 수 있다는 의미지만, 이는 TS를 사용하는 의미가 없어지니 최대한 **지양**하도록 한다. 
```typescript
function App() {
  const [value, setValue] = useState("");
  const onChange = (event: React.FormEvent<HTMLInputElement>) => {
    const {
      currentTarget: { value },
    } = event;
    setValue(value);
  };
  const onSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log("Hello", value);
  };
  return (
    <div>
      <form onSubmit={onSubmit}>
        <input
          onChange={onChange}
          value={value}
          type="text"
          placeholder="username"
        />
        <button>Log In</button>
      </form>
    </div>
  );
}
```
- event의 보호를 받을 수 있다.
	- `event: React.FormEvent<HTMLInputElement>`: input 태그에서 일어나는 이벤트라는 걸 알려준다.
	- `event: React.FormEvent<HTMLFormElement>`: form 태그에서 일어나는 이벤트라는 걸 알려준다.
	- 이와 같은 방식은 **React.js에서만** 정답일수 있다. 다른 라이브러리에서는 다른 방식을 찾아야 할 수도 있다.
	- React.js에서 사용되는 event는 [여기](https://reactjs.org/docs/events.html)서 확인해볼 수 있다.
- `target`이 아닌 `currentTarget`인 점 주의하기.
- **TypeScript는 구글링과 document 참고가 답이다.**

<br/>

## TS에서 Theme 적용하기(다크모드, 라이트모드)
- [TypeScript와 styled-components 함께 사용하기](https://styled-components.com/docs/api#typescript)

### 1. Declaration File(`styled.d.ts`)
> Declaration files, if you're not familiar, are just files that describe the shape of an existing JavaScript codebase to TypeScript. By using declaration files (also called . ... ts files), you can avoid misusing libraries and get things like completions in your editor.
```typescript
import "styled-components";

declare module "styled-components" {
  export interface DefaultTheme {
    textColor: string;
    bgColor: string;
    btnColor: string;
  }
}
```
- 내 styled-components의 테마를 **정의**할 수 있다.

### 2. `theme.ts`
- `styled.d.ts` 이 정의 파일 속 속성들과 똑같아야 한다.
```typescript
import { DefaultTheme } from "styled-components";

export const lightTheme: DefaultTheme = {
  bgColor: "white",
  textColor: "black",
  btnColor: "tomato",
};

export const darkTheme: DefaultTheme = {
  bgColor: "black",
  textColor: "white",
  btnColor: "teal",
};
```
- 속성들이 자동완성되어서 실수를 방지할 수 있다.

### 3. `index.tsx`
- `<ThemeProvider>`를 통해 theme을 지정해줄수 있다.
```typescript
import React from "react";
import ReactDOM from "react-dom";
import { ThemeProvider } from "styled-components";
import App from "./App";
import { darkTheme, lightTheme } from "./theme";

ReactDOM.render(
  <React.StrictMode>
    <ThemeProvider theme={darkTheme}>
      <App />
    </ThemeProvider>
  </React.StrictMode>,
  document.getElementById("root")
);
```
- `<ThemeProvider>` 내부에 존재하는 모든 컴포넌트들은 theme object에 접근할 수 있게 된다.


### 4. `App.tsx`
-  각 styled-components에서 theme을 가져다 쓸 수 있다.
```typescript
import React, { useState } from "react";
import styled, { keyframes } from "styled-components";

const Container = styled.div`
  background-color: ${(props) => props.theme.bgColor};
`;

const H1 = styled.h1`
  color: ${(props) => props.theme.textColor};
`;

function App() {
  return (
    <div>
      <Container>
        <H1>PROTECTED</H1>
      </Container>
    </div>
  );
}

export default App;
```









<hr/>

## Etc.
- "vscode-styled-components Extension" 다운받으면 styled-components 내에서도 자동완성 적용된다.

