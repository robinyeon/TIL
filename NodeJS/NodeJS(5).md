
# 데이터베이스 (Database)
- GET으로 받기만 하지말고 ‘진짜 데이터를 저장’하기

## 주소에 파라미터 넣기
```pug
h4 
	a(href=`/videos/${video.id}`)=video.title
```
- Pug에서 속성값 부분에 자바스크립트를 적용할때는 `#{}`이 아닌 ``${}``(template string)을 이용한다

<br/>

## 절대주소와 상대주소 (Absolute url & Relative url)
### 절대주소 (with `/`)
- e.g. `a(href="/edit") Edit Video &rarr;`
-  `/`와 함께 쓰임으로써 앞의 내용을 다 잘라내고 어디에 있었든 상관없이 **“root경로+/edit”**으로 이동하게 된다. (http://localhost:4000/edit)
- 절대주소와 상대주소가 헷갈린다면 그냥 절대주소로 다 작성하여 설정하는게 가장 마음 편하다.\

### 상대주소
- e.g. `a(href="edit") Edit Video &rarr;`
- 현 주소의 가장 마지막 부분을 바꿔끼운다(덧붙이는게 아님). e.g. http://localhost:4000/videos/**0** 에서 -> http://localhost:4000/videos/**edit** 으로 이동하게 된다.

<br/>

## Method & Action
- e.g. `form(method=POST, action=“/videos/edit”)`
### Action
- ‘이 데이터를 어디로 보낼 것인가?’에 대한 url을 적는 곳. 만약 현 위치와 같다면(즉 GET과 POST을 같은 url로 받는다면) 추가로 작성해주지 않아도 된다.
### Method
#### GET & POST
- 둘의 차이점 비교법: ‘이 데이터가 백엔드에 가서 무엇을 하는가?’를 고려해본다.
	- 이 데이터는 그저 데이터를 받는게 목적인가? -> GET
	- 이 데이터가 나의 DB 상태를 수정하는가? -> POST
- e.g. GET: 구글에서 검색할때 주소창을 통해서 url로 엔터치는건 getting (**url에 입력된 내용이 남아있게된다.**).
- e.g. POST: 파일을 보내거나 DB에 있는 값을 바꾸고 싶을때 사용한다. Save를 누르는건 posting.

<br/>

## Shortcut: `.route()`
- 하나의 공통된 url에 GET, POST 방식을 쓸 때 유용하다.
```pug
videoRouter.get("/:id(\\d+)/edit", getEdit);
videoRouter.post("/:id(\\d+)/edit", postEdit);
```
===
```pug
videoRouter.route("/:id(\\d+)/edit").get(getEdit).post(postEdit);
```

<br/>

## `res.redirect()`
- 브라우저가 자동으로 이동하게 해준다.
- e.g. `res.redirect(`/videos/${id}`)`

<br/>

## Form(edit.pug)_express 설정
- Form의 body를 이해하지 못하는 express를 위해 부가 설정을 해줘야 한다.
```javascript
// server.js
app.use(express.urlencoded({ extended: true }));
```
### [express.urlencoded](https://expressjs.com/ko/api.html#express.urlencoded)
- HTML 형식의 form을 우리가 사용할 수 있는 **JS object 형식**으로 통역해준다.
> This is a built-in middleware function in Express. It parses incoming requests with urlencoded payloads and is based on body-parser.
- 그 중 `{extended: true}`라는 속성은 body에 있는 정보들을 보기 좋은 형식으로 갖춰주는 일을 한다.
> … allowing for a JSON-like experience with URL-encoded.
- `server.js`에서 설정하는 순서도 중요하다. **라우터들이 작동하기 이전에 설정되었으면 하는 미들웨어**이기 때문에 라우터들의 윗쪽에 작성한다.
```javascript
app.use(express.urlencoded({ extended: true }));
app.use("/videos", videoRouter);
app.use("/users", userRouter);
app.use("/", globalRouter);
```
#### Form의 내용을 req.body로 부터 받는구나!
- 즉, **req.body로 form에 있는 값들의 JS적 접근이 가능하다.**
- 물론 미들웨어 설정이 우선시 되어야 하며(위의 내용), **input에 name 설정**은 필수다.

<br/>

## MongoDB
- **Document-based database**: 데이터를 Object형식으로 제공한다. JSON-like-document로 저장가능하다.
1. 설치 후 terminal에서 `mongo`입력하여 진입
2. `show dbs`로 기존의 데이터베이스들을 확인할 수 있으며 새로운 데이터베이스는 실질 데이터가 입력된 후에야 검색된다.( 그 외에도 `help`로 명령어들을 확인 할 수 있다.)

### [MongoDB, Mongoose 설정](https://mongoosejs.com/docs/connections.html)
- `db.js` 파일을 생성하여 별도로 작성하곤 한다.
```javascript
import mongoose from "mongoose";

mongoose.connect("mongodb://127.0.0.1:27017/youtube-clone");	//terminal내 mongo진입 시 확인 가능한 IP에 생성하고자 하는 collection(이 경우엔 youtube-clone)을 덧붙여준다.

const db = mongoose.connection;

const handleOpen = () => console.log("✅ Connected to DB");
const handleError = (error) => console.log("❌ DB Error", error);

db.on("error", handleError);
db.once("open", handleOpen);
```
- [on](https://mongoosejs.com/docs/connections.html#:~:text=mongoose.connection.on)은 여러번 발생 가능한것에 쓰임 e.g. 클릭
- once는 오직 한번만 발생: “connection이 open”이라는 말은 디비에 연결된다는 것을 의미한다.
Server.js <> (Router <> Controller <> Template)

<br/>

## Mongoose
- 몽구스는 ‘MongoDB <> JS’ 사이의 대화를 가능하게 해준다.
### Model
- 몽구스<>몽고디비 그 사이에서 개발자가 몽구스를 도와주는 방법은 ‘어떻게 생겼는지’를 정의해주는 것이다.
- e.g. 비디오 데이터가 어떻게 생겼는지, title을 가지고 있는지? 갖고 있다면 그 값은 String인지 등
- 한번 정의해주고나면, 몽구스는 아주 똑똑하기 때문에 CRUD, searching 등의 기능을 다 도와준다.
- 즉, **우리의 데이터베이스는 ‘특정 값’을 원하는게 아니라, ‘어떤 값이 어떤 형태를 지니는지’를 알고 싶어한다.** 이 때문에 Model을 만드는 것.
### Schema
- Model의 형태, 즉 생김새를 스키마라고 한다.
```javascript
import mongoose from "mongoose";

const videoSchema = new mongoose.Schema({
  title: { type: String},
  description: { type: String},
  createdAt: { type: Date},
  hashtags: [{ type: String}],
  meta: {
    views: { type: Number },
    rating: { type: Number },
  },
});

const Video = mongoose.model("Video", videoSchema);		// .model(Model의 이름(대문자로 시작), schema)

export default Video;
```
- Shortcut: `{type: String}` === `String` 으로 적을 수 있다.
- Meta is like extra stuffs.

### init.js
- `server.js`는 express와 같이 서버 설정과 관련된 것
- `init.js`는 importer라고 생각하면된다. 임포트에 이상이 없다면 `init.js`는 app을 실행시킨다.
	- 이는 즉 `package.json`의 scripts내 dev 역시 수정해줘야함을 의미한다.


## [Mongoose Query(몽구스 쿼리)](https://mongoosejs.com/docs/queries.html)

## JS가 기다리는 방법: 1. 콜백(callback), 2. Promise(async await)
### 1. callback
- 무언가가 발생되고 실행되는 함수
```javascript
export const home = (req, res) => {
  console.log("start");
  Video.find({}, (error, videos) => {
    console.log("search finished");
    return res.render("home", { pageTitle: "Home", videos });
  });
  console.log("middle");
};
```
- 콜백은 가장 마지막에 불리기 때문에 `start` -> `middle` -> `search finished`의 순으로 실행된다.
- 장점: error들을 바로 불러다가 쓸 수 있음 

### 2. Promise(async await)
- Callback의 최신버전이라고 볼 수 있다.
```javascript
export const home = async (req, res) => {
  console.log("start");
  const videos = await Video.find({});
  console.log("finished");
  console.log(videos);
  return res.render("home", { pageTitle: "Home", videos });
};
```
- await를 적기 위해서는 async가 필수다.
- 위의 코드는 콜백에 적힌 코드와 같은 내용이지만 더 직관적으로 이해가 가능하다는 장점이 있다.
- try, catch 문을 이용해서 callback에서 사용했던 error를 똑같이 갖다 쓸 수 있다.
- JS는 원래 기다리는 기능 없이 그저 위에서 아래로 읽어나갔다. 하지만 작업별로 시간이 달라서 꼬이는 경우가 생겼고, 이에 callback이라는 해결책을 만들었다. 나아가 더욱 직관적인 Promise를 사용한다면 JS는 결과값을 받을 때까지 계속 기다려준다.

<br/>

## Return and Render
- Function return은 함수를 끝낼 뿐이다.
- express에서는 render function만 사용해도 정상 렌더링하며 작동한다.
- 자주 하는 실수: res.render 이후에 res.render나 res.redirect 등을 다시금 작성하면 안된다.
	- 작동은 하지만 에러 발생: `Cannot set headers after they are sent to the client. render`
	- 즉 이미 render한 이후에 또 render할 수 없다는 뜻
- `return res.render(…)`와 같이 return과 render를 함께 써서 실수를 줄일 수 있다. (함수를 아예 종료시키니까)

<br/>
