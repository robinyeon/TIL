***WIP***

## Response
- **브라우저가 request를 보내면 서버가 response 해준다**
```javascript
const handleHome = (req, res) => {
    return res.end();  // req을 끝내는 방법중 하나로, 아무 응답도 안해서 유저가 계속 기다리게 하는 거 보단 낫지
}
```
- 또는 `return res.send("<h1>I still hate you :)</h1>")`과 같이 다양한 함수 존재하니 [express 공식문서](https://expressjs.com/ko/4x/api.html)참고
- req, res의 자리 두개가 **반드시** 들어가야 한다.

<br/>


# Middleware(미들웨어)
- All middleware are controllers(=handlers)(e.g. handleHome 같은거), all controllers are middlewares.
- 브라우저의 req와 서버의 res중간에 있는 소프트 웨어(요청 후와 응답 전 사이)


## next()
- 미들웨어는 다른 controller들과 같지만 next 함수를 지니게 된다. (즉 모든 controller가 지니고 있는데 next를 쓴다면 미들웨어가 되는것. 미들웨어로 지정 전까진 그 누구도 그게 미들웨어인지 모르는거지)
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
- next(): `gossipMiddleware` -> `handleHome` 다음 함수를 실행시켜줌. next()는 res하지 않고 req를 지속시켜 줌(넘기고 넘겨서).
  - `gossipMiddleware`가 `return res.send("blah blah")`한다면 `handleHome`으로 넘어가지 않고 끝나버림.
- `gossipMiddleware`가 `next()`을 호출한다면 미들웨어가 되는거지

## app.use()
- 순서가 중요 app.use() -> app.get()
- middleware를 위에다가 두면 모든 route(url)에 적용된다.
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





















