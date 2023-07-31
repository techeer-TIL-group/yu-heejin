## Object.assign() 기본 사용법

```jsx
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);   // Object { a: 1, b: 4, c: 5 }
console.log(returnedTarget === target);   // true
```

- 출처(source) 객체들의 **모든 열거 가능한 자체 속성을 복사해 대상 객체에 붙여넣고 대상 객체를 반환한다.**
- **같은 이름의 프로퍼티가 있는 경우 source 객체의 프로퍼티로 덮어쓴다.**

## Object 복사 (clone)

```jsx
let user = { firstName: 'John', lastName: 'Doe' };
let userClone = Object.assign({}, user);
```

- 객체의 복사에도 빈번히 사용된다.

## 인수가 여러개인 경우

```jsx
let user = { username: 'John' };
let userId = { id: 1 };
let email = { email: 'john@example.com' };

user = Object.assign(user, userId, email);

console.log(user);   // { username: 'John', id: 1, email: 'john@example.com' }
```

- 여러개의 인수도 합칠 수 있다.
- 같은 키가 들어있는 경우 마지막으로 추가한 객체로 덮어쓴다. (마지막 인자)

## 참고 자료

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign
- https://engineer-mole.tistory.com/151