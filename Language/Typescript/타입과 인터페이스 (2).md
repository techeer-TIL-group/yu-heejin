## 확장하는 방법(상속)

### interface

```tsx
interface AnimalInterface {
  species: string
  height: number
  weight: number
}

interface AfricaAnimal extends AnimalInterface {
  continent: string
}
```

- `extends`로 확장한다.

### type

```tsx
type AnimalType = {
  species: string,
  height: number,
  weight: number
}

type AfricaAnimal = AnimalType & {
  continent: string
}
```

- 특수문자 `&`로 확장한다.

## 선언적 확장

```tsx
interface Animal {
  weight: number;
}

interface Animal {
  height: number;
}

const tiger: Animal = {
  weight: 100,
  height: 200,
};

type _Animal = {
  weight: number;
};

type _Animal = {//error : 식별자가 중복됨
  height: string;
};
```

- `type`은 새로운 속성을 추가하기 위해 다시 같은 이름으로 선언할 수 없지만, `interface`는 항상 선언적 확장이 가능하다.

## interface는 객체에만 사용 가능하다.

### 원시 타입(Primitive Types)

```tsx
{
  type CustomString = string;
  const str: CustomString = '';

  // ❌
  interface CustomStringByInterface = string;
}
```

- 타입은 원시 타입을 정의할 수 있으나, 인터페이스는 불가능하다.

### 유니온 타입(Union Types)

```tsx
{
  type Fruit = 'apple' | 'lemon';
  type Vegetable = 'potato' | 'tomato';

  // 'apple' | 'lemon' | 'potato' | 'tomato'
  type Food = Fruit | Vegetable;
  const apple: Food = 'apple';
}
```

- 유니온 타입은 타입만 사용 가능하다.

### 튜플 타입(Tuple Types)

```tsx
{
  type Animal = [name: string, age: number];
  const cat: Animal = ['', 1];
}
```

- 튜플 타입은 타입으로만 정의 가능하다.

### 객체/함수 타입(Object/Function Types)

```tsx
{
  // 인터페이스를 사용할 때, 같은 이름의 인터페이스는 자동 병합된다.
  interface PrintName {
    (name: string): void;
  }

  interface PrintName {
    (name: number): void;
  }

  // ✅
  const printName: PrintName = (name: number | string) => {
    console.log('name: ', name);
  };
}

{
  // 타입을 사용할 때, 그것은 유일 해야하고, 오직 &를 사용해야만 병합 가능하다.
  type PrintName = ((name: string) => void) & ((name: number) => void);

  // ✅
  const printName: PrintName = (name: number | string) => {
    console.log('name: ', name);
  };
}
```

- 인터페이스와 타입 모두 객체 타입이나 함수 타입을 선언할 수 있다.
- 하지만, **인터페이스의 경우 같은 인터페이스를 여러번 선언할 수 있고, 자동으로 병합된다.**
- 반면 **타입은 병합되지 않고 유니크해야한다.**

```tsx
{
  interface Parent {
    printName: (**name: number**) => void;
  }

  // ❌ 인터페이스 'Child'는 인터페이스 'Parent'를 잘못 확장했다.
  interface Child extends Parent {
    printName: (**name: string**) => void;
  }
}

{
  type Parent = {
    printName: (**name: number**) => void;
  };

  type Child = Parent & {
    // 여기서 두 printName은 intersection 된다.
    // 이것은 `(name: number | string) => void`과 같다.
    printName: (**name: string**) => void;
  };

  const test: Child = {
    printName: (name: number | string) => {
      console.log('name: ', name);
    },
  };

  test.printName(1);
  test.printName('1');
}
```

- 인터페이스를 상속할 때 서브타입은 슈퍼타입과 충돌할 수 없고, 오직 확장만 가능하다.

```tsx
{
  interface Parent {
    printName: (name: number) => void;
  }

  interface Child extends Parent {
    // ✅
    printName: (name: string | number) => void;
  }
}
```

- 인터페이스는 extends를 사용하여 상속을 구현하고, 타입은 &를 이용해 교차(intersection)을 구현한다.

### 매핑된 객체 타입(Mapped Object Types) / computed value

```tsx
type names = 'firstName' | 'lastName'

type NameTypes = {
  [key in names]: string
}

const yc: NameTypes = { firstName: 'hi', lastName: 'yc' }

interface NameInterface {
  // error
  [key in names]: string
}
```

- `type`은 가능하지만 `interface`는 불가능하다.

```tsx
type Vegetable = 'potato' | 'tomato';

{
  type VegetableOption = {
    [Property in Vegetable]: boolean;
  };

  const option: VegetableOption = {
    potato: true,
    tomato: false,
  };

  // "potato" | "tomato"
  type VegetableAlias = keyof VegetableOption;
}

{
  interface VegetableOption {
    // ❌ 매핑된 타입은 프로퍼티나 메서드로 선언할 수 없다.
    [Property in Vegetable]: boolean;
  }
}

export {};
```

- 매핑된 객체 타입은 타입으로만 정의될 수 있고, `in` 키워드와 `keyof` 키워드를 사용할 수 있다.

### 알려지지 않은 타입(Unknown Types)

```tsx
{
  const potato = { name: 'potato', weight: 1 };

  // type Vegetable = {
  // name: string;
  // weight: number;
  // }
  type Vegetable = typeof potato;

  const tomato: Vegetable = {
    name: 'tomato',
    weight: 2,
  };
}
```

- `unknown` 타입을 다룰 때, `typeof`를 사용해 타입을 확인할 수 있지만, 타입으로만 가능하고 인터페이스는 불가능하다.

## interface와 성능

```tsx
type type2 = { a: 1 } & { b: 2 } // 잘 머지됨
type type3 = { a: 1; b: 2 } & { b: 3 } // resolved to `never`

const t2: type2 = { a: 1, b: 2 } // good
const t3: type3 = { a: 1, b: 3 } // Type 'number' is not assignable to type 'never'.(2322)
const t3: type3 = { a: 1, b: 2 } // Type 'number' is not assignable to type 'never'.(2322)
```

- 여러 `type`, `interface`를 `&`하거나 `extends` 할 때를 생각하면, `**interface`는 속성 간 충돌을 해결하기 위해 단순한 객체 타입을 만든다.**
    - `interface`는 객체의 타입을 만들기 위한 것이고, 객체만 오기 때문에 단순히 합치면 되기 때문이다.
- 반면 `type`의 경우 재귀적으로 순회하면서 속성을 `merge`하는데, 이 경우 일부 `never`가 나오면서 제대로 머지가 안 될 수도 있다.
    - `type`은 `interface`와 달리 원시 타입이 올 수도 있기 때문에 충돌이 나서 제대로 `merge`가 안되는 경우에는 `never`가 떨어진다.

```tsx
type t1 = {
  a: number
}

type t2 = t1 & {
  b: string
}

const typeSample: t2 = { a: 1, b: 2 } // error
// before(3.x): Type 'number' is not assignable to type 'string'.
// after(4.x): Type 'number' is not assignable to type 'string'.(2322) input.tsx(14, 5): The expected type comes from property 'b' which is declared here on type 't2'
```

- interface들을 합성할 경우, 이는 캐시가 되지만 type의 경우 그렇지 않다.
- type 합성의 경우, 합성 자체에 대한 유효성을 판단하기 전에 모든 구성요소에 대한 type을 체크하므로 컴파일 시 상대적으로 성능이 좋지 않다.

## 참고 자료

- [https://bny9164.tistory.com/48](https://bny9164.tistory.com/48)
- [https://ykss.netlify.app/typescript/type_vs_interface/](https://ykss.netlify.app/typescript/type_vs_interface/)