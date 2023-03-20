## 기존 방식

```jsx
import React from 'react';

function Component(props) {
	return (
		<div>
			안녕하세요 제 이름은 {props.name} 입니다.
      나이는 {props.age} 살 이구요.
      잘 부탁드려요!
		</div>
	)
}
```

- 함수형 컴포넌트에서 props 값을 조회할 때마다 props.name, props.age처럼 props. 키워드를 붙여 사용한다.
- 하지만, ES6가 제공하는 **비구조화 할당 문법을 사용하면 내부 값을 바로 추출**할 수 있다.

## 비구조화 할당(destructuring assignment)

### 첫번째 방법

```jsx
import React from 'react';

const MyComponent = (props) => {
  const {name, children} = props;
  return(
    <div>
    	안녕하세요 제 이름은 {name} 입니다.
        나이는 {age} 살 이구요.
        잘 부탁드려요!
    </div>
  )
}
```

- **객체에서 값을 추출하는 문법**을 비구조화 할당이라고 부른다.
- 이 문법은 구조 분해 문법이라고도 불리며, 함수의 파라미터 부분에서도 사용할 수 있다.

### 두번째 방법

```jsx
import React from 'react';

const MyComponent = ({ name, age }) => {
  return(
    <div>
    	안녕하세요 제 이름은 {name} 입니다.
        나이는 {age} 살 이구요.
        잘 부탁드려요!
    </div>
  )
}
```

- 함수의 파라미터 부분에 비구조화 할당을 적용하는 방법이다.

## 참고 자료

- [https://velog.io/@apro_xo/React.js-비구조화-할당으로-props-값-추출하기](https://velog.io/@apro_xo/React.js-%EB%B9%84%EA%B5%AC%EC%A1%B0%ED%99%94-%ED%95%A0%EB%8B%B9%EC%9C%BC%EB%A1%9C-props-%EA%B0%92-%EC%B6%94%EC%B6%9C%ED%95%98%EA%B8%B0)
- [https://velog.io/@orangetreex/React-정복기5-비구조화-할당-문법](https://velog.io/@orangetreex/React-%EC%A0%95%EB%B3%B5%EA%B8%B05-%EB%B9%84%EA%B5%AC%EC%A1%B0%ED%99%94-%ED%95%A0%EB%8B%B9-%EB%AC%B8%EB%B2%95)
- [https://mrsql.tistory.com/m/35](https://mrsql.tistory.com/m/35)