## 유닛(Unit) 타입

- 딱 세 가지만 할당하도록 제한
    
    ```tsx
    type AdminRole = "manager" | "director" | "staff";
    ```
    
- 딱 하나만 할당하도록 제한
    
    ```tsx
    type AdminRole = "manager"; // "manager" 만 가능
    type MemberCount = 8; // 8 만 가능
    
    const myRole: AdminRole = "manager"; // 다른 값을 할당하는 것은 불가능하다.
    ```
    
    - 위 코드처럼 딱 하나의 원시값만 가지는 것을 유닛(Unit) 타입이라고 한다.

### 유닛 타입 사용 시 발생할 수 있는 오류

```tsx
type AdminRole = "manager";

function printRole(role: AdminRole) {
  console.log(role);
}

printRole("manager"); // ex1) ✅ OK

let yourRole = "manager";

printRole(yourRole); // ex2) ❌ ERROR
// Argument of type 'string' is not assignable to parameter of type '"manager"'.
```

- `printRole` 함수는 오직 `“manager”` 만 받을 수 있다.
- 위 코드에서 `let`을 `const`로 바꾸면 에러는 사라진다.
    - 이는 받을 수 있는 값이 `manager` 하나로 좁혀졌기 때문이다.

## Union Type

```tsx
type Marvel = "IronMan" | "Hulk";
type Dc = "BatMan" | "SuperMan";
```

- 합집합이며, `|`로 구분하고, 자바스크립트의 `OR` 연산자와 비슷한 역할을 한다.

### 사용 예시

```tsx
type Hero = Marvel | Dc;

const hero1: Hero = "Hulk"; // ✅ OK
const hero2: Hero = "BatMan"; // ✅ OK
```

## Intersection Type

```tsx
type FavoriteSport = "swimming" | "football";
type BallSport = "football" | "baseball";

type FavoriteBallSport = FavoriteSport & BallSport; // 'football'
```

- 교집합이며, `&` 기호를 사용하여 양쪽에 모두 할당할 수 있는 값만 남긴다.
- 만약 아무것도 겹치는 것이 없다면 never 값이 된다.
    
    ```tsx
    type FavoriteSport = "swimming" | "ski";
    type BallSport = "football" | "baseball";
    
    type FavoriteBallSport = FavoriteSport & BallSport; // never
    ```
    

## 참고 자료

- [https://fe-developers.kakaoent.com/2022/221124-typescript-tip/](https://fe-developers.kakaoent.com/2022/221124-typescript-tip/)
- [https://velog.io/@soulee__/TypeScript-Union-Type](https://velog.io/@soulee__/TypeScript-Union-Type)