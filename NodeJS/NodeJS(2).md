***WIP***

# NodeJS란?
- 크롬 V8 자바스크립트 엔진으로 빌드 된 **자바스크립트 런타임**
- 초반의 자바스크립트는 브라우저 내에서만 돌아갔다: 웹사이트를 좀 더 유연하게 만들기 위해서 시작된거니까.
- 따라서 초반의 자바스크립트는 html, css, 브라우저와 함께 쓰였다면, 이후 Ryan이라는 사람이 자바스크립트를 브라우저에서 분리해서 NodeJS를 만들었다.
- 브라우저에서 분리하고 Python, Java, C처럼 프로그래밍 언어로 만든거지. 혁명적: 백엔드를 만들수도, 이미지를 처리 할 수도 있음.
- 즉, **NodeJS는 브라우저에서 분리된 자바스크립트**

<br/>


# npm
- **패키지 매니저**: 작은 패키지들을 미리 만들어놨는데, 개발을 쉽게 해줌. 새로 계속 만들어지고 있음. npm으로 그 패키지들을 사용할 수 있음: 이러한 이유로 nodeJS가 더 폭발적으로 성장할 수 있었다.
- e.g. Express라는 package는 한달에 7천만이 넘게 다운로드 되고 있다. > npm install express를 통해 간편히 사용가능!
  - boring and old but stable, almost perfect.
- NodeJS와 소통 가능하게끔 도와줌.
- Pre-install: npm은 NodeJS와 함께 설치가 된다(default choice).

<br/>


# package.json
- 파일에 정보를 입력하는 방식: 프로그래머가 파일에 정보를 저장하기 위해 만든 방식 중 하나다.
- NodeJS인 경우 이런 파일의 이름은 'package.json'이어야 한다. this is not optional. gotta be lowercase 'package.json'
- `main`: 내가 패키지를 배포할 시 사람들이 사용하게 될 부분: 즉 지금은 당장 안필요하다는거지
- `dependencies`: 이것이 작동되기 위해 **필요한** 패키지들.
 
## devDependencies
- dependencies: 프로젝트에게 필요한 패키지 e.g. 자동차가 굴러가기 위해서 필요한 가솔린
- devDependencies: 개발자(사람)에게 필요한 dependencies. e.g. 내가 자동차를 운전할 때 음악이 없으면 안되는 것
- node_modules에 설치되는건 마찬가지이므로 그렇게 큰 문제는 아니다. 
- `--save-dev`
  - e.g. `npm install --save-dev @babel/core`에서 --save-dev을 지우고 설치한다면 dependencies에 들어가게 된다.
  - package.json은 그저 텍스트 파일이기 때문에 다른 곳에 생겼다면 그저 옮기기만 하면 됨. 그렇게 큰 문제가 아님.

## scripts
- 이 프로젝트(폴더) 내에서 내가 실행(run)하고 싶은 것을 적는 곳.
- 즉, **콘솔에서 내가 만든 scripts를 쓸 수 있다**.
```json
 "scripts": {
    "win": "node index.js"
  },
```
- `npm run win`을 통해서 `node index.js`을 실행할 수 있게 된다.        
  ![npm run win](https://user-images.githubusercontent.com/85475577/147930725-793a7610-8a6c-4404-aafd-f580a1f84fac.png)

## node_modules
  - npm으로 설치한 모든 패키지가 들어가는 폴더
  - express는 혼자 작동되지 않고 다른 패키지들이 필요하다(=== `dependencies`). 이에 node_modules안에 수많은 것들이 설치된 것.
  - dependencies도 dependencies를 지니고 있음: 체인처럼 연결된 dependencies. 그 모든 것을 node_modules안에 담은 것.

## npm install === npm i
- **주의**: 반드시 package.json 창을 닫고 실행하기. 왜냐하면 괜히 수정된 버전을 저장하지 않고 npm install을 실행할 시 충돌이 일어나기 때문이다.
- package.json 내 dependencies를 확인하고 필요한 모든것들을 한번에 다운받아 준다: package.json이 중요한 이유
  - e.g. package.json이 있으니 누구에게 프로젝트를 전달할 때 node_modules 용량이 엄청날테니 제하고 보낸 후 `npm i`만 하면 되는거지: 제하는 방법 `.gitignore`

## package-lock.json
- 패키지가 수정 됐는지 해시값으로 확인.
- 우리 프로젝트를 공유하여 누군가 `npm i`을 실행했을 때, package-lock.json안에 있는 version **그대로** 설치되게끔 락을 걸어둔 것

## .gitignore
- `.gitignore`폴더 내에 `/node_modules` 작성하면 완료.

<br/>

# Babel
- **"Babel is a JavaScript compiler"**: NodeJS가 새로운(신문법) 자바스크립트를 이해할 수 있게끔 도와줌.
- NodeJS가 이해하지 못하는 최신 자바스크립트 코드가 있음. 두가지 해결책: (1) 노드가 이해하는 자바스크립트만 쓰기 (2) babel 사용하기.
- Babel을 사용하면 내가 지금 사용하는 node의 버전이 최신 자바스크립트를 이해하는지 못하는지 걱정할 필요가 없다.

## babel.config.json
```json
{
  "presets": ["@babel/preset-env"]
}
```
- babel을 실행하기 위해서는 다양한 것들을 우선적으로 설정해줘야한다.
- babel이 babel.config.json 파일을 찾아서 자동으로 담겨있는 내용에 맞게 설정을 진행한다.
- preset은 babel을 위한 엄청 거대한 **플러그인**(서로 소통하게 해줌). 그중에서 가장 유명한 것이 "@babel/preset-env".

## babel-node
```
"scripts": {
    "dev": "babel-node index.js"
  },
```
- (babel의 docs대로 모든 것 설치설정 이후,) package.json 내 scripts 부분에 위와 같이 `"dev": "babel-node index.js"`작성시, 더이상 `node index.js`가 아닌 babel이 탑재된 babel-node가 실행되어 최신 문법 코드를 동작시킬 수 있게된다. 

<br/>

# Nodemon
- 파일이 수정되면 nodemon이 **자동으로 재시작**을 해줌 
- 우리가 만든 파일이 수정되는 걸 감시해주는 패키지
```json
"scripts": {
    "dev": "nodemon --exec babel-node index.js"
  },
```
- 위와 같이 설정하면 `npm run dev`를 매번 칠 필요없이 처음 한번만 실행 후 자동으로 재시작 된다.    
  ![Nodemon](https://user-images.githubusercontent.com/85475577/147939463-d90c3f90-6700-449d-88cb-1543a6b45765.png)   
- **주의**: index.js의 위치를 바꿀경우(e.g. src폴더 안에 넣기) scripts역시 수정해줘야 한다.
```json
"scripts": {
    "dev": "nodemon --exec babel-node src/index.js"
  },
```

<br/>

# Server
```javascript
import express from "express";
// 기존의 `const express = require("express");`과 같은 것. babel을 깔았기에 사용 가능한 문법
// `~ from "express";` node_modules 안에서 express를 찾는다는 의미

const app = express();
// express 쓰기 위해 따라야 하는 순서중 하나로, express application을 생성한다.

app.listen(4000, () => {console.log("Server listening on port 4000 🚀")})
//  app.listen()`: 서버가 사람들이 뭔가를 요청할 때까지 기다리고 귀기울이고 있게끔 함
// 서버가 어떤 포트를 귀기울이고 있을지 설정하고 포트로 들어왔을때 실행할 콜백을 적는다.
```


## http
- 우리가 서버와 소통하는 많은 방식 중 하나(지만 가장 고착화되고 유명한 방식)
- **브라우저**가 우리를 대신해서 서버에 요청을 보낸다.
### "Cannot GET /"
- GET: http method 
- `/`는 root페이지를 의미한다.
### http method
- GET은 "GET me your homepage, GET me your profile page"과 **making the websites come to us (by browser)**.


## GET Request
```javascript
import express from "express";

const PORT = 4000;

const app = express();

// 반드시 const app=express()와 app.listen()사이에다가 application 설정(e.g. GET)

app.listen(PORT, () => console.log(`✅ Server listening on port http://localhost:${PORT} 🚀`))
```
- app을 만들고(by `const app=express()`) 설정들을 적어서 모든 준비가 완료되면 app을 listen해서 외부에 개방(by `app.listen()`)
### `app.get()`
- `app.get("/", ()=>console.log("HOME"))` 루트서버로 get 요청을 보낼 시 console.log
  - 계속 로딩만 될 것: 왜냐하면 request를 받기는 했지만 console.log 찍는것 외에는 아무것도 없다보니 계속 기다리기만 하는것.
- 반드시 **함수**로 작성해야 정상적으로 작동한다.








