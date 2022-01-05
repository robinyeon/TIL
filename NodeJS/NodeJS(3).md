# Response
- **브라우저가 request를 보내면 서버가 response 해준다**
```javascript
const handleHome = (req, res) => {
    return res.end();  // req을 끝내는 방법중 하나로, 아무 응답도 안해서 유저가 계속 기다리게 하는 거 보단 낫지
}
```
- 또는, `return res.send("<h1>I still hate you :)</h1>")`과 같이 다양한 함수 존재하니 [express 공식문서](https://expressjs.com/ko/4x/api.html)참고
- req, res의 자리 두개가 **반드시** 들어가야 한다.

<br/>

# Middleware(미들웨어)
- **All middleware are controllers, all controllers are middlewares**.
    - controllers === handlers e.g. handleHome 같은거
- 브라우저의 req와 서버의 res중간에 있는 소프트 웨어(요청 후와 응답 전 사이)

## next()
- 모든 controller는 `next()`를 지니고 있고, 이 함수를 사용하는 순간 미들웨어가 된다. 즉, 지정되기 전까지 그 누구도 어떤 컨트롤러가 미들웨어인지 알 수 없고 모든 미들웨어는 컨트롤러인 것이다.
```javascript
const gossipMiddleware = (req, res, next) => {
    console.log("I'm in the middle!")
    next();
}

const handleHome = (req, res, next) => {
    return res.send("<h1>I still hate you :)</h1>")
}

app.get("/", gossipMiddleware, handleHome)
```
- next(): `gossipMiddleware` --그 다음--> `handleHome` 다음 함수를 실행시켜준다.
- next()는 res하지 않고 req를 지속시켜 다음으로 넘겨 준다.
  - 위의 상황에서 `gossipMiddleware`가 `return res.send("blah blah")`한다면 `handleHome`으로 넘어가지 않고 끝나버림.
- 즉 위의 예시에서는 `gossipMiddleware`가 `next()`을 호출하니 미들웨어가 되는 것.
- `(req, res, next)`
    - **req**: means '**Here's all the information you need.**'
    - **res**: means '**This is what you need to response.**'
    - **next**: means '**Here's what you need if you feel uncomfortable to response.**'

## app.use()
- e.g. `app.use(gossipMiddleware)`
- 순서가 중요하다: `app.use()` --그 이후의 라인에--> `app.get()`
    - 이처럼 미들웨어를 상단에 두면 해당 함수가 모든 route(url)에 적용된다.
```javascript
app.use(gossipMiddleware)
app.get("/", handleHome)
app.get("/login", handleLogin)
```
===
```javascript
app.get("/", gossipMiddleware,handleHome)   // 미들웨어는 왼쪽에서 오른쪽 순서로 작동한다.
app.get("/login", gossipMiddleware, handleLogin)

```

<br/>

# External Middlewares
## 예시1. Morgan
- 이용 방법(공식 홈페이지 참고): `npm i morgan`로 설치 후 아래와 같이 작성한다:
```javascript
import morgan from "morgan"; 

const logger = morgan("dev");

app.use(logger);
```         
- `morgan("dev")`     
    ![morgan_middleware](https://user-images.githubusercontent.com/85475577/148062765-a567d902-a883-4a7e-a48b-71acf980ffc0.png)     
    - 위의 그림과 같이 'Method, Path, Status code, Speed' 순으로 로그해준다.
    - dev 외에도 combine, common, short, tiny 존재.
- 이처럼 다양한 외부 미들웨어를 가져다가 사용 할 수 있으며, 당연하게도 이것들 역시 `next()`를 호출 할 수 있다.

<br/>

# Routers
- 우선, 프로젝트를 시작하기 전 "어떤 데이터를 다루게 될지"를 생각해보기
    - e.g. Youtube 클론코딩에서는 '비디오'와 '유저' 데이터를 다루게 될 것
- 바로 이 데이터들이 도메인을 짜는데에 출발점이 된다.

## Global Routers
- 홈에서 바로 갈 수 있는 라우터들
```
/ -> Home
/join -> Join
/login -> Login
/search -> Search
```

## e.g. Users Routers & Video Routers
```
/edit-user -> Edit user ==> /users/edit
/delete-user -> Delete user ==> /users/delete

/watch-video -> Watch Video ==> /videos/watch
/edit-video -> Edit Video ==> /videos/edit
/delete-video -> Delete Video ==> /videos/delete
```

- 즉, 라우터는 작업중인 주제를 기반으로 url을 그룹화 해준다.
- 또한 컨트롤러와 url의 관리를 쉽게 해주는 (개발자를 위한) 미니앱이라고 할 수 있다.

## 예외사항
- 이러한 의문점이 생길 수도 있다: 'join은 users의 행위이니 /users/join으로 설계해야하는게 아닌가?'
- 논리상으로는 옳을 수 있으나,
    1. 깔끔하지 않다
    2. 마케팅 측면에서도 (길다보니) 홍보에 불리한 점 등
    위와 같은 이유로 일부 예외를 두곤 한다.

## 사용방법 e.g. `/videos/watch` 생성
```javascript
const videoRouter = express.Router(); // 라우터 생성

app.use("/videos", videoRouter); // root url(/video)을 활용하여 '누군가 /videos 로 시작하는 url을 입력하면 videoRouter 안으로 들어가게끔하라' 입력

const handleWatchVideo = (req, res) => res.send("Watch video")
videoRouter.get("/watch", handleWatchVideo); // 이미 videoRouter안으로 들어와있는 상태이기 때문에 /videos/watch로 적지 않는다.
```










