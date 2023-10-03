## 자바스크립트에서 비구조와 할당

```jsx
const { name, age, gender } = someObject;
```

## 타입스크립트에서 비구조화 할당

### 오류 코드

```tsx
const { name: string, age: number, gender: string } = someObject;
```

- 위와 같이 설정할 경우 name, age, gender를 각각 string, number, string으로 명명하는 코드

### 쉬운 방법

```tsx
const { name, age, gender }: { name: string, age: number, gender: string } = someObject;
```

- {} 로 감싸진 전체에 대해서 타입 지정

### 권장하는 방법

```tsx
interface User {
	name: string,
	age: number,
	gender: string,
}

const { name, age, gender }: User = someObject;
```

- interface/type alias로 미리 타입을 정의하고 해당 객체를 타입으로 지정하는 방법

## 비구조화 할당 사용 시 주의할 점

- 타입 선언 시 속성 키 값 우측에 ?를 포함할 경우 선택적 속성으로 만들 수 있다.
    
    ```tsx
    interface TestType {
      a: number
      b?: number
    }
    
    const object: TestType = {
      a: 1,
      b: 2,
    }
    
    const { a, b } = object
    ```
    
    - 이 경우 선언한 타입 외에 undefined 타입도 함께 union 타입으로 자동 세팅된다.
- 그러나 만약 **커스텀 타입 안에서 또 다른 커스텀 타입을 사용하고, 이를 또 다시 선택적 속성으로 선언할 경우 해당 타입에 비구조화 할당 문법을 적용할 경우 문제가 발생**할 수 있다.
    
    ```tsx
    interface TestTypeSub {
      a_sub: number
      b_sub: number
    }
    
    interface TestType {
      a: number
      b?: number
      c?: TestTypeSub
    }
    
    const object: TestType = {
      a: 1,
      b: 2,
      c: {
        a_sub: 3,
        b_sub: 4,
      },
    }
    
    const { a_sub, b_sub }: TestTypeSub = object.c
    
    // 다음과 같은 오류가 발생한다.
    // Type 'TestTypeSub | undefined' is not assignable to type 'TestTypeSub'.
    // Type 'undefined' is not assignable to type 'TestTypeSub'.
    ```
    
    - object.c는 선택적 속성이라 undefined일 수 있는데, 어떻게 그 안에서 a_sub와 b_sub라는 값을 추출하냐는 오류
    - 이 경우 아래와 같이 **Nullish 병합 연산자**를 사용하면 된다.
        
        ```tsx
        const { a_sub, b_sub }: TestTypeSub = object.c ?? {
          a_sub: 3,
          b_sub: 4
        }
        ```
        
        - **object.c가 유효하지 않은 데이터일 경우 할당할 default 값을 지정해주는 것이다.**
        - 이와 비슷한 역할을 하는 OR 연산자 ( || )도 존재한다.

### [추가] OR, Nullish의 차이점

- 두 연산자의 차이점은 유효하지 않은 데이터로 판단하는 기준이다.
- Nullish 병합 연산자의 경우 null, undefined만 유효하지 않은 데이터로 취급한다.
- OR 연산자의 경우 null, undefinded와 함께 false, 0 같은 falsy 값도 유효하지 않은 데이터로 취급한다.

## 참고 자료

- [https://velog.io/@modolee/typeScript-destructuring](https://velog.io/@modolee/typeScript-destructuring)
- [https://findmypiece.tistory.com/228](https://findmypiece.tistory.com/228)