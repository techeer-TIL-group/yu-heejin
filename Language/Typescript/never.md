## never 타입

- 타입스크립트에서 `never`는 **없는 값의 집합이다.**
    - 즉, `never`타입은 값의 공집합이다.
- 절대 발생할 수 없는 타입을 나타낸다.
- 집합에 어떤 값도 없기 때문에, `never` 타입은 `any` 타입의 값을 포함해 어떤 값도 가질 수 없다.
- 위와 같은 특징 때문에 `never`는 `uninhabitable type (점유할 수 없는 타입)`, `bottom type (바닥 타입)`이라고도 불린다.
    
    ```tsx
    declare const any: any
    const never: never = any // ❌ 'any' 타입은 'never'타입에 할당할 수 없다.
    ```
    

## never가 필요한 이유?

- 숫자에서 아무것도 존재하지 않는 것을 표현하기 위해 0이 존재하는 것처럼, 타입 시스템에서도 **그 어떤 것도 불가능하다는 것을 나타내는 타입**이 필요하다.

### ‘불가능’의 의미

1. 어떤 값도 가질 수 없는 빈 타입
    - 제네릭 및 함수에서 허용되지 않는 파라미터
    - 호환되지 않는 타입 교차
    - 빈 유니언 타입 (무의 합집합)
2. 실행이 완료되면 caller에게 제어 권한을 반환하지 않는 (혹은 의도된) 함수의 반환 유형 (ex. node의 `process.exit()`)
    - void와는 다른 유형으로, void는 caller에게 아무것도 리턴하지 않는다는 것을 의미한다.
3. rejected된 promise의 fullfill 값 (거부된 프로미스에서 처리된 값의 타입)
    
    ```tsx
    const p = Promise.reject('foo') // const p: Promise<never>
    ```
    

## 유니언/교차 타입의 never 의 동작

> 숫자 0처럼, never 타입은 유니언/교차 타입에서 특별한 속성을 지닌다.
> 
1. 숫자에 0을 더하면 동일한 숫자가 나오는 것과 같이, never 타입은 유니언 타입에서 사라진다.
    
    ```tsx
    type Res = never | string // string
    ```
    
2. 숫자에 0을 곱하면 0이 나오는 것과 같이, never 타입은 교차 타입을 덮어쓴다.
    
    ```tsx
    type Res = never & string // never
    ```
    

## never 타입 사용법

### 허용할 수 없는 함수 매개변수에 제한을 가한다.

- `never` 타입을 이용해서 다양한 사용 사례에 놓인 함수에 제안을 걸 수 있다.

### switch, if-else 문의 모든 상황을 보장한다.

- 함수가 단 하나의 `never` 타입 인수만을 받을 수 있는 경우, 타입스크립트 컴파일러가 오류를 발생하지 않고는 해당 함수를 never 타입 이외의 값으로 호출할 수 없다.

```tsx
function fn(input: never) {}

// 오직 `never` 만 받는다.
declare let myNever: never
fn(myNever) // ✅

// 아무 값이나 전달하거나 아무 값도 전달하지 않으면 타입 에러 발생
fn() // ❌ 인자 'input'에 아무 값도 주어지지 않음
fn(1) // ❌ 'number' 타입은 'never' 타입에 할당할 수 없음
fn('foo') // ❌ 'string' 타입은 'never' 타입에 할당할 수 없음

// `any`도 통과할 수 없다.
declare let myAny: any
fn(myAny) // ❌ 'any' 타입은 'never' 타입에 할당할 수 없음
```

- 이러한 함수를 이용하면 `switch`, `if-else` 문의 모든 상황을 보장할 수 있다.
    - 이를 기본 케이스(default case)로 사용하면 남아있는 것은 `never` 타입이어야 하기 때문에 모든 상황에 대처하는 것을 보장할 수 있다.

```tsx
function unknownColor(x: never): never {
    throw new Error("unknown color");
}

type Color = 'red' | 'green' | 'blue'

function getColorName(c: Color): string {
    switch(c) {
        case 'red':
            return 'is red';
        case 'green':
            return 'is green';
        default:
            return unknownColor(c); // 'string' 타입은 'never' 타입에 할당할 수 없음
    }
}
```

### 타이핑을 부분적으로 허용하지 않는다.

```tsx
type VariantA = {
    a: string,
}

type VariantB = {
    b: number,
}

declare function fn(arg: VariantA | VariantB): void

const input = {a: 'foo', b: 123 }
fn(input) // 타입스크립트 컴파일러는 아무런 문제도 지적하지 않지만, 우리의 목적에는 맞지 않는다.
```

- VariantA, varianB 타입의 매개변수를 받는 함수가 있다고 했을 때, 사용자는 두 타입의 모든 속성을 모두 포함하는 하위 타입을 전달해서는 안된다.
- 매개변수에는 유니언 타입 VariantA | VarianB를 활용할 수 있다.
    - 그러나 타입스크립트의 타입 호환성은 구조적 서브 타이핑을 기반으로 하기 때문에 **객체 리터럴을 전달하지 않는 한 파라미터의 타입보다 더 많은 속성을 가진 객체를 함수에 전달하는 것이 허용된다.**
- `never`를 사용하면 구조적 타이핑을 비활성화하고 사용자가 두 속성을 모두 포함하는 객체를 전달하지 못하도록 할 수 있다.
    
    ```tsx
    type VariantA = {
        a: string
        b?: never
    }
    
    type VariantB = {
        b: number
        a?: never
    }
    
    declare function fn(arg: VariantA | VariantB): void
    
    const input = {a: 'foo', b: 123 }
    fn(input) // ❌ 속성 'a'의 타입은 호환되지 않는다.
    ```
    

### 의도하지 않은 API 사용을 방지한다.

- 데이터를 읽고 저장하는 Cache 인스턴스를 생성한다고 가정하자.
    
    ```tsx
    type Read = {}
    type Write = {}
    declare const toWrite: Write
    
    declare class MyCache<T, R> {
      put(val: T): boolean;
      get(): R;
    }
    
    const cache = new MyCache<Write, Read>()
    cache.put(toWrite) // ✅ 허용
    ```
    
- get 메서드를 통해서만 데이터를 읽을 수 있는 읽기 전용 캐시를 원할 경우, 이를 위해 put 메서드의 인수에 never를 전달해 어떤 값도 전달하지 않을 수 있다.
    
    ```tsx
    declare class ReadOnlyCache<R> extends MyCache<never, R> {} // 이제 MyCache 내부의 타입 매개변수 `T`가 `never`가 된다.
    
    const readonlyCache = new ReadOnlyCache<Read>()
    readonlyCache.put(data) // ❌ 'Data' 타입의 인자는 'never' 타입의 매개변수에 할당될 수 없다.
    ```
    

### 이론적으로 도달할 수 없는 분기를 표시한다.

- infer를 사용해 조건부 타입 내에 추가 타입 변수를 생성할 경우 모든 infer 키워드에 대해 else 분기를 추가해야 한다.
    
    ```tsx
    type A = 'foo';
    type B = A extends infer C ? (
        C extends 'foo' ? true : false// 이 표현식 내에서 'C'는 'A'를 나타낸다.
    ) : never // 이 분기는 도달할 수 없지만, 생략도 할 수 없다.
    ```
    

### 유니언 타입에서 멤버 필터링

```tsx
type Foo = {
    name: 'foo'
    id: number
}

type Bar = {
    name: 'bar'
    id: number
}

type All = Foo | Bar

type ExtractTypeByName<T, G> = T extends {name: G} ? T : never

type ExtractedType = ExtractTypeByName<All, 'foo'> // 결과 타입은 Foo
```

- `never` 타입은 **조건부 타입에서 원하지 않는 타입을 필터링할 수 있다.**
- `never` 타입은 유니언 타입에서 자동으로 제거된다.
- 특정 기준에 따라 유니언 타입에서 멤버를 선택하는 유틸리티 타입을 작성할 때, `never` 타입은 쓸모가 없기 때문에 `else` 분기에 배치하기에 완벽한 타입이다.

### 매핑된 타입의 키 필터링

```tsx
type Filter<Obj extends Object, ValueType> = {
    [Key in keyof Obj 
        as ValueType extends Obj[Key] ? Key : never]
        : Obj[Key]
}

interface Foo {
    name: string;
    id: number;
}

type Filtered = Filter<Foo, string>; // {name: string;}
```

- 타입스크립트에서 타입은 불변이다.
- 객체 타입에서 한 속성을 제거하고 싶으면, **기존 타입을 변환하고 필터링해 새로운 타입을 만들어야한다.**
- 이때, 매핑된 타입의 키를 `never`로 조건부로 다시 매핑하면 해당 키가 필터링된다.

### 제어 흐름 분석의 좁은 타입

- 함수 반환 값을 never로 설정하면 함수는 **실행이 끝났을 때 제어권을 호출자에게 반환하지 않는다.**
    - 이러한 점을 활용하면 흐름 분석을 제어해 타입을 좁힐 수 있다.
    - **함수는 몇몇 이유로 아예 혹은 절대(never) 리턴을 하지 않는다.** (모든 코드 경로에 예외를 발생시키거나, 무한 루프이거나, 프로그램에서 종료(예: Node의 `process.exit`)되는 경우 등)
- 다음 예제는 foo에 대한 유니언 타입에서 undefined를 제거하기 위해 never 타입을 반환하는 함수를 사용한다.
    
    ```tsx
    function throwError(): never {
        throw new Error();
    }
    
    let foo: string | undefined;
    
    if (!foo) {
        throwError();
    }
    
    foo; // string
    ```
    

### 호환되지 않는 타입의 불가능한 교차 타입 표시

- 호환되지 않는 타입의 교차로 never 타입을 얻을 수 있다.
    
    ```tsx
    type Res = number & string // never
    ```
    
- 아무 타입과 never를 교차해서 never 타입을 얻을 수 있다.
    
    ```tsx
    type Res = number & never // never
    ```
    

## never 타입 검사

```tsx
type IsNever<T> = T extends never ? true : false

type Res = IsNever<never> // never 🧐
```

- 실제로 Res의 타입이 never 임에도, true, false 모두 가능하다.
- 타입스크립트는 **조건부 타입에 대해 자동적으로 유니언 타입을 할당**하는데, `never`는 빈 유니언 타입이므로 할당이 발생하면 할당할 것이 없기 때문에 조건부 타입은 `never`이다.

## 참고 자료

- [https://yceffort.kr/2022/03/understanding-typescript-never](https://yceffort.kr/2022/03/understanding-typescript-never)
- [https://ui.toast.com/posts/ko_20220323](https://ui.toast.com/posts/ko_20220323)
- [https://velog.io/@hschoi1104/TypeScript-Never-Type](https://velog.io/@hschoi1104/TypeScript-Never-Type)