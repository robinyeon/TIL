## 왜 ReactJS를 사용하는가?
1. There's many big websites behind ReactJS.
  - 단순히 '큰 회사들이 사용하기 때문'이 아닌, 그들이 리액트를 택한 후 보이는 행보들이(e.g. 적극투자) 더욱 리액트의 입지를 견고히 하고 있음.
2. 굉장히 큰 커뮤니티
  - ReactJS is mostly JavaScript: 자바스크립트를 사용하다보니 자바스크립트의 커뮤니티를 그대로 끌고 왔다고 봐도 과언이 아니다.
    - e.g. 라이브러리, 프레임워크, 가이드라인, 튜터, 채용 등
  - 즉, 리액트를 기반으로 한 생태계가 존재
  - Learn once, write anywhere: 여전히 그 한계치를 넓혀가는 중

### VanillaJS vs ReactJS
- 리액트는 UI Interactivity를 가능하게 함
- **즉 JS로 element 생성(업데이트)하고, React가 HTML로 변역**

<br/>

## ReactJS Rules
1. element 생성을 위해 HTML body에 직접 작성하지 않는다
  - 자바스크립트와 리액트를 이용하여 element생성
  - React가 interactivity를 만드는 엔진과 같다면, React.dom은 모든 리액트 element를 HTML body에 둘 수 있도록 해준다.
```
<script>
  const root = document.getElementById("root");
  const span = React.createElement("span");
  ReactDOM.render(span, root);        //span을 root안에 render하라는 뜻
</script>
```

<br/> 

## `.createElement()`
- 첫번째 인자: 만들 태그
- 두번째 인자: properties(props)
- 세번째 인자: 넣을 내용
- e.g. `const span = React.createElement("span", { id: "sexy-span", style: {color:"red"} }, "hello!");`

<br/>

## create-react-app 없는 바닐라 리액트 코드 예시:
```
<!DOCTYPE html>
<html>
  <body>
    <div id="root"></div>
  </body>

  <!-- importing react and react-dom -->
  <script
    src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"
    crossorigin
  ></script>
  <script
    src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"
    crossorigin
  ></script>

  <script>
    const root = document.getElementById("root");
    const span = React.createElement(
      "span",
      { id: "sexy-span", style: { color: "red" } },
      "hello!"
    );
    ReactDOM.render(span, root);
  </script>
</html>
```

<br/>

## Replacing `.addEventLister()`
```
const btn = React.createElement(
      "button",
      {
        onCick: () => console.log("clicked!"),
      },
      "click me!"
    );
```
- 두번째 parameter로 들어간 props 안에 `id`, `style`등은 html tag안에 넣어주고,
- Event listener는 따로 안 넣어줘도 대신 실행

<br/>

## Replacing `.createElement()`: JSX
- 그 전에, 브라우저가 JSX 이해할 수 있게끔 code transformer `Babel standalone` 설치 필요
- **JSX에서 자바스크립트 쓸 땐 `{}` 필수**
```
const Button = (
      <button
        style={{ backgroundColor: "tomato" }}
        onClick={() => {
          console.log("clicked!");
        }}
      >
        CLICK ME!
      </button>
    );
```
===
```
const btn = React.createElement(
  "button",
  {
    style: { backgroundColor: "tomato" },
    onClick: () => console.log("clicked!"),
  },
  "click me!"
);
```

<br/>

## Component
- 나의 컴포넌트는 **대문자**로 시작해야한다
  - e.g. 만약 소문자로 시작하면 React와 JSX는 HTML button태그라고 착각

<br/>

## Rendering
```
  <script type="text/babel">
    const root = document.getElementById("root");
    let counter = 0;
    function countUp() {
      counter += 1;
      render();
    }
    function render() {
      ReactDOM.render(<Container />, root);
    }
    const Container = () => (
      <div>
        <h3>Total clicks: {counter}</h3>
        <button onClick={countUp}>CLICK ME!</button>
      </div>
    );
    render();
  </script>

```
- 리액트의 좋은점은 UI에서 '바뀐 부분만' 업데이트 해준다는것.
  - 다시 render할때마다 Container가 통째로 바뀌는거 같지만 그렇지 않다.
- 위처럼 reRender를 굳이 하지 않아도 되는 방법: below

### How to automatically trigger re-rendering
- `useState`'s modifier(e.g. `setCounter`) would change the value, **AND** re-render automatically
```
<script type="text/babel">
    const root = document.getElementById("root");
    const App = () => {
      const [counter, setCounter] = React.useState(0);
      const onClick = () => {
        setCounter(counter + 1);
      };
      return (
        <div>
          <h3>Total clicks: {counter}</h3>
          <button onClick={onClick}>CLICK ME!</button>
        </div>
      );
    };
    ReactDOM.render(<App />, root);
  </script>
```
- modifier function : whole component will be recreated again(rerendered), but only changing the changed value on HTML.
  - when you change the state, that will trigger re-render.
  - **state가 바뀌면 리액트가 컴포넌트를 re-rendering 해준다.**

### state 바꾸는 법
- 위처럼 setState 사용하되, if need to calculate the state based on current value: `setCounter(current => current+1)` this way is much more safe.

<br/>

## props
- parents component => child component

### Btn(props)
```
   const Btn = (props) => {
      console.log(props);
      return (
        <button
          style={{
            backgroundColor: "tomato",
            color: "white",
            padding: "10px 20px ",
            borderRadius: "10px",
            border: "none",
          }}
        >
          {props.banana}
        </button>
      );
    };

    const App = () => {
      return (
        <div>
          <Btn banana="Save Changes" />
        </div>
      );
    };

```

### Or Btn({banana})\_shortcut
- props가 object고, 그 안에 banana가 있다는 사실을 아니까. 
```
   const Btn = (props) => {
      console.log({banana});
      return (
        <button
          style={{
            backgroundColor: "tomato",
            color: "white",
            padding: "10px 20px ",
            borderRadius: "10px",
            border: "none",
          }}
        >
          {banana}
        </button>
      );
    };

    const App = () => {
      return (
        <div>
          <Btn banana="Save Changes" />
          <Btn banana="Continue" />
        </div>
      );
    };

```

<br/>

## `React.memo()`
```
const Btn = ({ text, changeValue }) => { 
      return (
        <button
          onClick={changeValue}
          style={{
            backgroundColor: "tomato",
            color: "white",
            padding: "10px 20px ",
            borderRadius: "10px",
            border: "none",
          }}
        >
          {text}
        </button>
      );
    };
const App = () => {
  const [value, setValue] = React.useState("Save Changes");
  const changeValue = () => setValue("Revert Changes");
  return (
    <div>
      <Btn text={value} changeValue={changeValue} />
      <Btn text="Continue" />
    </div>
  );
};
```
- "props가 변하지 않는 컴포넌트는 re-render되지 않았으면 좋겠다" e.g. `<Btn text="Continue" />`
- e.g. `const MemorizedBtn = React.memo(Btn);`
- 기존의 Btn 대신 MemorizedBtn로 변경 -> props가 변경되는 버튼만 리렌더링 됨.
- 후에 앱 구동 속도에 영향을 줌
- 내가 렌더링을 컨트롤 할 수 있다는 것

<br/>

## prop types
- prop의 type이 어떤건지(e.g. string, number) 규정지음으로써 실수 최소화
- 임포트 후 아래와 같이 사용 가능
```
Btn.propTypes = {
      text: PropTypes.string.isRequired,
      fontSize: PropTypes.number,
    };
```

<br/>

## create-react-app
- 바닐라 리액트에서 일일이 임포팅 했던 script 등을 미리 준비해줌
- package.json에서 우리가 script 확인 가능

### install
1. terminal에서 node.js 깔린거 확인 `node -v`
2. `npx create-react-app 프로젝트이름`
3. 설치 후 `code 프로젝트이름`하면 VSCode에서 열림
4. VSCode 터미널에서 `npm start` 하면 브라우저 자동 열림(development server열림)
5. create-react-app 폴더 정리

<br/>

## CSS modular
- App.module.css 파일 만들어서 import
- e.g. `import styles from "./Button.module.css"`
- 필요에 따라 골라 꺼내쓸 수 있도록 styles라는 object에 넣는것.
```
import styles from "./Button.module.css"

function Button({ text }) {
    return <button className={styles.btn}>{text}</button>
}
```

<br/>

## useEffect
- 언제 코드가 실행될지 결정하는 방법
1. **state 변할때 전체 component, 즉 전체 code는 실행된다**
2. API call 같은 것 또한 다시 실행됨
3. **가끔 component 내부의 몇몇 코드는 처음 딱 한번만 실행되고 다시는 실행되지 않도록 하고싶을때 useEffect 사용**
- 특정 코드들은 첫 component render때에'만' 실행되게 하고 싶음 e.g. API call이 다른 state 변화 될때마다 call 된다면 무거워 

### useEffect(1, 2)
- 1: function which is going to run only once
- 2: which one to watch
```
function App() {
  const [counter, setCounter] = useState(0);
  const onClick = () => setCounter((prev) => prev + 1);
  console.log("I run ALL the time");
  const iRunOnlyOnce = () => {
    console.log("I run ONLY ONCE");
  };
  useEffect(iRunOnlyOnce, []);
  return (
    <div className="App">
      <h1 className={styles.title}>{counter}</h1>
      <button onClick={onClick} text={"Continue"}>CLICK ME</button>
    </div>
  );
}
```

### watching: 특정 무언가가 업데이트 될때만 코드 실행하기
- {keyword}가 바뀔때만 코드를 실행하기
```
 useEffect(() => {
    console.log("Search for", keyword);
  }, [keyword]);
```
- `[]` what react is watching


### useEffect recap code below:
```
console.log("I run ALL the time.");

  useEffect(() => {
    console.log("I run only ONCE.")
  }, []);

  useEffect(() => {
    if (keyword !== "" && keyword.length > 5) {
      console.log("I run only when 'keyword' changes.");
    }
  }, [keyword]);

  useEffect(() => {
    if (counter !== 0) {
      console.log("I run only when 'counter' changes")
    }
  }, [counter]);
```

<br/>

## CleanUp function
- component가 없어질때(destroy) 무언가를 실행하는 것
- useEffect의 callback내에 return 되는 **함수** === destroy 될때 실행될 것 (함수로 안적으면 적용 안되더라)
```
const byeFn = () => {
    console.log("destroyed :(");
  }
  const hiFn = () => {
    console.log("created :)")
    return byeFn;
  }
  useEffect(hiFn, [])
  return (<h1>Hello Laphoo!</h1>);
}
```

### 보통 위와 같이 적지 않고 아래와 같이 useEffect 안에 한번에 넣어서 적음
```
const Hello = () => {
  useEffect(() => {
    console.log("hi :)");
    return () => console.log("bye :(")
  }, [])
  return (<h1>Hello Laphoo!</h1>);
}
```

<br/>

## AJAX
- AJAX 모델: 여러 기술을 사용하는 접근법, 웹 개발 기법
- 빠르게 동작하는 동적인 웹 페이지를 만들기 위한 개발 기법의 하나
- XMLHttpRequest API (XHR) -> Fetch API

### [Fetch](https://wooooooak.github.io/javascript/2018/11/25/fetch&json()/)
- 자바스크립트에서 기본적으로 제공하는 fetch 함수
- Body 데이터의 form을 직접 변환해야함
  - 그래서 axios나 다른 라이브러리 쓰는 것. axios가 에러핸들링도 편함.
- Fetch 로는 데이터를 바로 사용할 수 없음
  - 한줄로 쭉 나오는 형태: 스트림일 뿐 데이터가 완전히 다 받아진 상태가 아님
  - 따라서 `.then(res=>res.json())`로 다 읽은 body의 텍스트를 promise 형태로 반환
  - `.then((json) => console.log(json))` 서버에서 주는 json 데이터가 출력됨
```
  useEffect(() => {
    fetch(`https://yts.mx/api/v2/list_movies.json?minimum_rating=8.8&sort_by=year`)
      .then((res) => res.json())
      .then((json) => {
        setMovies(json.data.movies)
        setLoading(false)
      })
  }, [])
```

### async await
- async: "얘 비동기 할건데 어떤 비동기냐면, 이 안에선 await를 사용해서 순서대로 내려갈거야"
- `fetch` -> `.then` -> `.then`보다 async await가 더 좋다
- 위의 코드를 async await 사용으로 변환:
```
  const getMovies = async () => {
    const response = await fetch(
      `https://yts.mx/api/v2/list_movies.json?minimum_rating=8.8&sort_by=year`
    );
    const json = await response.json();
    setMovies(json.data.movies);
    setLoading(false);
  }
  
  useEffect(() => {
    getMovies();
  }, []);
```

<br/> 

## map돌려서 리스트 만들때 key값 주는 것 잊지 말기
- key는 react.js에서만, map안에서 컴포넌트들을 render할때 사용
- map의 두번째인자인 index를 먹이는 방법도 있음: 고유한 값이면 무엇이든 상관없음
```
<ul>
  {movie.genres.map((genre, index) => <li key={index}>{genre}</li>)}
</ul>
```
or
```
<ul>
  {movie.genres.map((genre) => <li key={genre}>{genre}</li>)}
</ul>
```

<br/>

## create-react-app global version 문제
1.	`npm uninstall -g create-react-app`
2.	`npm add create-react-app`
3.	다시 `npx create-react-app 프로젝트명`

<br/> 

## React-router-dom
- `npm install react-router-dom`
  - 버전5 -> 버전6 달라진 점: Switch컴포넌트가 사라지고 -> Routes컴포넌트로 대체
- screen = page = route
- url을 쳐다보고 있는 component

### Router
- BrowserRouter: 보통의 웹사이트처럼 "/" url
- HashRouter: "/#/"가 추가로 붙는다

### Switch
- React Router에서는 원하면 두개의 route를 한번에 렌더링 할 수 있음
- Switch를 쓰는 이유: 한번에 하나의route만 가져오고 싶기 때문

### Route
- path를 속성으로 갖고 진행

### 제목 클릭시 이동하게 만드는법
- `<a>`링크 사용해도 되지만 그렇게 되면 전체 새로고침 됨
- 이를 위한 react-router-dom내 `Link`: 브라우저 새로고침 없이도 유저를 다른 페이지로 이동시켜주는 컴포넌트

<br/>

### Dynamic URL
- dynamic하다는건 url에 변수를 넣을 수 있다는 의미
- e.g. `/movie` -> `<Route path="/movie/:id">` then `<h2><Link to={`/movie/${id}`}>{title}</Link></h2>`
### useParams
- url내에 있는 변수(위의 `:id`와 같은것) 가져오는 법
- e.g. `<Route path="/movie/:id">` {id=34015} or `<Route path="/movie/:id">`{tomato=34015}
```
const { id } = useParams();
console.log(id);
```

<br/>

## Publishing
1. `npm i gh-pages`
2. package.json 내의 scripts에 'build' 있는것 확인
  - build run하게되면 production ready code를 생성하게 됨: 코드가 압축되고 모든게 최적화 됨
3. `npm run build` -> build 폴더가 생성됨
4. package.json 가장 마지막에 `, "homepage": "https://robinyeon.github.io/Movie-App"`
  - `,`잊지말고
  - `"homepage": "https://사용자명.github.io/연결돼있는레포명"`
5. package.json 내의 scripts에 `"deploy": "gh-pages -d build"` 추가
  - gh-pages에 디렉토리(-d) build에 있는걸 deploy하고 싶다는 의미
6. build -> deploy의 순서를 항상 지켜야하는데, 귀찮으니까 `"predeploy": "npm run build",`
  - deploy 실행하게 되면(npm run deploy) 자동으로 predeploy가 먼저 실행됨
  - 그럼 자연스럽게 build 먼저 되면서 -> deploy 진행
7. 이후 뭔가 수정하고 업데이트 하고 싶다면 `npm run deploy`만 하면 됨

