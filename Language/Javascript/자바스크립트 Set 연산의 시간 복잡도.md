## 삽입

```jsx
let exampleSet = new Set();
exampleSet.add(1); // exampleSet: Set {1}
exampleSet.add(1); // exampleSet: Set {1}
exampleSet.add(2); // exampleSet: Set {1, 2}
```

- 집합에는 중복되는 항목들을 추가할 수 없다.
- **시간 복잡도는 O(1)**

## 삭제

```jsx
let exampleSet = new Set();
exampleSet.add(1); // exampleSet: Set {1}
exampleSet.delete(1); // true
exampleSet.add(2); // exampleSet: Set {2}
```

- 해당 항목이 존재해서 삭제되었다면 true, 해당 항목이 존재하지 않으면 false 반환
- 배열에서 항목 하나를 삭제하려면 O(n)의 시간이 걸리지만, **집합의 경우 O(1)**

## 포함 여부

```jsx
let exampleSet = new Set();
exampleSet.add(1); // exampleSet: Set {1}
exampleSet.has(1); // true
exampleSet.has(2); // false
```

- **시간 복잡도 O(1)**

## 참고 자료

- [https://progyu.github.io/2019/10/29/191029-TIL-JS-in-Set/](https://progyu.github.io/2019/10/29/191029-TIL-JS-in-Set/)