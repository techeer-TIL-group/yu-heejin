```tsx
a ?? b
```

- 왼쪽 피연산자가 `null`, `undefined`일 때 오른쪽 피연산자를 반환하고, 그렇지 않으면 왼쪽 피연산자를 반환하는 연산자

## ??와 ||의 차이

- `||`는 첫번째 truthy 값을 반환한다.
- `??`는 첫번째 정의된 값을 반환한다.

### 사용 예시

```jsx
height = height ?? 100;
```

- height에 값이 정의되지 않은 경우 height엔 100이 할당된다.

```jsx
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```

- `height || 100`은 height에 0을 할당했지만, **0을 falsy한 값으로 취급하기 때문에 null이나 undefined와 동일하게 처리한다.**
- 반면, `height ?? 100`은 height가 **정확하게 null이나 undefined일 경우에만 100이 된다.**

## 연산자 우선순위

```jsx
let height = null;
let width = null;

// 괄호를 추가!
let area = (height ?? 100) * (width ?? 50);

alert(area); // 5000
```

- =, ?보다는 먼저, 대부분의 연산자보다 나중에 평가된다.

## 제약 사항

```jsx
let x = 1 && 2 ?? 3; // SyntaxError: Unexpected token '??'
```

- 안전성의 문제로 `??`는 `&&`나 `||`와 함께 사용할 수 없다.
- 사람들이 `||`를 `??`로 바꾸기 시작하면서 만드는 실수를 방지하고자 명세서에 제약이 추가되었기 때문이다.
- 제약을 피하려면 괄호를 쓰면 된다.
    
    ```jsx
    let x = (1 && 2) ?? 3; // 제대로 동작합니다.
    
    alert(x); // 2
    ```
    

## 참고 자료

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing
- https://ko.javascript.info/nullish-coalescing-operator
