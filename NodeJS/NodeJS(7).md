***WIP***

# Login

## 비밀번호 체크
입력한 패스워드를 해싱하고 그값이 디비에 있는 해시값과 같은지 비교하면 되는것
### bcrpyt.compare()
- bcrpyt.compare(입력된패스워드, 해싱된패스워드)로 비교할 수 있음 그러면 리설트는 불린값으로 나옴

## Sessions and Cookies
유저를 기억하는 법
1. 유저에게 쿠키 주기
쿠키 이해를 위해선 세션 이해가 우선되어야 한다.
- 세션: 백엔드와 브라우저 간에 어떤 활동을 했는지 기억하는 것. 브라우저와 백엔드 사이의 메모리(히스토리)
- 세션이 일하게 하려면 백엔드와 브라우저가 서로에 대한 정보(piece)를 갖고 있어야함: 요청을 처리한 후 서버는 누가 요청을 보냈는지도 잊어버리고 끝. 와이파이처럼 연결되어있는 개념이 아님. stateless(무상태) 한번 연결이 되었다가 끝. 서로 상태(state)가 없는거지
- 따라서 유저에게 뭔가(텍스트)를 줘서, 유저가 백엔드에게 요구할때마다 같이 가져오게 함. 그들이 누군지 기억할 수 있도록.

### express-session
- express에서 세션 처리를 할 수 있게 도와주는 미들웨어
