# Router
- 라우터는 url이 어떻게 시작하는지에 따라(공통의 시작부분에 따라) 나누는 방법이다.
- 연습이 아닌 실제 프로젝트를 진행할 수록 컨트롤러 속 html은 길어지게 된다. 이에 라우터와 함께 두지 않고 별도 컨트롤러 폴더를 생성하는 것이 좋다.
    - e.g. userController, videoController
    - globalController는 생성할 필요가 없다.
		- e.g. 회원가입(“/login”)은 유저가 하니까 userController에 들어감.
    		- e.g. 홈(“/“)으로 들어가면 트렌딩 비디오가 보이니까 videoController에 들어감.
    		- 글로벌라우터는 url을 깔끔히 만들기 위한 것일뿐 그 이상의 의미는 없다.
- 꼼꼼히 라우터를 따져가며 컨트롤러를 하나씩 만들어가기.

## Exports

### export
- 한 파일 내에서 여러개의 데이터를 한번에 내보낼 수 있다.
```javascript
export const trending = (req, res) => res.send("Home Page Videos");
export const watch = (req, res) => res.send("Watch");
export const edit = (req, res) => res.send("Edit");
```
- 다른 파일에서 임포트 할때에는 아래와 같이 `{}`안에 작성한다:
```javascript
import {trending} from "../controllers/videoController";
```
- 임포트 시 반드시 실제 name을 적어야 한다.

### export default
- 한 파일 내에서 하나만 내보낼 수 있다.
```javascript
// export의 경우
export default join;

//import의 경우
import join from "../controllers/userController";
```
- 임포트 시 이름을 임의로(마음대로) 가져올 수 있다.
	- e.g. 위의 예시에서 join대신 `import whatEvaYouWantToName from "../controllers/userController";`로도 가능하다.


## Planning Routes
```
/ -> home
/join -> join
/login -> login
/search -> search

/videos/:id -> see video
/videos/:id/edit -> edit video
/videos/:id/delete -> delete video
/videos/upload -> upload video

/users/:id -> see user profile
/users/logout -> log out
/users/edit -> edit my profile
/users/delete -> delete my profile
```

## 파라미터 e.g.`/:id`
- url 안에 변수를 넣을 수 있게 해준다.
- 예를들어 `/:potato`여도 작동한다. 가장 중요한건 `:`을 넣는 것: express에게 이것이 변수라는 사실을 알려준다.

## 순서의 문제
```javascript
videoRouter.get("/upload", upload);
videoRouter.get("/:id", see);
videoRouter.get("/:id/edit", edit);
videoRouter.get("/:id/delete", deleteVideo);
```
### Q. 왜 upload를 가장 위에 뒀는가?
```javascript
videoRouter.get("/:id", see);
videoRouter.get("/upload", upload);
videoRouter.get("/:id/edit", edit);
videoRouter.get("/:id/delete", deleteVideo);
```
- 위와 같이 순서를 설정할 경우, express는 `/upload`에 들어가는 “upload”역시 `/:id`로 인식하고 `/videos/:id`를 실행한다.
- 또한`/:id`에 ‘숫자’만 받아들이게 설정한다면 해결이 가능하다: 아래의 ‘정규식’ 파트 참고


## [정규식(Regular expression)](https://chrisjune-13837.medium.com/%EC%A0%95%EA%B7%9C%EC%8B%9D-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC-%EC%98%88%EC%A0%9C%EB%A5%BC-%ED%86%B5%ED%95%9C-cheatsheet-%EB%B2%88%EC%97%AD-61c3099cdca8)
- 문자열로부터 특정 정보를 꺼낼때 사용한다. e.g. `\d+` digit(숫자), `\w+` word(아무단어)
### `/:id(\\d+)`
	- JS이기 때문에 `\` 두번 사용
	- `:id`는 이름을 사용하기 위해 남겨둔다.(없어도 작동은 함)
		- e.g. url은 `:id`가 필요하지 않을지 모르지만, 컨트롤러는 `req.params.id`를 사용하기 위해 id라는 이름 필요하다.

<br/>

# Returning HTML
- 각각의 컨트롤러마다 html을 리턴시키면 코드가 지나치게 길어지는 문제가 생긴다.
- 또한 공통된 부분(e.g. footer)을 바꾸려면 일일이 체크해가며 바꿔줘야한다.
- 이러한 문제점을 해결하기 위해 등장한 ‘Pug’ 

## Pug
- Pug는 ‘템플릿 엔진’이다. 즉, 템플릿을 이용해서 뷰 만들기를 도와준다.
	- Pug가 제시한 방식으로 작성하면 -> 알아서 기존 html로 변환해(렌더링) 적용시켜줌.
	- view=template=html 유저가 보는 무언가를 지칭한다.
- 설치 방법(자세한 방법은 [Pug 공식문서](https://pugjs.org/api/getting-started.html) 또는 [Express 공식문서](https://expressjs.com/ko/guide/using-template-engines.html)참고하기. 굉장히 정리가 잘돼있음!)
	1. `npm i pug`로 설치
	2. Pug를 사용하기 위한 설정: “이번 프로젝트의 템플릿 엔진은 pug다”라고 express에게 알려주기.
		2-1.  server.js에 `app.set(“view engine”, “pug”)`
		2-2.  이어서 server.js에 `app.set(“views”, process.cwd() + “/views”)`
	3. express는 view engine을 서칭할때 현재 작업중인 폴더(`process .cwd`_currently working directory) 내의 views 폴더안에서 찾으니 그 안에 실제로 pug 파일을 만든다.
		- [참고](https://expressjs.com/ko/4x/api.html#app.set): views 부분을 참고하면 디폴트 값에 `process.cwd() + '/views'`가 적혀있다.
	4. 이후 src폴더 내 views 디렉토리를 생성하고 이와 같이 작성하며 활용해간다: `export const trending = (req, res) => res.render("home”);`
		- Express가 views 디렉토리에서 pug 파일 찾도록 설정되어 있어서 굳이 import나 export가 필요하지 않다.
		- 하지만 경로설정으로 인해 에러가 발생할 것이다.
### cwd 
- Currently Working Directory
- cwd는 node.js를 run하는 디렉토리로 설정된다.
- 서버를 기동하는 파일의 위치에 따라, 어디서 노드를 부르는지에 따라 작동한다.
- 현재 package.json 에서 `npm run dev`을 하다보니 package.json이 있는 위치가 cwd로 설정된다.

#### 문제점
![architecture](https://user-images.githubusercontent.com/85475577/148238085-82022ca2-ec0a-4556-905a-84bc0db38f12.png)    
- views 디렉토리는 src 디렉토리 내에 있기 때문에 cwd로 설정된 src폴더의 위치와 동일선상에 있지 않다.

#### 해결책
1. views 폴더를 바깥으로 빼내기 : 작동은 해. 근데 src안에 다 있었으면 좋겠어…
2. [세팅을 바꾸기](https://expressjs.com/ko/4x/api.html#app.set): `app.set(“views”, process.cwd() + “/src/views” );`

### Pug에서 JS 사용하기
- `#{}`내에 작성한다.
	- e.g. `footer &copy; #{new Date.getFullYear()} Wetube`

### [Partials](https://pugjs.org/language/includes.html)
- 반복되는 부분을 컴포넌트로 빼낸 것 컴포넌트로 빼낸 것
	- e.g. “/“에 있는 footer와 “/join”에 있는 footer를 반복하지 않고 동시에 수정할때에 유용하다.
- 반복되는 부분을 partials 폴더내에 따로 파일로 작성하여`include partials/footer`와 같이 끌어다가 사용하다.

### Extends & Block
- 반복되지만 내용이 달라지는 부분을 위해 ‘공간을 마련’할 수 있다.
- 베이스가 되는 레이아웃을 상속하고 거기서 extend 해나가는 것 (extends & block)’
```pug
extends base
include mixins/video

block content
    h2 Welcome! Here you will see the trending videos :)
```
- block은 베이스 템플릿이 창문을 가지는 방법이다. 즉, base 내에서 block을 통해 변경 가능한 공간을 만들 수 있다.


### Variables
- `h1 #{pageTitle}`과 `h1=pageTitle`의 차이
- `h1=pageTitle`로 작성하게 되면 변수로 인식된다. `h1 #{pageTitle}`과 같은 결과가 나오겠지만,
	- h1이 지니는게 오롯이 `pageTitle`이라는 변수 뿐이라면 `h1=pageTitle`로 작성한다.
	- 반면, 변수를 텍스트와 섞어쓴다면 `h1 #{pageTitle} | Dogtube`과 같이 작성한다.

### [Conditionals](https://pugjs.org/language/conditionals.html)
- if, else if, else와 같은 조건을 설정해주는 방법 
```pug
body    
       header 
           if fakeUser.loggedIn    
               small Hello #{fakeUser.username}
           nav
               ul 
                   if fakeUser.loggedIn
                       li
                           a(href="/login") Log out
                   else
                       li
                           a(href="/login") Login
```

### [Iteration](https://pugjs.org/language/iteration.html) 
- 반드시 배열(array)이 필요하다.
```pug
   ul
       each video in videos 
           li=video
       else
           li Sorry nothing found :(
```
- JS의 forEach와 비슷: 배열 내 각각의 아이템에 접근 가능하게 해준다.
- 위의 `else`는 pug에만 있는 문법으로, 자바스크립트가 아니다. array의 length가 0이면 실행된다.


### Mixin
- **Mixin는 똑똑한 partial이다**: 데이터를 받을 수 있는 partial
- 같은 형태를 반복해서 보이지만 다른 데이터를 지닐 때 사용한다.
- partial의 대표적인 예시는 footer, mixin의 대표적인 예시는 homepage의 video list
- mixin 폴더를 생성하여 아래 예시와 같이 작성한다:
```pug
mixin video(info)
     div  
        h4=info.title
        ul
            li #{info.rating}/5.
            li #{info.comments} comments.
            li Posted #{info.createdAt}.
            li #{info.views} views.
```
- mixin을 가져다 쓰는 경우, 임포트 마냥 상단에 include를 해줘야한다 e.g. `include mixins/video`
- `+`를 앞에 붙여줘야 한다.
> 1. controller에 배열 생성하여 home.pug에 넘겨주고,
> 2. home.pug에서 iteration+mixin			
>	```pug
>	   each potato in videos 
>      +video(potato)
>	```
> 3. mixin파일인 video.pug에서 파라미터로 받아 활용한다.

<br/>

# [MVP CSS](https://andybrewer.github.io/mvp/)
- 제대로 된 CSS는 가장 마지막에 진행하게 되겠지만, 우선 못생긴채로 만들기 싫으니 임시방편으로 디폴트 스타일을 바꿔 넣는 방법이다.
  ![before applying MVP CSS](https://user-images.githubusercontent.com/85475577/148238364-b795c9e4-383f-42d8-a968-155d220fffb5.png)   
  ![after applying MVP CSS](https://user-images.githubusercontent.com/85475577/148238463-1976472a-091e-4cbb-a9e0-b334439b0f37.png)    

<br/>

## etc.
### Reserved Words
- new, delete와 같이 JavaScript 에서 변수 로 쓸 수 없는 단어
### 파일명
- 띄어쓰기 금지
- 소문자로 작성
