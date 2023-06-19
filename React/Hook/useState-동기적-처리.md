- `useState`나 `Hook`은 비동기로 실행되는 함수이기 때문에 **업데이트 직후의 상태값을 가져올 수 없다.**
- 페이지를 구성하는데 수많은 `state`가 존재하는데, **만약 하나하나의 `state` 변화에 리랜더링을 발생시킨다면 성능 저하가 발생할 것이다.**
- `React`는 `setState`가 연속 호출되면 `batch` 처리를 통해 한번에 렌더링되게 한다.
    - 즉, 많은 `setState`를 연속으로 사용해도 **배치 처리로 인해 렌더링은 한번만 된다.**
    - `batch`란 `React`가 여러개의 `state` 업데이트를 하나의 리렌더링으로 묶는 것을 의미한다.
    - 16ms동안 변경된 상태 값들을 하나로 묶는다.

## useState를 동기적으로 처리하려면?

- `useState`를 동기적으로 사용하려면 `useEffect`를 사용해야 한다.
    - `useEffect`의 `dependency`에 원하는 `state`를 넣어두고, 그 값이 변경되면 `useEffect` 내부의 함수들이 실행되는 원리를 이용할 수 있다.
- 혹은 setState의 인자로 함수를 집어넣는 방법도 있다!
    
    ![Untitled](https://velog.velcdn.com/images%2Falstnsrl98%2Fpost%2Ffd5b419d-e3a9-4057-9170-f01a46bd5cea%2Fcode.png)
    
    - `setState` 내 함수의 매개변수로 **이전 상태가 들어온다.**

## 참고 자료

- [https://velog.io/@tilsong/useState는-왜-바로-상태를-변화시키지-않을까](https://velog.io/@tilsong/useState%EB%8A%94-%EC%99%9C-%EB%B0%94%EB%A1%9C-%EC%83%81%ED%83%9C%EB%A5%BC-%EB%B3%80%ED%99%94%EC%8B%9C%ED%82%A4%EC%A7%80-%EC%95%8A%EC%9D%84%EA%B9%8C)
- [https://velog.io/@rkio/React-useState는-비동기이다.-동기-처리하려면](https://velog.io/@rkio/React-useState%EB%8A%94-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%9D%B4%EB%8B%A4.-%EB%8F%99%EA%B8%B0-%EC%B2%98%EB%A6%AC%ED%95%98%EB%A0%A4%EB%A9%B4)
- [https://velog.io/@alstnsrl98/useState는-동기-비동기-동기적-처리](https://velog.io/@alstnsrl98/useState%EB%8A%94-%EB%8F%99%EA%B8%B0-%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%8F%99%EA%B8%B0%EC%A0%81-%EC%B2%98%EB%A6%AC)