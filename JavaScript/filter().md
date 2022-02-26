## [filter()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
- 주어진 조건에 부합하는 모든 요소들을 모아 새로운 배열로 반환한다.
```javascript
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter(word => word.length > 6);

console.log(result);
// expected output: Array ["exuberant", "destruction", "present"]
```
- 즉 조건에 부합하지 않는 요소들을 filter하여 리턴한다.
