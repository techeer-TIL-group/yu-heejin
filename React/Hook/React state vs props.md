## state

- 일반적으로 **컴포넌트의 내부에서 변경 가능한 데이터를 관리**해야할 때 사용한다.
- 컴포넌트 내에서 **지속적으로 변경이 일어나는 값**을 관리하기 위해 사용된다.
- 개발자가 의도한 동작에 의해 변할 수도 있고, 사용자의 입력에 따라 새로운 값으로 변경될 수도 있다.
- **값을 저장하거나 변경할 수 있는 객체로 보통 이벤트와 함께 사용**한다.
- 컴포넌트에서 **동적인 값을 state(상태)라고 부르며, 동적인 데이터를 다룰 때 사용**된다.
- state값이 변경되고 재렌더링이 필요한 경우 React가 자동으로 계산하여 변경된 부분을 렌더링한다.

### state 사용하기

```jsx
import { useState } from "react";
 
const App = () => {
    const [value, setValue] = useState(초기값);
    return ...
}
```

- state의 값 변경을 위해 setState hook을 사용한다.
- state값을 임의로 변경해서는 안된다.

### state값을 직접 변경하지 말 것

```jsx
import { useState } from 'react';
 
function Example() {
    const [count, setCount] = useState(0);
    return (
        <div>
            <p>버튼을 {count}번 눌렀습니다.</p>
            <button onClick={() => {count = count + 1;}>클릭</button> // 이렇게 하면 안 된다.
        </div>
    );
}
```

- state 값을 직접 변경하게 되면 React가 Component를 다시 렌더링할 타이밍을 알아차리지 못한다.
- 따라서 반드시 setState 함수를 이용해 값을 변경해야한다.

### Object, Array를 갖는 state를 다룰 때 주의사항

```jsx
const [user, setUser] = useState({name: "민수", grade: 1})
 
setUser((current) => {
    current.grade = 2; // 이렇게 하면 안 된다.
    return current;
})
```

- 위 코드의 경우 React가 State의 변경을 감지하지 못한다.
- user object내의 값이 변경되었지만, user 자체는 변경되지 않았기 때문이다.

```jsx
const [user, setUser] = useState({name: '민수', grade: 1 })
 
setUser((current) => {
    const newUser = { ...current }
    newUser.grade = 2
    return newUser
})
```

- 기존 user의 내용을 새로운 object에 담은 후 내부 값을 변경하면 된다.

## props

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

- 상위 컴포넌트가 하위 컴포넌트에 값을 전달할 때 사용한다.
    - 단방향 데이터 흐름을 갖는다.
- **프로퍼티는 수정할 수 없다는 특징**이 있다.
    - 자식 컴포넌트의 입장에서는 읽기 전용 데이터이다.

### props 사용하기

```jsx
// 1. 문자열을 넘기는 경우
import React, { Component } from 'react';
import Header from './component/Header';
import Footer from './component/Footer';
import Main from './component/Main';

function App() {
  return (
    <div>
      <Header />
      <Main name="감자" />
      <Footer />
    </div>
  );
}

export default App;

// 2. 숫자를 넘기는 경우
import React, { Component } from 'react';
import Header from './component/Header';
import Footer from './component/Footer';
import Main from './component/Main';

function App() {
  return (
    <div>
      <Header />
      <Main name={9} />
      <Footer />
    </div>
  );
}

export default App;

import React from 'react';

function Main(props) {
    return (
        <div>
            <main>
                <h1>안녕하세요. {props.name} 입니다.</h1>
            </main>
        </div>
    );
}

export default Main;
```

- 프로퍼티에 문자열을 전달할 때는 `“”`를, 문자열 외의 값을 전달할 땐 `{}`를 사용한다.

```jsx
import React, { Component } from 'react';
import Header from './component/Header';
import Footer from './component/Footer';
import Main from './component/Main';

function App() {
  return (
    <div>
      <Header />
      <Main name="감자" color="brown"/>
      <Footer />
    </div>
  );
}

export default App;

import React from 'react';

function Main(props) {
    return (
        <div>
            <main>
                <h1 style={{color: props.color}}>안녕하세요. {props.name} 입니다.</h1>
            </main>
        </div>
    );
}

export default Main;
```

- 이처럼 N개의 프로퍼티 전달도 가능하다.
- javascript의 비구조화 할당 문법도 사용 가능하다.
    
    ```jsx
    import React from 'react';
    
    function Main({name, color}) { // props 대신 비구조화 할당
        return (
            <div>
                <main>
                    <h1 style={{color}}>안녕하세요. {name} 입니다.</h1>
                </main>
            </div>
        );
    }
    
    export default Main;
    ```
    

### 프로퍼티의 자료형, 타입 정의

```jsx
import React from 'react';
import PropTypes from 'prop-types' // 프로퍼티 타입을 지정해주기 위해 사용 한다.

function Main({name, color}) {
    return (
        <div>
            <main>
                <h1 style={{color}}>안녕하세요. {name} 입니다.</h1>
            </main>
        </div>
    );
}

// 프로퍼티 타입 지정
Main.propTypes = {
  name: PropTypes.string
}

export default Main;
```

- 프로퍼티의 자료형을 미리 선언할 수 있다.
    - 타입스크립트와 비슷하다.
- 리액트 엔진이 프로퍼티로 전달하는 값을 효율적으로 알 수 있고, 버그 예방에도 도움이 된다.
- react에서 제공하는 props-types을 사용하여 각각의 자료형을 선언하면 된다.

### 프로퍼티 기본값 및 필수값 설정

```jsx
import React from 'react';
import PropTypes from 'prop-types' // 프로퍼티 타입을 지정해주기 위해 사용 한다.

function Main({name, color}) {
    return (
        <div>
            <main>
                <h1 style={{color}}>안녕하세요. {name} 입니다.</h1>
            </main>
        </div>
    );
}

// 프로퍼티 타입 지정
Main.propTypes = {
  name: PropTypes.string
}

// 프로퍼티 기본값 지정
Main.defaultProps = {
  name: '디폴트'
}

// 프로퍼티 타입 지정 및 필수값 설정
Main.propTypes = {
  name: PropTypes.string.isRequired,
}

export default Main;
```

### props.children으로 태그 사이의 값 가져오기

## state와 props 차이점

- `props`와 `state`는 일반 자바스크립트 객체이다.
- 두 객체 모두 렌더링 결과물에 영향을 주는 정보를 갖고 있지만, 한 가지 방식에서 차이가 있다.
    - `props`는 **함수 매개변수처럼 컴포넌트에 전달**되는 반면, `state`는 **함수 내에 선언된 변수처럼 컴포넌트 안에서 관리된다.**

## 참고 자료

- https://onlyfor-me-blog.tistory.com/463
- https://goddaehee.tistory.com/301
- https://lakelouise.tistory.com/260
- https://goddaehee.tistory.com/300
- https://reactjs-kr.firebaseapp.com/docs/components-and-props.html
- https://im-designloper.tistory.com/80