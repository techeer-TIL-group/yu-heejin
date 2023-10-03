## 타입스크립트에서 함수 정의

```tsx
// 일반 함수
function getParam(val: number): string {
  return "매개변수의 값: " + val;
}

// 화살표 함수
let getParam = (val: number): string => {
  return "매개변수의 값: " + val;
}
```

- 타입스크립트의 함수는 자바스크립트처럼 함수를 생성할 수 있지만, **매개변수의 타입과 반환 타입을 설정해야한다.**
- 하지만 **함수의 반환 타입을 자동으로 추론**하기 때문에 함수의 반환 타입을 생략할 수 있다. (but 함수의 반환 타입을 명시하는 것이 좋다.)
    
    ```tsx
    // 일반 함수
    function getParam(val: number) {
      return "매개변수의 값: " + val;
    }
    
    // 화살표 함수
    let getParam = (val: number) => {
      return "매개변수의 값: " + val;
    }
    ```
    
- 함수가 값을 반환하지 않으면 반환 타입을 void로 정의한다.
    
    ```tsx
    function pringVal(val: number): void {
      console.log("매개변수의 값: " + val);
    }
    ```
    

## Call Signature

- 변수 또는 객체는 다음 예제처럼 타입을 미리 정의하고 선언할 수 있다.
    
    ```tsx
    // 타입을 먼저 정의
    type StringOrNumber = string | number;
    
    // 위에서 정의된 타입으로 변수 선언
    let val: StringOrNumber = 100;
    ```
    
- 마찬가지로 함수도 타입을 미리 정의할 수 있으며, **함수의 타입을 정의하는 것을 호출 시그니처(Call Signature)라고 부른다.**
- 다음은 number 타입은 2개의 매개변수와 string 값을 반환하는 함수이다.
    
    ```tsx
    function add(x: number, y: number): string {
      return "매개변수의 합:  " + (x + y);
    }
    ```
    
    - 위 함수의 호출 시그니처를 표현하면 다음과 같다.
        
        ```tsx
        (x: number, y: number) => string
        ```
        
        - 소괄호 내부에는 함수의 매개변수를 정의하고 화살표 다음에는 함수의 반환 타입을 정의한다.
        - **호출 시그니처는 함수의 본문을 포함하지 않아 타입 추론이 불가능하므로 함수의 반환 타입을 명시해야 한다.**
- 다음은 호출 시그니처를 이용해 함수의 타입을 정의 후 함수를 선언하는 예제이다.
    
    ```tsx
    type add = (x: number, y: number) => string;
    
    let addFunc: add = (x, y) => {
      return "매개변수의 합: " + (x + y);
    }
    ```
    

## 함수의 호출 방식이 하나일 때

```tsx
//단축형 호출 시그니처
type add = (a:number, b?:number) => number

//전체 호출 시그니처
type add = {
	(a:number, b?:number):number
}
```

- 함수의 호출 방식이 하나라는 것은 **파라미터 타입, 리턴 타입이 1개의 종류로 고정**되어 있다는 의미이다.
- 함수 호출 시그니처는 값이 아닌 타입 정보만 포함하며, 바디를 포함하지 않기 때문에 반환타입을 명시해야 한다.

### 함수 정의 시 한 번만 정의하기

```tsx
const add = (a: number, b: number): number => a + b;
```

### `Type`을 이용해 함수 시그니처 정의하기

```tsx
type AddType = (a: number, b: number) => number;

const addType: AddType = (a, b) => a + b;
```

### `Interface`를 이용해 함수 시그니처 정의하기

```tsx
interface AddInterface {
  (a: number, b: number): number;
}

const addInterface: AddInterface = (a, b) => a + b;
```

## 오버로딩: 함수의 호출 방식이 여러개일 때

- 오버로딩이란 메서드의 이름은 같지만 여러 케이스의 parameter를 가질 수 있는 것을 의미한다.

### 파라미터의 타입이 다를 수 있는 경우

```tsx
interface Person {
  name: string;
  age?: number;
}

interface MakePerson {
  (param: string): Person;
  (param: Person): Person;
}

const makePerson: MakePerson = (param) => {
  if (typeof param === "string") {
    return {
      name: param,
    };
  }

  return param;
};
```

- `makePerson` 함수는 `string` 문자열만 받았을 땐 해당 문자열을 `name` 프로퍼티에 넣어 `Person` 객체를 생성한다.
- 그러나 `Person` 오브젝트에 대한 정보를 받았을 경우 그대로 반환한다.

### 파라미터의 개수가 다를 수 있는 경우

```tsx
interface Add {
  (first: number, second: number): number;
  (first: number, second: number, third: number): number;
}

const add: Add = (a, b, c?: number) => {
  if (c) {
    return a + b + c;
  }

  return a + b;
};

add(1, 2); // 3
add(1, 2, 3); // 6
```

## 정리

- 함수의 반환타입은 생략 가능하나 명시하는 것을 권유한다.
- 함수의 타입을 정의하는 방법을 호출 시그니처라고 한다.

## 참고 자료

- [https://developer-talk.tistory.com/359](https://developer-talk.tistory.com/359)
- [https://jake-seo-dev.tistory.com/193](https://jake-seo-dev.tistory.com/193)