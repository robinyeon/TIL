***WIP***

*From [ReactJS Masterclass by nomadcoders](https://nomadcoders.co/react-masterclass)*

# Styled-components
## Adopting
- `color: ${(props)=>props.bgColor}`

## Extending
- `const B = styled(A)`` `

## `as`
- extend나 prop을 받는것이 아닌,
- 스타일 그대로 가져가는 대신 HTML tag를 변경하고 싶을 때 사용

## Attributes `attrs`
- 컴포넌트 생성 시 속성값 설정이 가능하다.
- `const Input = styled.input.attrs({required:true})`
