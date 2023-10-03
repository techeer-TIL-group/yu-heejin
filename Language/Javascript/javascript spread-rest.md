## Spread 연산자

- ‘펼치다’, ‘퍼뜨리다’의 의미
- 해당 문법을 사용하면 객체 혹은 배열을 펼칠 수 있다.
    - 객체나 배열을 통째로 끌고와서 사용 가능
- 기존의 것은 건드리지 않고 새로운 객체를 만들 때 사용한다.

### 사용 예시

```tsx
// Spread 사용 예) 객체의 경우
const a = {
  one: '하나',
  two: '둘',
}

const b = {
  ...a,
  three: '셋'
}

console.log(b); // { one:'하나', two:'둘', three:'셋' }
```

```tsx
// Spread 사용 예) 배열의 경우
const a = [1, 2, 3]
const b = [...a, 999, ...a]

console.log(b); // [1, 2, 3, 999, 1, 2, 3]
```

### 주의할 점

```tsx
const arr = [1, 2, 3, 4, 5];

console.log(arr); // [ 1, 2, 3, 4, 5 ]
console.log(...arr); // 1, 2, 3, 4, 5
console.log(1, 2, 3, 4, 5); // 1, 2, 3, 4, 5
```

```tsx
const arr = [1, 2, 3, 4, 5];
const a = [...arr];

console.log(a) // [1, 2, 3, 4, 5];
```

- spread를 사용하면 배열이 아닌 **개별적인 요소**가 나온다

## Rest 파라미터

- Rest 파라미터(나머지 매개변수) 구문을 구조 분해 할당 문법과 같이 사용하면 나머지(객체, 배열)를 rest에 담에서 추출할 수 있다.
- Rest 파라미터 구분을 사용하면 함수에서 정해지지 않은 수의 매개변수를 배열로 받을 수 있다.
- 객체, 배열, 함수의 매개변수에서 사용 가능하다.
- 객체와 배열에서 사용할 땐 구조 분해 할당 문법과 함께 사용된다.
- spread의 반대라고 생각하면 편한데, **spread는 배열을 개별적으로 전개하지만 Rest 파라미터는 개별을 배열로 묶어준다.**
    
    ```tsx
    function func(...param) {
      console.log(param);
    }
    
    func(1, 2, 3); // [ 1, 2, 3 ]
    ```
    

### 사용 예시

- 객체에서의 Rest
    
    ```tsx
    // Rest 사용 예) 객체의 경우 1
    const a = {
      one: '하나',
      two: '둘'
      three: '셋'
    }
    
    const { three, ...rest } = a;
    
    console.log(three); // '셋'
    console.log(rest); // { one:'하나', two:'둘' }
    
    // rest 안에는 three 값을 제외한 값이 들어있다.
    ```
    
- 배열에서의 Rest
    
    ```tsx
    // Rest 사용 예) 배열의 경우 1
    const a = [1, 2, 3];
    
    const [ one, ...rest ] = a;
    
    console.log(one); // 1
    console.log(rest); // [2, 3]
    ```
    
- 다음과 같이 사용하면 오류가 발생한다.
    
    ```tsx
    // Rest 사용 예) 오류남
    const a = [1, 2, 3];
    
    const [ ...rest, last ] = a;
    ```
    
- 함수 매개변수에서의 Rest
    
    ```tsx
    // Rest 사용 예) 함수 파라미터 1
    function sum(...rest) {
      return rest;
    }
    
    const result = sum(1,2,3);
    
    console.log(result); // [1, 2, 3]
    ```
    
    - 함수의 매개변수가 몇 개가 될 지 모르는 상황에서 rest 매개변수를 사용하면 유용하다.
    - result가 가리키고 있는 것은 함수에서 받아온 매개변수들로 이루어진 배열이다.
        
        ```tsx
        // Rest 사용 예) 함수 파라미터 2
        function sum(...rest) {
          return rest.reduce((acc,crr) => {
            return acc + crr;
          }, 0);
        }
        
        const result = sum(1,2,3);
        
        console.log(result); // 6
        ```
        
        - 즉, 매개변수가 배열 형식으로 들어온다.

## 참고 자료

- [https://hanamon.kr/javascript-spread-reat/](https://hanamon.kr/javascript-spread-reat/)
- [https://velog.io/@chlwlsdn0828/Js-Spread-연산자-Rest-파라미터](https://velog.io/@chlwlsdn0828/Js-Spread-%EC%97%B0%EC%82%B0%EC%9E%90-Rest-%ED%8C%8C%EB%9D%BC%EB%AF%B8%ED%84%B0)