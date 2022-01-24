*From [Kakao-clone by nomadcoders](https://nomadcoders.co/kokoa-clone)*

## Fixed
- 스크롤 내려도 그 자리 그대로 존재하게 해준다. e.g. 웹사이트 우측하단에 자주 있는 채팅봇 버튼, 스크롤 내려도 항상 상단에 존재하는 헤더
- top, left, right, bottom이라는 프로퍼티 중 하나만 적용해도 다른 레이어에 존재하게 된다. block이든 margin이든 다 상관없게 된다.

## Static
- 디폴트 값
- 레이아웃이 박스를 처음 위치하는 곳에 두는 것

## Relative
- element가 **처음 위치한 곳을 기준**으로 상하좌우 움직인다.

## Absolute
- **가장 가까운 relative 부모를 기준**으로 상하좌우 움직인다.
- 즉, absolute를 쓰려면 기준이 될 relative를 설정하고 사용하는게 편하겠지.
