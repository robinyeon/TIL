## Node.js
- 기존의 JavaScript: html 조작과 변경을 위한 언어
  - html 조작이 가능하다보니 다이나믹한 웹페이지 제작 가능
- 각 브라우저마다 JavaScript 해석엔진 탑재 e.g. V8, SpiderMonkey, Chakra
- 그 중 크롬의 V8의 성능을 높이 사 따로 출시 === Node.js === JavaScript를 브라우저 내 말고도 다른 환경에서 실행가능하게끔
- 즉, Node.js는 JavaScript 런타임(실행환경)
- Node.js 덕분에 JavaScript를 프로그래밍 언어처럼 사용하기 시작

<br/>

### Non-blocking I/O
1. 일반 프로그래밍 언어로 만든 서버
    - 여러개의 요청이 들어왔을 경우, 들어온 순서대로 차례로 하나씩 처리
    - 중간에 시간을 많이 잡아먹는 요청이 있을 경우, 그 뒤의 요청들을 처리하기 힘들어짐
2. Node.js로 만든 서버
    - 일단 모든 요청들을 접수'만' 받은 후, 빨리 결과가 나오는 순으로 처리

<br/>

### Node.js는 완료가 빨리된 것부터 처리할 수 있게 설계된 런타임 _ [Event Loop](https://youtu.be/8aGhZQkoFbQ)
1. SNS, 채팅서비스 웹서버에서 강점을 보임: 1초에 10만개가 넘는 요청들을 바로 처리 가능
    - 다른 일반 서버의 경우: 똑같은 서버를 몇만대 복사해서 만들어두거나(Scaling), CPU 멀티쓰레딩을 이용하거나, Non-blocking 스타일로 코드를 짜거나 등
    - Node.js 서버의 경우: 애초에 설계상 한꺼번에 많은 요청을 받을 수 있으니 서버 덩치 키울 걱정을 덜 수 있음 (물론처리 속도는 다른 차원의 문제)

2. 스타트업, 프로토타입용(시범 서비스)
    - 초보자도 빠르고 쉽게 제작 가능
    - 대량의 요청에도 나름 감당 가능
    - 자바스크립트 문법만으로 프론트, 백엔드 전부 가능

<br/>
  
### 단점
- 처리속도가 떨어질 수 있다
- 수학 연산이나 이미지 처리 같은 라이브러리가 부족할수도
- Non-blocking 처리방식은 다른 언어에서도 비슷하게 구현가능

<br/>

### 서버를 띄우기 위한 기본 세팅 (express 라이브러리)
```
const express = require("express);
const app = express();

app.listen(8080, function() {
  console.log("listening to 8080");
});
```
- express 라이브러리 기본 사용법이니 큰 이해가 동반될 필요는 없음
- `const express = require('express');` 아까 설치한 라이브러리를 첨부해주세요
- `const app = express();` 그걸로 새로운 객체를 만들어주세요
- `.listen(서버 띄울 포트번호, 띄운 후 실행할 코드)`쓰면 내 컴퓨터에 서버를 열 수 있음
  - 포트: 컴퓨터에는 통신 위한 6만개의 구멍이 있는데 그중 특정 구멍(#8080)을 통해 들어오면 이 서버를 띄우겠다는 의미

<br/>

### Basic GET request
- 고객(client): 주소창에 url을 입렵해서 서버에 get요청
- 서버: 누군가 '/pet'으로 들어오면 보내줄 코드를 짬
```
app.get("경로", function (요청, 응답) {
  응답.send("보낼 메세지")
});
```
e.g. 누군가가 '/pet' 으로 방문하면 안내문을 띄워주기
```
app.get("/pet", function (req, res) {
    res.send("펫용품을 쇼핑 할 수 있는 페이지 입니다.");
});
```

<br/>

### nodemon
- 저장 후 터미널에서 서버 껐다가(ctrl+c) node server.js 로 서버 켜기
- 서버 재실행 귀찮으니 자동화 시키기 위해 nodemon 라이브러리 설치 
- **라이브러리**: 도서관. 필요한 정보가 있을때 찾아쓰는 책과 같이 코드를 쉽게 짜기 위해 빌려쓰는 코드 모음집
  - express는 서버를 쉽게 만들이 귀한 라이브러리, npm은 라이브러리 설치를 위한 라이브러리
  - `npm init` 통해 `package.json` 생성: 어떤 라이브러리 썼는지 기록가능

<br/>

### HTML 파일 첨부하기
- `.sendFile(보낼 파일 경로)`
```
app.get("/", function (req, res) {
    res.sendFile(__dirname + "/index.html");
});
```
