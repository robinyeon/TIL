## Validation
- **Mongoose helps us validate the data.**(Protected by Mongoose)
- 모델에서 데이터 형태를 미리 자세히 지정해뒀을때의 장점.
- e.g. `title: String`으로 지정하면 입력란에 숫자가 입력되어도 자동으로 문자열로 변환된다. (5 -> “5”)

<br/>

## MongoDB
- terminal내에서 진입: mongod -> mongo -> show dbs-> use youtube-clone -> show collections
- collection: document들의 묶음. youtube-clone의 경우에는 videos(video들의 묶음)
- DB에 데이터를 저장하는 방법 2가지:
```javascript
await Video.create({
    title,
    description,
    createdAt: Date.now(),
    hashtags: hashtags.split(",").map((word) => `#${word}`),
    meta: {
      views: 0,
      rating: 0,
    },
  })
```
===
```javascript
const video = new Video({
    title,
    description,
    createdAt: Date.now(),
    hashtags: hashtags.split(",").map((word) => `#${word}`),
    meta: {
      views: 0,
      rating: 0,
    },
  });
await video.save();
```

<br/>

## Default
- e.g. 작성시간을 설정할 경우, 매번 `createdAt: Date.now()` 라고 쓰기 귀찮으니 아예 모델(스키마)에 디폴트 값으로 넣어버리면 편하다.
```javascript
  createdAt: { type: Date, required: true, default: Date.now },
```
- 바로 실행시키고 싶지 않으니 `()`을 제한다. 만약 `Date.now()`라고 하면 그 스키마가 있는 파일을 저장하는 시간이 디폴트로 저장

<br/>

## Power of [defining schema](https://mongoosejs.com/docs/schematypes.html)
- type 이외에도 대소문자(uppercase, lowercase), trim 등을 설정할 수 있다.
- **최대한 구체적으로 데이터 설정하기**

<br/>

## 정규식 (Router with Regular Expression)
```javascript
// 기존 라우터
videoRouter.get("/:id(\\d+)", watch);
```
- 기존 라우터 내 파라미터(digit only)의 경우, MongoDB가 만들어주는 _id(24byte hex string)를 담기 힘들다.

```javascript
videoRouter.get("/:id", watch);
```
- 하지만 위와 같이 수정을 하게 되면 `/update` 역시 id로 인식되는 문제가 생긴다.
- 해결책
	1. 코드 순서 바꾸기: `/update`를 위에 둔다.
	2. 16진법 값을 배워서 적용하기: 24바이트의 16진법으로 아이디값을 부여한다. 
		- `[0-9a-f]{24}` : 0부터9, 그리고 a부터 f까지의 24자 string을 찾아내는것. (24byte hex string을 적용한것)
> Return the ObjectID id as a 24 byte hex string representation.]
> - [MongoDB docs](https://mongodb.github.io/node-mongodb-native/api-bson-generated/objectid.html)
- [이 사이트](https://www.regexpal.com/)에서 정규식을 시험해 볼 수 있다.

<br/>

## [Model.findById](https://mongoosejs.com/docs/api/model.html#model_Model.findById)
- id로 데이터를 찾을 수 있게 해준다.
- id가 `req.params`에서 온다는 사실을 꼭 기억하기.
### 즉, 몽고디비가 만든 16진법 24바이트를 -> (라우터 파일에서) express를 시켜 url을 인식하게 만들어 id라는 변수에 넣고 -> (컨트롤러에서) req.params로 써서 id를 받아온 후 findById로 가져다가 사용한다.


<br/>

## `.exec()`
- `.exec()`를 호출하면 몽구스는 프로미스를 리턴해준다. (Video.find().exec())
- 하지만 youtube-clone의 경우에는 이미 async await를 적용했기 때문에 별도로 추가해주지 않아도 된다.

<br/>

## 404 처리
- 사용자가 존재하지 않는 id에 접근하는 경우
- 일반적으로 에러를 if에서 처리한다: `if(!video) {…}`

<br/>

## Mongoose 활용하기
### Model.findByIdAndUpdate()
- 불러오기와 수정을 한번에 처리 할 수 있다.

###[Model.exist()](https://mongoosejs.com/docs/api/model.html#:~:text=notanid%27%20%7D).catch(noop)%3B-,Model.exists(),-Parameters)
- object 전부 불러올 필요없이 boolean으로 결과값을 받을 수 있다.
- `()`에 조건을 적어준다
- e.g. `Video.exists({_id: id})` object의 id(_Id)가 req.params의 id와 같은 경우 찾기

### [Mongoose Middleware](https://mongoosejs.com/docs/middleware.html#:~:text=mongoose-,Middleware,-Middleware%20(also%20called)
- 데이터를 저장하거나 업데이트 하기 전에 할 일을 시킬 수 있다.
- 중요한건 흐름을 방해하지 않으면서 메인기능을 수행하기 전에 가로챈다는 점. e.g. save하기 전에 먼저 실행할 함수 설정하기.
- **미들웨어는 무조건 모델이 생성되기 전에 만들어야 한다.**
```javascript
import mongoose from "mongoose";

const videoSchema = new mongoose.Schema({
  title: { type: String, required: true, trim: true },
  description: { type: String, required: true, trim: true },
  createdAt: { type: Date, required: true, default: Date.now },
  hashtags: [{ type: String, trim: true }],
  meta: {
    views: { type: Number, default: 0, required: true },
    rating: { type: Number, default: 0, required: true },
  },
});

// 아래의 Video 모델이 만들어지기 전에 여기! 이곳에 미들웨어를 만들어야 한다.

const Video = mongoose.model("Video", videoSchema);

export default Video;
```
- 이 미들웨어는 this를 통해서 해당 데이터를 불러온다.
>  In document middleware functions, `this` refers to the document.
```javascript
videoSchema.pre("save", async function () {
  console.log("We are about to save:", this);
});
```
콘솔 결과:		  
![.pre("save")_this](https://user-images.githubusercontent.com/85475577/148687678-c354a8d4-163a-4ef7-a86c-9eafddd2adc3.png)

```javascript
videoSchema.pre("save", async function () {
  this.hashtags = this.hashtags[0]
    .split(",")
    .map((word) => (word.startsWith("#") ? word : `#${word}`));
});
```
- 하지만 이는 영상을 처음 올릴 때만, 즉 업로드할때만(postUpload) 적용 가능한 미들웨어다.
- 왜냐하면 postEdit에 적용되어있는 `findByIdAndUpdate`는 `findOneAndUpdate`를 부르게 되는데, [`findOneandUpdate`는 this를 통해서 document(오브젝트)에 접근할 수 없다.](https://mongoosejs.com/docs/middleware.html#:~:text=You%20cannot%20access%20the%20document%20being%20updated%20in%20pre(%27updateOne%27)%20or%20pre(%27findOneAndUpdate%27)%20query%20middleware.)
- 따라서 postUpload뿐만 아니라 postEdit에도 적용할 수 있는 해시태그용 함수로 `.pre(“save”)`는 쓸 수 없는것. 
- Q. 그러면 다른 방법은 뭐가 있을까? A. **Statics**

### [Statics](https://mongoosejs.com/docs/guide.html#:~:text=will%20not%20work.-,Statics,-You%20can%20also)
- `Video.findById()`, `Video.create()`, `Video.exists()`와 같이 적용할 수 있는 나만의 함수를 만들 수 있다.
- `schema.static(“nameOfTheStatic”, function(){…})`

### Model.findByIdAndDelete()
- just a shortcut for `findOneAndDelete({_id:id})`
- `findByIdAndRemove()`도 존재하지만 대부분의 경우에는 `findByIdAndDelete()`를 사용하라고 공식문서에서도 권고하고 있다. (이는 아마 롤백(데이터를 복구)마저 지워버리는 remove의 성격때문인듯하다)

<br/>

## 검색기능(Search)
- `req.query`를 사용하면 손쉽게 해결 가능하다.
![req.query_url](https://user-images.githubusercontent.com/85475577/148687633-1c177a6d-d5b4-4994-9fa3-d58085d60e56.png)     
- 상기 이미지 내 `keyword=hello` 부분을 쓸 수 있는 방법이다.
- 반드시 input에 name 넣는것 잊지 않기

### [MongoDB Regex](https://docs.mongodb.com/manual/reference/operator/query/regex/)
- 검색 기능을 구현할 때, 정규식은 여러 가지를 지정할 수 있게 해준다.

#### e.g. `/welcome/ig`
- i: ignore case 대소문자 무시
- g: global 
- welcome으로 끝나는 단어만 검색: `welcome$`
- welcome으로 시작하는 단어만 검색: `^welcome`

#### Contain
```javascript
title: {
        $regex: new RegExp(keyword, "i")
      },
```
- 해당keyword를 포함하고 있는가?

#### `^` Starts with
```javascript
        $regex: new RegExp(`^${keyword}`, "i"),
```
- keyword로 시작하는 것만!

 #### `$` Ends with
```javascript
        $regex: new RegExp(`${keyword}$`, “I`”),
```
- keyword로 끝나는 것만!
