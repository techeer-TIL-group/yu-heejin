## 인덱스 시그니처란?

- `{ [Key: T]: U }` 형식
- 객체가 여러 `Key`를 가질 수 있으며, `key`와 매핑되는 `value`를 가지는 경우 사용한다.

## 인덱스 시그니처를 사용하는 이유

- 인덱스 시그니처는 객체가 <Key, Value> 형식이며, Key와 Value의 타입을 정확하게 명시해야 하는 경우 사용할 수 있다.

### 사용 예시

- 다음과 같이 급여와 관련된 객체가 존재한다고 가정하자.
    
    ```tsx
    let objSalary {
      bouns: 200,
      pay: 2000,
      allowance: 100,
      incentive: 100
    }
    ```
    
- 객체 내부에 존재하는 속성의 값을 모두 합산해야 하는 경우 인덱스 시그니처를 사용할 수 있다.
    
    ```tsx
    function totalSalary(salary: 무슨 타입...? ) {
      let total = 0;
      for (const key in salary) {
        total += salary[key];
      }
      return total;
    }
    
    totalSalary(objSalary);
    ```
    
    - **`totalSalary()` 함수의 인자 salary는 key가 string이고 value가 number 타입인 객체만 허용해야 한다.**
    - `key`가 `string` 타입이 아닌 경우 객체의 속성을 접근하는데 문제가 발생하며, `value`가 `number` 타입이 아닌 경우 총금액을 계산하는데 문제가 발생하기 때문이다.
- 인덱스 시그니처를 사용해 다음과 같이 선언하면 된다.
    
    ```tsx
    function totalSalary(salary: {[key: string]: number}) {
      let total = 0;
      for (const key in salary) {
        total += salary[key];
      }
      return total;
    }
    ```
    

## 자바스크립트에서 인덱스 시그니처

```jsx
const dog = {
    breed: "retriever",
    name: "elice",
    bark: () => console.log("woof woof"),
};
 
dog["breed"]; // value의 key인 breed 문자열로 객체의 value 접근 => 'retriever'
```

- 객체의 특정 `value`에 접근할 때 그 `value`의 `key`를 문자열로 인덱싱해 참조하는 방법

## 타입스크립트에서 인덱스 시그니처

```tsx
interface Dictionary<T> {
    [key: number]: T;
}
 
let keys: keyof Dictionary<number>; // 숫자
let value: Dictionary<number>['foo']; // 오류, 프로퍼티 'foo'는 타입 'Dictionary<number>'에 존재하지 않습니다.
let value: Dictionary<number>[42]; // 숫자
 
// Record<K,T>
interface Dictionary<T> = Record<number, T>
```

- 자바스크립트의 **인덱스 시그니처에 대한 타입을 지정**해주는 것
- 보통 **객체에 어떤 프로퍼티들이 있는지 명확히 알 수 없을 때 사용**한다.
- `Record<K, T>`로 간편하게 지정해줄 수 있다.
    - 타입 `T`의 프로퍼티의 집합 `K`로 타입을 구성한다.

### 사용 예시

```tsx
type ArrStr = {
    [key: string]: string | number;
    [field: string]: string; // Duplicate index signature for type 'string'
    [index: number]: string; // 인덱스 시그니처에 number를 넣으면 string 타입 데이터를 참조
    length: number; 
		// 일반 프로퍼티와 공존 가능
};
```

- index 타입은 같은 것을 선언해줄 수 없다.
    - **하늘 아래(?) 같은 타입의 인덱스 시그니처는 없다.**
    - key가 string인 인덱스 시그니처는 하나만 존재 가능하다
    - 한번 더 정의하려고 할 시 타입 에러가 발생한다.
    - 다른 일반적인 프로퍼티와 공존 가능하다.
- `key`는 `object` **키의 자리를 표시하기 위한 용도**이다.
- `**ArrStr` 타입의 데이터를 인덱스 시그니처로 참조할 때 문자열을 넣으면 string 또는 number 타입 데이터를 참조한다.**
- 인덱스 시그니처에 number를 넣으면 string 값 참조

## 인덱스 시그니처 사용 방법

- 인덱스 시그니처는 속성에 타입을 선언하는 구문과 유사하지만, **속성 이름 대신 대괄호 안에 `key` 타입을 작성한다.**

### key와 value의 타입이 string

```tsx
type userType = {
  [key: string]: string;
}

let user: userType = {
  '홍길동': '사람',
  '둘리': '공룡'
}
```

### key의 타입은 string이며, value의 타입은 string, number, boolean

```tsx
type userType = {
  [key: string]: string | number | boolean;
}

let user: userType = {
  'name': '홍길동',
  'age': 20,
  'man': true
}
```

## 인덱스 시그니처 주의사항

### key의 타입은 string, number, symbol, Template literal 타입만 가능하다.

- 아래 코드처럼 key 타입을 string, number 타입이 아닌 타입으로 선언하는 경우 오류가 발생한다.
    
    ```tsx
    type userType = {
      [key: string | boolean]: string;
    }
    ```
    
- 다음 예제는 key 타입을 Template literal(유니온 타입)로 타입을 선언하는 경우이다.
    
    ```tsx
    type userInfoType = "name" | "age" | "address";
    
    type userType = {
      [key in userInfoType]: string;
    };
    
    let user: userType = {
      'name': '홍길동',
      'age': '20',
      'address': '서울'
    };
    ```
    

### 동일한 key를 여러개 가질 수 없다.

- key는 고유한 값이므로 동일한 key를 여러개 가질 수 없다.
    
    ```tsx
    type userType = {
      [key: string]: string;
    };
    
    let user: userType = {
      name: "홍길동",
      name: "또치",   // 오류 발생
      age: "20"
    };
    ```
    

## 인덱스 시그니처의 문제점

```tsx
type ArrStr = {
    [key: string]: string | number;
    [index: number]: string;
};
 
const a: ArrStr = {}; // 타입 선언
 
a["str"]; // 에러 x
```

- 타입 선언을 해주면 해당 객체를 프로퍼티로 강제로 할당해야 하는데, 그 타입에 인덱스 시그니처로만 이루어져 있으면 빈 객체로 할당해도 타입 에러가 발생하지 않는다.
    - 즉, **인덱스 시그니처만 사용할 시 타입을 표시해준 빈 객체에 에러가 나지 않는다.**
- `**key`마다 다른 타입을 가질 수 없다.**
    - `key` 타입이 `string`이면 무조건 `string` 또는 `number` 타입
- 타입에 유연함을 제공하는 대신 키 이름을 잘못 쓰는 등의 오류가 발생할 수 있다.
- 따라서 어떤 타입이 올 지 알 수 있는 상황이라면 정확한 타입을 정의하여 실수를 방지하는 것이 좋다.
- 인덱스 시그니처는 **런타임에 객체의 프로퍼티를 알 수 없는 경우에만 사용할 것을 권장한다.**

## 참고 자료

- [https://lakelouise.tistory.com/196](https://lakelouise.tistory.com/196)
- [https://developer-talk.tistory.com/297](https://developer-talk.tistory.com/297)
- [https://radlohead.gitbook.io/typescript-deep-dive/type-system/index-signatures](https://radlohead.gitbook.io/typescript-deep-dive/type-system/index-signatures)