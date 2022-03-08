***WIP***

## GraphQL
1. BackEnd NodeJS GraphQL
2. FrontEnd React GraphQL

### graphql-yoga
- React의 create-react-app 명령어와 같이 GraphQL 프로젝트를 빠르게 시작할 수 있게 해준다.

### GraphQL이 해결해주는 문제들
1. Over-fetching
- 필요한 정보보다 많은 정보를 서버로부터 받는 것 e.g. 유저의 이름만 필요한데 이메일, 프로필 이미지 등 정보 전체를 다 받게 되는 것: 비효율적
- GraphQL 이용시 over-fetching 없이 코드를 짤 수 있고 개발자가 어떤 정보를 원하는지에 대해 통제할 수 있다.

2. Under-fetching
- REST에서 하나를 완성하려고 많은 소스를 요청하는 것 e.g. 피드정보, 알람정보, 유저정보 소스를 각각 요청
- 정확히 원하는 정보만 받을 수 있고, 원하는 방식으로 조정할 수도 있다.
- GraphQL은 url이 존재하지 않는다. 하나의 종점만 있을 뿐이다.
- e.g. 아래의 query를 GraphQL 백엔드에 요청하면 같은 정보를 담은 object를 보내온다.
```GraphQL
{
    feed {
        comments
        likedNumber
    }
    notifications {
        isRead
    }
    user {
        username
        profilePic
    }
}
```
