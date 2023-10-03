`reduce()`는 배열 형태로 되어있는 정보를 하나의 정보로 합칠 때 굉장히 유용하다.

```jsx
const nums = [1, 2, 3, 4, 5]
const sum = nums.reduce((acc, n) => acc + n, 0) // 15
```

## reduce와 …

- 배열 속 객체의 값을 키로 갖는 하나의 새로운 객체를 만들 때 `reduce()`는 매우 유용하게 쓰인다.
    
    ```jsx
    const menuList = [
      { id: 'tpk', name: '떡볶이' },
      { id: 'zzm', name: '짜장면' },
      { id: 'kb', name: '김밥' }
    ]
    
    const menuObject = menuList
      .reduce(
        (acc, item, index) => 
          ({ ...acc, [item.id]: { name: item.name, index }}),
        {}
      )
    // 출력값
    // {
    //   "tpk": {
    //     "name": "떡볶이",
    //     "index": 0
    //   },
    //   "zzm": {
    //     "name": "짜장면",
    //     "index": 1
    //   },
    //   "kb": {
    //     "name": "김밥",
    //     "index": 2
    //   }
    // }
    ```
    
    - `reduce()`를 `Spread Operator`와 함께 사용하면 깔끔하게 Object를 하나로 합칠 수 있다.

## Spread Operator

```jsx
// Shallow Clone (excluding prototype)
let aClone1 = { ...a };

// Desugars into:
let aClone2 = Object.assign({}, a);
```

- `Spread Operator`는 `Object.assign()`에 대한 Syntex sugar이다.
- 결과적으로 두 연산을 같이 사용하면 **O(n^2)의 시간이 소요된다.**
    - 이중반복문을 사용하는 것과 같은 효율을 보여준다.
- `Spread Operator`는 **내부적으로 값을 복사하면서 연산하기 때문에 성능이 좋지 않다!**

### Syntax Sugar

- js 뿐만 아니라 프로그래밍 언어 전반적으로 적용되는 개념
- 읽는 사람 또는 작성하는 사람이 편하도록 디자인된 문법
- 직관적이고 간결한 문법을 갖고 있어 코드가 짧아지며, 가독성이 좋아지는 효과가 있다.

### Object.assign()

```jsx
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// Expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget === target);
// Expected output: true
```

- `Object.assign()` 메서드는 출처 객체들의 모든 열거 가능한 자체 속성을 복사해 대상 객체에 붙여넣은 후 대상 객체를 반환한다.
- 객체를 복사할 때 해당 객체가 가지고 있는 **키 값을 배열 형태로 만들고 순회하면서 새로운 객체에 저장한다.**
- **소스 객체의 속성을 순환하면서 각 속성을 타겟의 객체에 하나씩 복사해서 넣는다.**

## 참고 자료

- https://eggplantiny.github.io/blog/articles/do-not-use-reduce-and-object-spread/
- https://dkje.github.io/2020/09/02/SyntaxSugar/
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign
- https://dinn.github.io/javascript/js-reduce-spread/
- https://yceffort.kr/2021/06/reduce-spread-anti-pattern