```tsx
const names = ["Song", "James", "Mike"] as const;
type Name = typeof names[number];

// type Name: "Song", "James", "Mike"
```

## 패턴 분석

```tsx
const names = ["Song", "James", "Mike"] as const
```

- `readonly` 상수값, 즉 **불변한 값으로 변경**하기 위해 `as const` 사용

```tsx
type Name = typeof names[number];
// type Name: "Song" | "James" | "Mike"
```

- **배열 안에 있는 숫자로 이루어진 모든 index 값을 가져와 유니온 타입**으로 만들어준다.
    - index signature 문법 참고

```tsx
const people = [
  { age: 20, name: 'Song' },
  { age: 21, name: 'James' },
  { age: 22, name: 'Mike' }
] as const

type Name = typeof animals[number]['name']
```

- 배열 안에 객체가 있을 경우 다음과 같이 사용한다.

## const assertions (as const)

```tsx
const name = "Song";
// const name: "Song"

let name2 = "Song"
// let name2: string

let name3 = "Song" as const
// let name3: "Song"
```

- `as const`는 **선언한 변수들을 마치 `const`처럼 만들어준다.**
- `const`로 변수를 선언하면 타입은 `“Song”`이지만, `let`으로 선언한 경우 `string`이 된다.

```tsx
const abcd = ["a", "b", "c", "d"];
// const abcd: string[]
abcd.push("e"); // o

const ABCD = ["A", "B", "C", "D"] as const;
// const ABCD: readonly ["A", "B", "C", "D"]
ABCD.push("E"); // Property 'push' does not exist on type 'readonly ["A", "B", "C", "D"]'
```

- `const`로 선언한 객체는 객체 내부의 타입들이 넓은 범위의 타입으로 추론된다.
- 이때, const assertion을 사용하면 그 범위를 좁혀준다.

## 참고 자료

- [https://talkwithcode.tistory.com/101](https://talkwithcode.tistory.com/101)