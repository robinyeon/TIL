## [findIndex()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)

### 배열 내 제 위치에 있는 요소를 새로운 요소(값)로 대체하는 방법
1. 타겟의 인덱스를 파악한다(어디있는지)
2. 타겟이 오기 직전의 앞부분(front)과 뒷부분(back)으로 나눈다.
3. finalArray = [...front, "새로운 값", ...back]
```
const food = ["pizza", "mango", "kimchi", "kimbab"];
const target = 1;
food.slice(0, target); // ["pizza"]
food.slice(target + 1); //["kimchi", "kimbab"]
[...food.slice(0, target), "감", ...food.slice(target + 1)];
```

### findIndex()
- 위의 예시에서 `target`의 값을 구하는 방법
- `()`안 콜백에 조건을 작성한다. 배열의 앞([0])에서부터 순서대로 훑어내려가고 조건에 부합하는 첫번째 요소의 인덱스를 반환한다.
```javascript
const array1 = [5, 12, 8, 130, 44];

const isLargeNumber = (element) => element > 13;

console.log(array1.findIndex(isLargeNumber));
// expected output: 3
```
