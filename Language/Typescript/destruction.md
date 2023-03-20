## 자바스크립트에서 비구조와 할당

```jsx
const { name, age, gender } = someObject;
```

## 타입스크립트에서 비구조화 할당

### 오류 코드

```jsx
const { name: string, age: number, gender: string } = someObject;
```

- 위와 같이 설정할 경우 name, age, gender를 각각 string, number, string으로 명명하는 코드

### 쉬운 방법

```jsx
const { name, age, gender }: { name: string, age: number, gender: string } = someObject;
```

- {} 로 감싸진 전체에 대해서 타입 지정

### 권장하는 방법

```jsx
interface User {
	name: string,
	age: number,
	gender: string,
}

const { name, age, gender }: User = someObject;
```

- interface/type alias로 미리 타입을 정의하고 해당 객체를 타입으로 지정하는 방법

## 참고 자료

- [https://velog.io/@modolee/typeScript-destructuring](https://velog.io/@modolee/typeScript-destructuring)