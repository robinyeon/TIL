## [리스트와 Key](https://ko.reactjs.org/docs/lists-and-keys.html)
- Key는 리액트가 어떤 항목을 건드릴 때 식별하는 것을 돕는다.
- Key에는 고유한 값을 활용하되 마땅히 사용할 값이 없을 땐 `map()`함수에서 부여하는 `index`를 사용할 수 있으나, 최후의 수단으로 최대한 사용하지 않도록 한다.
  - 항목의 순서가 바뀔 수 있는 경우 등에 `index`를 사용하게 되면 컴포넌트 state와 문제가 발생할 수 있기 때문이다.
```javascript
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
);
```

### `map()` 함수 내부에 있는 엘리먼트에 key를 넣어주기
- Key는 주변의 배열 안에서만 의미가 있다.
```javascript
function ListItem(props) {
  // 여기에는 key를 지정할 필요가 없다.
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 배열 안에 key를 지정한다.
    <ListItem key={number.toString()} value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

- 잘못된 예시
```javascript
function ListItem(props) {
  const value = props.value;
  return (
    // 틀렸습니다! 여기에는 key를 지정할 필요가 없습니다.
    <li key={value.toString()}>
      {value}
    </li>
  );
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 틀렸습니다! 여기에 key를 지정해야 합니다.
    <ListItem value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
