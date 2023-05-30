## Type Annotation

- Annotate는 ‘주석을 달다’의 의미를 가진다.
- 타입스크립트에서는 변수, 함수, 함수 반환값의 데이터 타입을 지정하기 위해 ‘타입 어노테이션’을 사용한다.
    - 즉, **타입에 주석을 단다.**
- **한 번 식별자를 특정 타입으로 annotated하면 해당 타입만 사용할 수 있다.**

## 사용 방법

변수명 뒤에 `:`를 붙인 후 타입명을 지정하면 된다.

### 변수에 타입 지정하기

```tsx
let a: number;
//a라는 변수에는 number 타입의 값만 할당 및 재할당할 수 있다.

a = "Mark"; //error
a = 23; //correct

let a = "Joah"
//a에 첫 할당으로 문자열을 할당하면 a에 재할당시 문자열만 재할당할 수 있다.

a = 23; //error
a = "good" //correct
```

### 함수에 타입 지정하기

```tsx
function hello(a: number){
  console.log(a)
}
hello(33)
//correct

function hello(a: number){
  console.log(a)
}
hello("Joah")
//error
```

## 참고 자료

- https://velog.io/@joahkim/TypeScript-Type-Annotation
- https://nanggi-hl.tistory.com/218