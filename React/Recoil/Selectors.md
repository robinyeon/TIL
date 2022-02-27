# [Selectors](https://recoiljs.org/ko/docs/basic-tutorial/selectors/)

## Derived State
- 기존의 State를 활용할 수 있게끔해준다. 즉, state의 output을 활용한다.
```typescript
export const toDoState = atom<ITodo[]>({
  key: "todo",
  default: [],
});

export const toDoSelector = selector({
  key: "toDoSelector",
  get: ({ get }) => {
    const toDos = get(toDoState);
    return [
      toDos.filter((toDo) => toDo.category === "TO_DO"),
      toDos.filter((toDo) => toDo.category === "DOING"),
      toDos.filter((toDo) => toDo.category === "DONE"),
    ];
  },
});
```
- `toDoState`라는 기존의 atom을 활용하여 `toDoSelector`를 활용하는 코드
- `get`함수를 이용하여 state를 가져와 필요한 값으로 추출해낸다.
