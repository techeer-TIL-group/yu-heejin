## CSS in JS

- 스타일 정의를 CSS 파일이 아닌 자바스크립트 파일로 작성된 컴포넌트에 바로 삽입하는 스타일 기법
- 기존 웹사이트를 개발할 땐 html, css, js는 각자 별도의 파일에 두는 것이 best로 여겨졌다.
    - 하지만 React, Vue, Angular와 같은 모던 자바스크립트 라이브러리가 인기를 끌면서, **웹 애플리케이션을 여러 개의 재활용이 가능한 빌딩 블록으로 분리하여 개발하는 컴포넌트 기반 개발 방법**이 주류가 되고 있다.
    - 따라서, 웹페이지를 html, css, js 3개로 분리하는 것이 아니라, **여러 개의 컴포넌트로 분리하고, 각 컴포넌트에 html, css, js를 모조리 넣어버리는 패턴이** 많이 사용되고 있다.
- Styled Components는 CSS in JS 라이브러리 중에서도 가장 널리 사용되는 라이브러리이다.

## 사용하는 이유?

> 스타일을 자바스크립트 파일에 내장시켜 사용할 수 있다.
> 

> 재사용이 쉬운 css 커스텀 컴포넌트를 만들 수 있다.
> 
- **스타일을 자바스크립트 파일에 내장**시켜 사용하기 위해 사용한다.
- **스타일을 작성함과 동시에 해당 스타일이 적용된 컴포넌트를 만들 수 있도록 라이브러리를 제공**한다.

## 패키지 설치

```bash
npm install styled-components
```

## 기본 문법

1. styled 함수 import
    
    ```tsx
    import styled from 'styled-components';
    ```
    
2. html element 스타일링할 때
    
    ```tsx
    import styled from 'styled-components';
    
    styled.button`
    ...
    `;   // <button> html 엘리먼트에 대한 스타일 정의
    
    // 아래 코드와 유사하다.
    button {
      ...
    }
    ```
    
    - html 엘리먼트를 스타일링 할 떈 모든 알려진 html 태그에 대해 이미 속성이 정의되어 있기 때문에 해당 태그명의 속성에 접근한다.
3. react component 스타일링
    
    ```tsx
    import styled from 'styled-components';
    import Button from './Button';
    
    styled(Button)`
    ...
    `;   // <Button> 리액트 컴포넌트에 스타일 정의
    ```
    

## 고정 스타일링

- 컴포넌트 스타일링
    
    ```tsx
    export const SearchBarSelect = styled.select`
        width: 100px;
        height: 40px;
        margin-left: 12%;
        border-color: #D8D8D8;
        border-radius: 5px;
    `;
    
    // 다음과 같이 사용한다.
    <SearchBarSelect></SearchBarSelect>
    ```
    

## 가변 스타일링

- 스타일 컴포넌트는 리액트 컴포넌트에 넘어온 props에 따라 다른 스타일을 적용할 수 있다.

### 사용 예시 1

```jsx
const StyledButton = styled.button`
	...
	color: ${(props) => props.color || "gray"};
	background: ${(props) => props.background || "white"};
`;

function Button({ children, color, background }) {
	return (
		<StyledButton color={color} background={background}>
			{children}
		</StyledButton>
	);
}
```

- 버튼의 글자색과 배경색을 props에 따라 바뀌도록 작성한 코드이다.
- 자바스크립트의 || 연산자를 사용하여 props가 넘어오지 않은 경우, 기존에 정의한 기본 색상이 그대로 유지되도록 한다.
- 여기서 주의할 점은 **<Button />에 넘어온 color, background props를 StyleButton에 넘겨줘야한다는 것이다.**
- props.넘긴값으로 접근할 수 있다.

### 사용 예시2

```jsx
const StyledButton = styled.button`
	...
	${(props) =>
		props.primary &&
		css`
			color: white;
			background: navy;
			border-color: navy;
		`}
`;

function Button({ children, ...props }) {
	return <StyledButton {...props}>{children}</StyledButton>;
}
```

- props에 따라 바꾸고 싶은 CSS 속성이 하나가 아니라 여러 개일 경우도 존재한다.
- **Styled Component에서 제공하는 css 함수를 사용하면 여러 개의 CSS 속성을 묶어서 정의할 수 있다.**
- 넘겨야 할 props 값이 많아질 경우, …props 구문을 사용하면 children외에 모든 props를 간편하게 전달할 수 있다.
- 다음과 같이 하나의 props만으로 여러가지 css 속성이 한번에 적용된 버튼을 얻을 수 있다.
    
    ```jsx
    import Button from "./Button";
    <Button primary>Primary Button</Button>;
    ```
    

## 참고 자료

- [https://www.daleseo.com/react-styled-components/](https://www.daleseo.com/react-styled-components/)