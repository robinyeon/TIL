## [label](https://developer.mozilla.org/ko/docs/Web/HTML/Element/label)
- `<label>`을 `<input>`요소와 연관시키려면, `<input>`에 **id 속성**을 넣어야한다.
- 그런 다음 `<label>`에 id와 같은 값의  **for 속성**을 넣어야한다.
```html
<div class="preference">
    <label for="cheese">Do you like cheese?</label>
    <input type="checkbox" name="cheese" id="cheese">
</div>
```
