## [animation-delay](https://velog.io/@1703979/TIL-06)
- 애니메이션을 순차적으로 작동시킬 수 있다.
- 다만, infinite 적용시 한 사이클이 끝나면 다음 사이클에서는 delay가 적용되지 않는 현상을 확인했다.
  1. ease-in-out 외의 timing-function을 커스터마이징하여 사용하기.
  2. keyframes에서 한 사이클이 끝나는 텀을 늘리기.
