## Record Type

```tsx
type IFieldValue = {
  name: string;
  value: number;
};

type IFormName = 'event' | 'point';

const x: Record<IFormName, IFieldValue> = {
  event: {
    name: 'foo',
    value: 0,
  },
  point: {
    name: 'foo',
    value: 30,
  }
}
```

- 두 개의 제네릭 타입을 받을 수 있다.
- `Record<Key, Type>` 형식으로 키가 Key고 값이 Type인 객체 타입이다.
    - Key는 Key값의 타입, Type은 값의 타입으로 갖는 타입을 리턴한다.

## Record Type vs Index Signature

- 타입스크립트에서 **인덱스 시그니처(Index Signature)는 대괄호로 객체를 접근하는 방법이다.**
- 다음은 인덱스 시그니처를 사용하여 객체를 생성한 예이다.
    
    ```tsx
    type humanInfo = { 
      [name: string]: number 
    };
    
    let human: humanInfo = {
      '홍길동': 20,
      '둘리': 30,
      '마이콜': 40
    };
    ```
    
- 인덱스 시그니처 예제는 Record Type으로 작성 가능하다.
    
    ```tsx
    type humanInfo = Record<string, number>
    
    let human: humanInfo = {
      '홍길동': 20,
      '둘리': 30,
      '마이콜': 40
    };
    ```
    
- 인덱스 시그니처의 단점으로 문자열 리터럴을 key로 사용하는 경우 오류가 발생한다.
    
    ```tsx
    type humanInfo = { 
      [name: '홍길동' | '둘리' | '마이콜']: number 
    };
    ```
    
- 반면에, **Record Type의 경우 Key로 문자열 리터럴을 허용한다.**
    
    ```tsx
    type names = '홍길동' | '둘리' | '마이콜';
    
    type humanInfo = Record<names, number>
    
    let human:humanInfo = {
      '홍길동': 20,
      '둘리': 30,
      '마이콜': 40
    };
    ```
    
    - 속성을 제한하고 싶은 경우 **문자열 리터럴을 사용하여 Key에 허용 가능한 값을 제한한다.**
- 타입스크립트 2.9부터 열거형을 Key로 사용할 수 있다.
    
    ```tsx
    enum names {
      "홍길동" = 1,
      "둘리" = 2,
      "마이콜" = 3
    }
    
    type humanInfo = Record<names, number>;
    
    let human: humanInfo = {
      1: 20,
      2: 30,
      3: 40
    };
    ```
    
    - 문자열 리터럴보다 코드가 깔끔하다는 장점이 있다.

## keyof와 Record Type을 같이 사용

- keyof 키워드는 타입 또는 인터페이스에 존재하는 모든 키 값을 union 형태로 가져온다.
    
    ```tsx
    type keyType = {
      a: string,
      b: number
    };
    
    // Key의 타입은 "a" | "b"
    type Key = keyof keyType;
    ```
    
- 다음은 Record Type을 keyof와 같이 사용하는 예제이다.
    
    ```tsx
    type humanType = {
      name: string;
      age: number;
      address: string;
    };
    
    type humanRecordType = Record<keyof humanType, string>;
    
    let human: humanRecordType = {
      name: "또치",
      age: "20",
      address: "서울"
    };
    ```
    
    - 기존 타입의 속성 이름을 Record Type의 Key 타입으로 하고 싶은 경우 keyof와 함께 사용할 수 있다.

## 참고 자료

- [https://developer-talk.tistory.com/296](https://developer-talk.tistory.com/296)
- [https://velog.io/@ggong/Typescript의-유틸리티-타입-1-Record-Extract-Pick](https://velog.io/@ggong/Typescript%EC%9D%98-%EC%9C%A0%ED%8B%B8%EB%A6%AC%ED%8B%B0-%ED%83%80%EC%9E%85-1-Record-Extract-Pick)