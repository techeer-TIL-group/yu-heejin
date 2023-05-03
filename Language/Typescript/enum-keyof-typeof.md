## 타입 추론

- 타입스크립트는 기본적으로 타입을 추론해준다.
- 만약 추론한 타입이 의도한 타입과 맞지 않으면 직접 타입을 명시해주는 것이 좋다.

### 예시 (1)

```tsx
function add(x: number, y: number) {
  return x + y;
}

const result = add(1, 2); // result의 타입은 number
```

- `return`에 대한 타입을 명시하지 않았지만, `return` 에 대한 타입을 `number`로 명시했다.
- 이러한 경우 굳이 타입을 명시할 필요가 없다.

```tsx
function add(x: number, y) {
  return x + y;
}

const result = add(1, 2); // result의 타입은 any
```

- `return`에 대한 타입을 명시하지 않았고, `y`에 대한 타입을 명시하지 않은 경우 `return` 타입이 `any`로 추론된다.
- 이러한 경우 `**y`에 대한 타입을 명시해야만 정확한 `return` 타입을 가질 수 있기 때문에 매개변수에 대한 타입 명시가 필요하다.**

### 예시 (2)

```tsx
const tp: [number, number, string] = [1, 2, "Hello"];
typeof tp; // [number, number, string]

const tp2 = [1, 2, "Hello"];
typeof tp2; // (number | string)[]
```

- tp는 [number, number, string] 형태를 가진 튜플이다.
- tp2는 타입에 대한 명시를 없앴을 때 타입이 (number | string)[]으로 추론된다.
- 의도한 튜플 형식 타입이 아니라 `array` 형식의 타입으로 추론되었으므로 명확히 명시해야 한다.

## typeof 기본 사용법

- Javascript 에서는 해당 값의 타입을 추출할 수 있는 `typeof` 연산자가 존재한다.
    
    ```tsx
    // "string"을 출력합니다
    console.log(typeof "Hello world");
    ```
    
- Typescript에서도 변수나 프로퍼티의 타입을 추론할 때 사용한다.
    
    ```tsx
    let s = "hello";
    let n: typeof s;    // let n: string
    ```
    

## 객체와 typeof

```tsx
const obj = {
   red: 'apple',
   yellow: 'banana',
   green: 'cucumber',
};

// 위의 객체를 타입으로 변환하여 사용하고 싶을때
type Fruit = typeof obj;
/*
type Fruit = {
    red: string;
    yellow: string;
    green: string;
}
*/

let obj2: Fruit = {
   red: 'pepper',
   yellow: 'orange',
   green: 'pinnut',
};
```

- 객체 데이터를 객체 타입으로 변환해주는 연산자
- 위 코드에서 **obj는 객체이기 때문에 객체 자체를 타입으로 사용할 수 없다.**
- 따라서 **만약 객체에 쓰인 타입 구조를 그대로 가져와 독립된 타입으로 만들어 사용**하고 싶은 경우 앞에 `typeof` 키워드를 명시해주면 해당 객체를 구성하는 타입 구조를 가져와 사용할 수 있다.
- 즉, **값을 type으로 사용하고 싶을 경우 사용한다.**
- 함수도 타입으로 변환하여 재사용이 가능하다.
    
    ```tsx
    function fn(num: number, str: string): string {
       return num.toString();
    }
    
    type Fn_Type = typeof fn;
    // type Fn_Type = (num: number, str: string) => string
    
    const ff: Fn_Type = (num: number, str: string): string => {
       return str;
    };
    
    // --------------------------------------------------------------
    
    class Classes {
       constructor(public name: string, public age: number) {}
    }
    
    type Class_Type = Classes;
    // type Class_Type = { name: string, age, number }
    
    const cc: Class_Type = {
       name: '임꺾정',
       age: 18888,
    };
    ```
    
    - 클래스같은 경우 **클래스 자체가 객체 타입이 될 수 있기 때문에 `typeof`를 붙이지 않아도 된다.** (인터페이스는 자체가 타입이므로 불가능하다.)

## keyof

```tsx
type Type = {
   name: string;
   age: number;
   married: boolean;
}

type Union = keyof Type;
// type Union = name | age | married

const a:Union = 'name';
const b:Union = 'age';
const c:Union = 'married';
```

- 객체 형태의 타입을 따로 **속성들만 뽑아 모아 유니온 타입으로 만들어주는 연산자**
- **object의 key 값만 가져오고 싶은 경우 사용한다.**
- 만약 obj 객체의 `key` 값인 `red, yellow, green`을 상수 타입으로 사용하고 싶을 땐 `typeof obj` 자체에 `keyof` 키워드를 붙여주면 된다.
    
    ```tsx
    const obj = { red: 'apple', yellow: 'banana', green: 'cucumber' } as const; // 상수 타입을 구성하기 위해서는 타입 단언을 해준다.
    
    // 위의 객체에서 red, yellow, green 부분만 꺼내와 타입으로서 사용하고 싶을떄
    type Color = keyof typeof obj; // 객체의 key들만 가져와 상수 타입으로
    
    let ob2: Color = 'red';
    let ob3: Color = 'yellow';
    let ob4: Color = 'green';
    ```
    
- 반대로 객체의 `value` 값을 상수 타입으로 사용하고 싶다면 다음과 같이 사용한다.
    
    ```tsx
    const obj = { red: 'apple', yellow: 'banana', green: 'cucumber' } as const;
    
    type Key = typeof obj[keyof typeof obj]; // 객체의 value들만 가져와 상수 타입으로
    
    let ob2: Key = 'apple';
    let ob3: Key = 'banana';
    let ob4: Key = 'cucumber';
    ```
    

## enum

```tsx
// enum
const enum EDirection {
  Up, // 기본적으로 0, 첫 값을 지정해줄 수 있다. (3이면 3, 4, 5, 6으로 진행된다.)
  Down, // 1
  Left, // 2
  Right, // 'Hello' 문자열도 가능
}

// object
const ODirection = {
  Up: 0,
  Down: 1,
  Left: 2,
  Right: 3,
} as const;
```

- 여러개의 변수들을 하나의 그룹으로 묶고 싶을 때 사용한다.
- enum은 자바스크립트로 변환될 때 숫자가 남지 않지만, object는 자바스크립트로 변환할 때 숫자가 남아있다.

### enum과 object를 매개변수로 사용할 때

```tsx
// Using the enum as a parameter
// enum을 타입으로 사용할 때
function walk(dir: EDirection) {}

// It requires an extra line to pull out the keys
// object를 enum처럼 타입으로 사용하고 싶을 때
// enum에 비해서 타입으로 정의되는 부분이 복잡하다.
type Direction = typeof ODirection[keyof typeof ODirection];
function run(dir: Direction) {}

walk(EDirection.Left);
run(ODirection.Right);
```

## 활용하기

### Enum 대체 상수 타입

```tsx
enum EDirection {
   Up,
   Down,
   Left,
   Right,
}

const ODirection = {
   Up: 0,
   Down: 1,
   Left: 2,
   Right: 3,
} as const;

console.log(EDirection.Left); // 2
console.log(ODirection.Right); // 3

// Enum을 타입으로 사용
function walk(dir: EDirection) {
   console.log(dir);
}

// 객체를 타입으로 사용하기 위해선 typeof 와 keyof 파라미터를 사용해야 한다
type Direction = typeof ODirection[keyof typeof ODirection];
function run(dir: Direction) {
   console.log(dir);
}

walk(EDirection.Left); // 2
run(ODirection.Right); // 3
```

- 열거형 타입 Enum을 사용하고 싶지 않으면 상수 타입으로 응용 가능하다.

### 제네릭 활용

```tsx
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };

getProperty(x, "a"); // 성공
getProperty(x, "m"); // 오류: 인수의 타입 'm' 은 'a' | 'b' | 'c' | 'd'에 해당되지 않음.
```

- 만일 함수의 매개변수 `key`가 반드시 매개변수 `obj`의 제네릭 타입 T에 존재해야할 때, `keyof T`를 사용하면 **객체의 `key`값을 모아 유니온 타입으로 만들 수 있다.**

## 참고 자료

- [https://inpa.tistory.com/entry/TS-📘-타입스크립트-keyof-typeof-사용법](https://inpa.tistory.com/entry/TS-%F0%9F%93%98-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-keyof-typeof-%EC%82%AC%EC%9A%A9%EB%B2%95)
- [https://www.typescriptlang.org/ko/docs/handbook/2/typeof-types.html](https://www.typescriptlang.org/ko/docs/handbook/2/typeof-types.html)
- [https://greatpapa.tistory.com/161?category=559680](https://greatpapa.tistory.com/161?category=559680)