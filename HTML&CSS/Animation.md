![IMG_DC6D46CFDE1D-1](https://user-images.githubusercontent.com/85475577/152683363-58c77f6b-382b-40d2-b42b-ca5ec3f65350.jpeg)
![IMG_5A5FF5638714-1](https://user-images.githubusercontent.com/85475577/152683450-4d35e6c3-f34c-4176-b22f-55d3dd8a0053.jpeg)


## [animation-delay](https://velog.io/@1703979/TIL-06)
- 애니메이션을 순차적으로 작동시킬 수 있다.
- 다만, infinite 적용시 한 사이클이 끝나면 다음 사이클에서는 delay가 적용되지 않는 현상을 확인했다.
  1. ease-in-out 외의 timing-function을 커스터마이징하여 사용하기.
  2. keyframes에서 한 사이클이 끝나는 텀을 늘리기.

- animation 속성은 적용되는 시점부터 바로 동작하게 됩니다.
- animation-delay는 원래 시작되는 시점에서부터 일정시간만큼 대기했다가 시작하게 하는 속성입니다.
- 일반적으로 속성값은 양수를 사용합니다. 하지만 음수를 넣을 경우, delay 없이 시작하지만 애니메이션의 동작지점이 절대값만큼 지난 시점부터 진행됩니다.

### animation-delay 사용 후 한 사이클을 다시 재시작 하는 방법
- 마치 setTimeout과 같이, animation-delay는 animation이 적용되는 첫 시점에만 적용된다.
- 따라서 한 사이클이 끝난 후 -> 멈췄다가 -> 다시 새로운 사이클로 재동작하게하려면, animation이 동작하는 시간에 공백을 만들어 주면 된다.
- e.g. **1s 동안 animation을 진행한다면, 0.5s(50%)에 이미 동작을 완료하고 1s(100%)에는 완료된 동작을 유지(50%와 동일한 내용)하는 코드를 작성하는 방식**
```css
@keyframes lineAnim {
  0% {
    transform: none;
  }
  25% {
    transform: scaleY(2);
  }
  50%,
  100% {
    transform: none;
  }
}
```
