## Actions

```tsx
const addTodoAction = {
  type: 'todos/todoAdded',
  payload: 'Buy milk'
}
```

- **type field를 가지고 있는 plain Javascript Object**
- 단순하게 **action은 애플리케이션에서 무언가 일어나는 것을 설명한 이벤트**라고 생각할 수 있다.

### type field

- action을 묘사하는 이름(feature)을 가진 string이어야 한다.
- `“todos/todoAdded”` 처럼 사용한다.
    - 보통 `“domain/eventName”` 과 같은 형식으로 사용한다.
    - 첫번째 부분은 action이 속한 카테고리의 특징을 나타내고, 두번째 부분은 어떤 것이 발생했는지 구체적으로 나타낸 것이다.

### payload

- **action은 무엇이 일어났는지에 대한 추가적인 정보를 담고 있는 또다른 fields를 가질 수 있다.**
- convention에 따라 payload라는 이름으로 해당 정보를 작성한다.

## Action creators

```tsx
const addTodo = text => {
  return {
    type: 'todos/todoAdded',
    payload: text
  }
}
```

- **action 객체를 생성하고 반환하는 함수**
- 보통 action creator를 사용하기 때문에 매번 action 객체를 작성하지 않아도 된다.

## Reducers

```tsx
const initialState = { value: 0 }

function counterReducer(state = initialState, action) {
  // Check to see if the reducer cares about this action
  if (action.type === 'counter/increment') {
    // If so, make a copy of `state`
    return {
      ...state,
      // and update the copy with the new value
      value: state.value + 1
    }
  }
  // otherwise return the existing state unchanged
  return state
}
```

- 변화를 일으키는 함수
- 현재 state와 action 객체를 받아 **필요한 경우 상태를 업데이트하는 방법을 결정하고 새로운 상태를 반환하는 함수이다.**
    - `(state, action) ⇒ newState`

### 내부 작동 방식

1. Reducer가 action의 타입이 조건에 맞는지 체크
2. 만약 맞으면 state의 복사본을 생성하고, 복사본을 새로운 값으로 업데이트하고 반환한다.
3. 그렇지 않다면 변경되지 않는 기존의 state를 반환한다.

## Store

```tsx
import { configureStore } from '@reduxjs/toolkit'

const store = configureStore({ reducer: counterReducer })

console.log(store.getState())
// {value: 0}
```

- 현재 Redux 애플리케이션 상태는 store라는 객체에 존재한다.
- store는 Reducer를 전달하여 생성하고, 현재 state값을 반환하는 getState라는 메서드가 존재한다.

## Dispatch

```tsx
store.dispatch({ type: 'counter/increment' })

console.log(store.getState())
// {value: 1}

// 일반적으로 올바른 action을 전달하기 위해 action creators를 호출한다.
const increment = () => {
  return {
    type: 'counter/increment'
  }
}

store.dispatch(increment())

console.log(store.getState())
// {value: 2}
```

- action을 발생시키는 것
- 상태를 업데이트하는 유일한 방법은 `store.dispatch()` 메서드를 부르고 action 객체를 넘겨주는 것이다.
- **store는 reducer function을 실행시키고 새로운 state를 내부에 저장할 것**이고, getState() 메서드를 호출하여 업데이트된 값을 가질 수 있다.
- **dispatch 작업은 일종의 이벤트 트리거라고 생각할 수 있다.**
    - 무언가 발생했을 때 store는 어떤 일이 발생하는지 알고 싶어한다.
    - **reducer는 이벤트 리스너와 같은 역할을 하며 관련된 action을 받으면 response로 state를 업데이트한다.**

## Selectors

```tsx
const selectCounterValue = state => state.value

const currentValue = selectCounterValue(store.getState())
console.log(currentValue)
// 2
```

- store의 state 값에서 **특정 정보를 추출하는 방법을 알고있는 함수**이다.
- 애플리케이션이 커지면 앱의 다른 부분이 동일한 데이터를 읽는 상황이 많아지는데, 이때 반복되는 로직을 줄이는데 도움이 된다.

## Redux의 data flow

![Untitled](https://blog.kakaocdn.net/dn/48psc/btrnCPCtZcS/R3pLS0bXmZsbvVSimayeA1/img.gif)

- state는 특정 시점의 app의 상태를 나타낸다.
- UI는 state 기반으로 렌더링된다.
- 이벤트가 발생하면 state는 무엇이 발생했는지를 기반으로 업데이트 된다.
- UI가 새로운 state에 의해 리렌더링 된다.

### 초기 setup

- redux 저장소는 root reducer function을 사용하여 생성된다.
- store는 root reducer를 한 번 호출하고 반환된 초기 state 값을 저장한다.
- UI가 처음 렌더링되면 UI 컴포넌트는 Redux store의 현재 state에 액세스 할 수 있고, 해당 데이터를 사용해서 렌더링할 항목을 결정한다.
- 향후 store의 업데이트를 subscribe 하여 state가 변경되었는지 알 수 있다.

### update

- 사용자가 버튼을 클릭하는 것과 같이 앱에서 이벤트가 발생한다.
- 앱의 코드는 action을 Redux store에 dispatch 한다.
    - `dispatch({type: ‘counter/increment’})`
- store는 이전의 state와 현재 action으로 다시 reducer 함수를 실행시키고, 반환된 새로운 state를 저장한다.
- store는 subscribe된 UI의 모든 부분에 store가 업데이트 되었음을 알린다.
- store의 데이터를 필요로하는 각 UI의 컴포넌트는 필요한 state가 변경되었는지 확인한다.
- 데이터가 변경된 각각의 컴포넌트는 화면에 새로운 내용을 업데이트 할 수 있도록 새로운 데이터와 함께 강제로 리렌더링된다.

## 참고 자료

- [https://developerntraveler.tistory.com/144](https://developerntraveler.tistory.com/144)