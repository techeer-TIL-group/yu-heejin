## Tree-shaking

- 사용하지 않는 코드를 삭제하는 기능
- Tree-shaking을 통해 **export했으나 아무데서도 import하지 않은 모듈이나 사용하지 않는 코드를 삭제해 번들 크기를 줄여 페이지가 표시되는 시간을 단축시킨다.**

## enum

- 열거형 변수로, 비슷한 상수를 하나로 합칠 때 편리하게 사용할 수 있다.
    
    ```tsx
    // 아무것도 지정하지 않은 경우에는 0부터 숫자를 매깁니다. 
    enum MOBILE_OS {
      IOS, // 0
      ANDROID // 1
    }
    
    // 임의의 숫자나 문자열을 할당할 수도 있습니다
    enum MOBILE_OS {
      IOS = 'iOS',
      ANDROID = 'Android'
    }
    
    // 아래와 같이 유형으로 사용할 수도 있습니다 
    const os: MOBILE_OS = MOBILE_OS.IOS
    function detectOSType(userAgent: string): MOBILE_OS {
        // 생략
    }
    ```
    
- `enum`은 타입스크립트가 자체적으로 구현하는 기능이다.
    - 자바스크립트에서는 사용할 수 없기 때문에 다음과 같이 객체로 선언한다.
        
        ```tsx
        const MOBILE_OS = {
            IOS: 'iOS',
            ANDROID: 'Android'
        }
        console.log(MOBILE_OS.IOS) // iOS
        ```
        

## Typescript에서 enum을 사용하면 Tree-shaking이 되지 않는다?

- `enum`은 편리한 기능이지만, **타입스크립트가 자체적으로 구현한 기능이기 때문에 문제가 발생한다.**

```tsx
export enum MOBILE_OS {
  IOS,
  ANDROID
}

// 문자열을 할당한 경우
export enum MOBILE_OS {
  IOS = 'iOS',
  ANDROID = 'Android'
}
```

- 위 코드를 트랜스파일하면 아래와 같은 자바스크립트 코드가 된다.
    
    ```tsx
    export var MOBILE_OS;
    (function (MOBILE_OS) {
        MOBILE_OS[MOBILE_OS["IOS"] = 0] = "IOS";
        MOBILE_OS[MOBILE_OS["ANDROID"] = 1] = "ANDROID";
    })(MOBILE_OS || (MOBILE_OS = {}));
    
    // 문자열을 할당한 경우
    export var MOBILE_OS;
    (function (MOBILE_OS) {
        MOBILE_OS["IOS"] = "iOS";
        MOBILE_OS["ANDROID"] = "Android";
    })(MOBILE_OS || (MOBILE_OS = {}));
    ```
    
- 자바스크립트에 존재하지 않는 것을 구현하기 위해 타입스크립트 컴파일러는 `IIFE(즉시 실행 함수)`를 포함한 코드를 생성한다.
- 그러나 **Rollup과 같은 번들러는 IIFE를 사용하지 않는 코드라고 판단하지 못해 Tree-shaking이 되지 않는다.**
    
    ![Untitled](https://engineering.linecorp.com/wp-content/uploads/2020/07/9fba7580-ba18-11ea-9c22-9344d1208a74-1024x512.png)
    
- 결국, **`MOBILE_OS`를 import하고 실제로는 사용하지 않더라도 최종 번들에 포함된다.**

## Enum의 대체제

### Union Types

```tsx
const MOBILE_OS = {
  IOS: 'iOS',
  Android: 'Android'
} as const;

type MOBILE_OS = typeof MOBILE_OS[keyof typeof MOBILE_OS]; // 'iOS' | 'Android'
```

- 위 코드는 아래와 같은 자바스크립트 코드로 트랜스파일된다.
    
    ```tsx
    const MOBILE_OS = {
        IOS: 'iOS',
        Android: 'Android'
    };
    ```
    
    - 타입스크립트 코드에서는 `MOBILE_OS` 타입을 정의한 이점과 동시에 자바스크립트로 트랜스파일해도 `IIFE`가 생성되지 않기 때문에 Tree-shaking을 할 수 있다.

### const enum?

- 타입스크립트의 `const enum`은 `enum`과 거의 비슷하지만, `**enum`의 내용이 트랜스파일할 때 인라인으로 확장된다는 점에서 차이가 있다.**

```tsx
const enum MOBILE_OS {
    IOS = 'iOS',
    ANDROID = 'Android',
}

const ios = MOBILE_OS.IOS
```

- 위 코드는 아래와 같은 자바스크립트 코드로 트랜스파일된다.
    
    ```tsx
    const ios = "iOS" /* IOS */;
    ```
    
    - Tree-shaking이라는 관점에서는 `Union Types`와 같다.
    - 사용하지 않으면 번들에 포함되지 않는다.
- 다만 이러한 방법은 **긴 문자열을 할당하는 경우에는 문제가 있을 수 있다.**
    
    ```tsx
    // TypeScript
    const enum NAME {
      JUGEM = '寿限無寿限無五劫の擦り切れ海砂利水魚の…', // 일본에서 '김수한무 거북이와 두루미 삼천갑자 동방삭...'과 비슷한 용도로 사용하는 긴 이름입니다.
    	TARO = '다로', 
    	JIRO = '지로', 
    } 
    const isJugem = name === NAME.JUGEM
     
    // JavaScript 트랜스파일 후
    const isJugem = name === "u5BFFu9650u7121u5BFFu9650u7121u4E94u52ABu306Eu64E6u308Au5207u308Cu6D77u7802u5229u6C34u9B5Au306Eu2026" /* JUGEM */;
    ```
    
    - const enum은 Babel로 트랜스파일할 수 없고, 타입스크립트의 —isolatedModules가 활성화된 환경에서는 큰 의미가 없다.

## 참고 자료

- https://engineering.linecorp.com/ko/blog/typescript-enum-tree-shaking