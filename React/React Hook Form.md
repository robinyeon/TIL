***WIP***

-  input천지 일때(e.g. 회원가입폼) 유용한 React Hook Form
```typescript
const {
    register,
    watch,
    handleSubmit,
    formState: { errors },
    setError,
  } = useForm<IForm>({
    defaultValues: {
      email: "@naver.com",
    },
  });
```
- `register`: onChange 핸들러 함수 만들고, props넣기, setState 대체. 문자열을 함께 보내줘서 name을 설정해줘야한다. 띄어쓰기 없이 camelCase나 snake_case
- `watch`: Form들의 변화를 감시(onChange 찍어줌)
- `handleSubmit`: 모든 validation을 마친 후 데이터가 유효할 때 handleSubmit(onValid) 함수를 호출한다.
- `formState`: Error 'type'을 표시해줌으로써 error handling. {value, message} object를 활용하여 메세지를 보낼 수도 있다.
- `defaultValues`: Input창을 디폴트값으로 미리 채워놓을 수 있다.
- `setError`: 특정한 에러를 발생시켜준다. 추가적인 에러가 필요하다면 항목의 이름을 새 지어주고 사용할 수 있다.
