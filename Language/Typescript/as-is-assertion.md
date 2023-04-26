## as

```tsx
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
```

- 컴파일 단계에서 타입 검사를 할 때 타입스크립트가 감지하지 못하는 애매한 타입 요소들을 직접 명시해주는 키워드
- 만약 위 코드에서 `as HTMLCanvasElement` 가 없다면, 타입스크립트는 그저 `myCanvas`가 여러 `HTMLElement` 중 어느 하나의 `HTMLElement`만을 가질 것이라는 사실 밖에 알지 못한다.
- `as HTMLCanvasElement`를 적어줌으로써 `myCanvas`가 `HTMLCanvasElement`를 가진다는 것을 알게 된다.
- `as` 키워드는 **오직 컴파일 단계에서만 실행**되며, 런타임 단계에서는 삭제된 채로 실행된다.
    - 즉, **컴파일 단계에서 제대로 넘어갔다면 런타임 단계에서의 에러 발생은 일어나지 않는다는 의미!**

## is

```tsx
function isString(test: any): test is string{
    return typeof test === "string";
}

function example(foo: any){
    if(isString(foo)){
        console.log("it is a string" + foo);
        console.log(foo.length); // string function
    }
}
example("hello world");
```

- `isString()`을 살펴보면, return문의 값이 true일 때 `test is string`이란 type predicate 구문이 있기 때문에 **타입스크립트는 해당 함수를 호출한 범위 내에서 isString()의 결과 타입을 string으로 좁힌다.**
- `isString()` 함수에서 return이 true이면, **함수가 호출된 범위 내에서는 test를 string 타입으로 보라는 의미**
- as와 마찬가지로 컴파일 단계에서만 사용되며, 런타임 단계에서는 순수한 js 파일과 동일하게 동작한다.

## 타입스크립트 타입 단언(Type Assertion)

```tsx
// as
let someValue: unknown = "this is a string";
let strLength: number = (someValue as string).length;
 
// 꺾쇠(Angle bracket), 타입 단언
let someValue: unknown = "this is a string";
let strLength: number = (<string>someValue).length; // unknown 타입이지만 Type Assertion을 통해서 타입 추론을 가능하게 해준다.
```

- **타입스크립트가 추론하지 못하는 타입을 as 키워드를 통해 명시해주는 것**
- 실제 데이터 타입을 변경하지는 않고, 타입 에러가 나지 않게끔만 해준다.

## Type Casting vs Type Assertion

- Type Casting: 데이터의 타입을 변환 → 실제 데이터 타입 변환
- Type Assertion: 데이터의 타입을 명시 → 데이터 타입에 영향을 주지 않음

## 타입 선언(Declaration) vs 타입 단언(Assertion)

```tsx
type Duck = {
    꽥꽥: () => void;
    헤엄: () => void;
};
 
const duck = {} as Duck; // 타입 단언
 
// 타입 선언
const duck: Duck = {
    꽥꽥() {
        console.log("꽥꽥");
    },
    헤엄() {
        console.log("어푸어푸?");
    },
};
```

- 타입 단언: 타입스크립트가 개발자의 말만 믿고 해당 타입으로 인식해 빈 객체임에도 에러를 내지 않음
- 타입 선언: 객체 프로퍼티를 모두 채우도록 강제하기 때문에 실수를 저지를 위험이 낮다.

## 참고 자료

- [https://velog.io/@kwak1539/타입스크립트-is-as-문법-정리](https://velog.io/@kwak1539/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-is-as-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%A6%AC)
- [https://lakelouise.tistory.com/195](https://lakelouise.tistory.com/195)
- [https://ahnheejong.gitbook.io/ts-for-jsdev/06-type-system-deepdive/type-assertion](https://ahnheejong.gitbook.io/ts-for-jsdev/06-type-system-deepdive/type-assertion)