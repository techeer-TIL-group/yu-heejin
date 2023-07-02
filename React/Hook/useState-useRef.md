## 기능상의 공통점

- `useState`, `useRef`의 기능상 공통점은 **함수형 컴포넌트에서 동적으로 상태 관리**를 할 수 있게 해준다는 점이다.

## 차이점

- `useRef`를 사용한 구현에서 글씨를 화면에 띄우지 않은 것은 **`useState`와 다르게 `useRef`는 `state`를 변화시킨 후에 component를 re-render하지 않기 때문이다.**
    - 따라서 `useRef`를 사용한 구현에서 `state`를 변화시키더라도 변화 후에 re-render되지 않아 화면에는 initial value로 나타날 것이기 때문에 rendering을 할 수는 있지만 변화했다는 의미가 존재하지 않아 따로 화면에 띄우지 않는다.
- 반면 `useState`의 경우, 선언한 `state`가 setter function에 의해 update될 경우, re-rendering process가 진행된다.
- 결과적으로 각각의 `state`에서 사용할 hook을 설계할 때, **주로 `state`의 re-rendering여부를 바탕으로 결정하는 것이 좋다.**
    - 렌더링이 필요한 state의 경우 useState를 사용하는 것이 간편하게 상태 관리를 할 수 있으며, 필요하지 않은 경우 useRef를 사용하는 것이 좋다.
- `useRef`를 통해 반환된 객체는 **component 생애주기 내내 변화하는 값을 가리킨다.**

## useRef for naming

```jsx
import React, {useRef} from 'react';
import Mycomponent from './Mycomponent';

const Naming = () => {
  const inputEl = useRef(null);
  
  const onClick = () => {
    inputEl.current.focus();
  }
  
  return (
    <div>
      **<Mycomponent ref={inputEl}/>**
      <button onClick={onClick}>Focus Text</button>
    </div>
  );
};

export default Naming;

import React, { forwardRef } from 'react';

const Mycomponent = forwardRef((props, ref) => {
  return (
    <div>
      <input ref={ref}></input>
    </div>
  );
});
```

- useRef는 상태 관리 목적 외에도 각 요소에 이름을 짓는 용도로도 사용할 수 있다.
- useRef를 이용해 특정 요소에 접근할 수 있다.

## 정리

- `useState`와 `useRef` 모두 상태 관리를 위해 사용할 수 있다.
- 하지만 `useState`의 경우 `state` 변화 후에 re-rendering을 진행하는 반면 `useRef`는 진행하지 않는다.
- 이 외에도 `useRef`는 이름을 지어 접근하는 용도와 생애주기 내내 변화하는 값을 가리키고 있다는 차별점도 가지고 있다.

## 참고 자료

- https://medium.com/humanscape-tech/react-usestate-vs-useref-4c20713f7ef
- https://velog.io/@skawnkk/useState-vs-useRef
- https://dev.to/trinityyi/when-to-use-useref-instead-of-usestate-3h4o