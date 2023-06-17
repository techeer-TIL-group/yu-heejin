## ts-jenum

- java의 `java.lang.Enum`과 같은 사용성을 얻기 위해 제공하는 라이브러리
- java의 `Enum`처럼 `static` 객체로 Enum을 다룰 수 있다.

## Java의 Enum

```tsx
public interface Calculation {
    int doCalc(int x, int y);
}

@Getter
@AllArgsConstructor
public enum Calculator {
    PLUS(Integer::sum),
    MINUS((x, y) -> x - y),
    MULTIPLY((x, y) -> x * y),
    DIVIDE((x, y) -> x / y);

    private Calculation calculation;
}
```

- 자바에서는 Enum을 마치 객체처럼 사용할 수 있다.

## Typescript Enum

```tsx
export class Calculator {

  static readonly operators: Calculation[] = [
    {operator: 'plus', do: (x, y) => x + y},
    {operator: 'minus', do: (x, y) => x - y},
    {operator: 'multiply', do: (x, y) => x * y},
    {operator: 'divide', do: (x, y) => x / y},
  ]

  static find(operator: string) {
    return this.operators.filter(o => o.operator === operator).pop();
  }
}

export interface Calculation {
  operator: string;
  do: (x: number, y: number) => number;
}
```

- 타입스크립트는 Enum타입을 열거형으로만 사용할 수 있다. (상수용)
- 자바의 Enum처럼 특정 연산을 수행할 수 없다.

## 사용법

### 클래스 생성하기

```tsx
import { Enum, EnumType } from 'ts-jenum';

@Enum('code') // (1)
export class EJobLevel extends EnumType<EJobLevel>() { // (2)
    // (3)
    static readonly IRRELEVANT = new EJobLevel("IRRELEVANT", '경력무관', 0, 99,);
    static readonly BEGINNER = new EJobLevel("BEGINNER", '인턴/신입', 0, 0,);
    static readonly JUNIOR = new EJobLevel("JUNIOR", '주니어', 1, 3);
    static readonly MIDDLE = new EJobLevel("MIDDLE", '미들', 4, 7);
    static readonly SENIOR = new EJobLevel("SENIOR", '시니어', 8, 20);

    private constructor(readonly _code: string, readonly _name: string, readonly _startYear, readonly _endYear,) {
        super();
    }
}
```

- @Enum(’fieldName’)
    - enum class의 **메인 key가 될 필드를 지정**한다.
    - **괄호 안에 값이 해당 Enum값의 키가 된다.**
    - 해당 key는 절대 중복이 되어서는 안된다.
        - EnumClass의 `static class`들의 구분자 역할을 한다.
- extends EnumType<EJobLevel>()
    - ts-jenum이 제공하는 **EnumType을 꼭 상속 받아야한다.**
    - **EnumType은 제네릭 타입으로 상속받은 EnumClass를 사용해야한다.**
        - enumClass에서 제공하는 여러편의 메소드들(`find`, `values`, `valueByName`…)을 사용할 때 타입 명시가 필요하기 때문이다.
- static readonly IRRELEVANT = …
    - Enum타입을 선언하듯 **EnumClass의 타입을 선언한다.**
    - 해당 클래스 안에서 선언한 `IRRELEVANT`, `BEGINNER`, `JUNIOR`, `MIDDLE`, `SENIOR` 등이 EnumClass의 타입으로 작동한다.

## 사용 예제

```tsx
import { Enum, EnumType } from 'ts-jenum';

@Enum('code')
export class JobLevel extends EnumType<JobLevel>() {
  static readonly IRRELEVANT = new JobLevel('IRRELEVANT', '경력무관', 0, 99);
  static readonly BEGINNER = new JobLevel('BEGINNER', '인턴/신입', 0, 0);
  static readonly JUNIOR = new JobLevel('JUNIOR', '주니어', 1, 3);
  static readonly MIDDLE = new JobLevel('MIDDLE', '미들', 4, 7);
  static readonly SENIOR = new JobLevel('SENIOR', '시니어', 8, 20);

  private constructor(
    readonly _code: string,
    readonly _name: string,
    readonly _startYear,
    readonly _endYear,
  ) {
    super();
  }

  get code(): string {
    return this._code;
  }

  get name(): string {
    return this._name;
  }

  get startYear(): number {
    return this._startYear;
  }

  get endYear(): number {
    return this._endYear;
  }

  static findName(code: string): string {
    return this.values().find((e) => e.equals(code))?.name;
  }

  static findByYear(year: number): JobLevel {
    return this.values().find(
      (e) => e.betweenYear(year) && e !== this.IRRELEVANT,
    );
  }

  betweenYear(year: number): boolean {
    return this.startYear <= year && this.endYear >= year;
  }

  getPeriod(): string {
    return `${this.startYear} ~ ${this.endYear}`;
  }

  equals(code: string): boolean {
    return this.code === code;
  }

  toCodeName() {
    return {
      code: this.code,
      name: this.name,
    };
  }
}
```

### 데이터들간 연관 관계 정리

- 데이터베이스에 저장된 영문자 `BEGINNER`는 화면에서 인턴/신입으로 노출되어야한다고 가정하자.
    - `IRRELEVANT(경력무관)`, `JUNIOR(주니어)`, `MIDDLE(미들)`, `SENIOR(시니어)`도 마찬가지
- 이러한 경우 리터럴 객체도 좋지만, **타입힌트, 자동완성, 상속 or 구성 등의 확장성까지 고려한다면 클래스로 추출하기 좋다.**
- 미리 정의된 데이터 세트 안에서만 활동한다면 `ts-jenum`의 `EnumClass`가 많은 도움이 된다.
    
    ```tsx
    @Enum('code')
    export class JobLevel extends EnumType<JobLevel>() {
      static readonly IRRELEVANT = new JobLevel('IRRELEVANT', '경력무관', 0, 99);
      static readonly BEGINNER = new JobLevel('BEGINNER', '인턴/신입', 0, 0);
      static readonly JUNIOR = new JobLevel('JUNIOR', '주니어', 1, 3);
      static readonly MIDDLE = new JobLevel('MIDDLE', '미들', 4, 7);
      static readonly SENIOR = new JobLevel('SENIOR', '시니어', 8, 20);
    
      ....
    }
    ```
    
- 이러한 관련 데이터들을 묶어서 API로 모두 다 함께 내려줘야한다면 다음과 같이 클래스의 메소드로 쉽게 변환 가능하다.
    
    ```tsx
    @Enum('code')
    export class JobLevel extends EnumType<JobLevel>() {
      ....
      toCodeName() {
        return {
          code: this.code,
          name: this.name,
        };
      }
    }
    ```
    

### 상태와 행위 한 곳에서 관리하기

- 다음과 같은 도메인 로직이 있다고 가정하자.
    - 경력무관: 0 ~ 99년차
    - 신입/인턴: 0년차
    - 주니어: 1~3년차
    - 미들: 4~7년차
    - 시니어: 8~20년차
- 이때 해당 사용자의 연차를 기준으로 등급이 어떻게 되는지 확인하는 기능이 필요하다면, enumClass를 활용하면 상태와 로직을 한 곳에서 관리할 수 있다.
    
    ```tsx
    @Enum('code')
    export class JobLevel extends EnumType<JobLevel>() {
      static readonly IRRELEVANT = new JobLevel('IRRELEVANT', '경력무관', 0, 99);
      static readonly BEGINNER = new JobLevel('BEGINNER', '인턴/신입', 0, 0);
      static readonly JUNIOR = new JobLevel('JUNIOR', '주니어', 1, 3);
      static readonly MIDDLE = new JobLevel('MIDDLE', '미들', 4, 7);
      static readonly SENIOR = new JobLevel('SENIOR', '시니어', 8, 20);
      ...
    
      // 전체 JobLevel 중 해당 연차에 포함되는 JobLevel 탐색
      static findByYear(year: number): JobLevel {
        return this.values().find(
          (e) => e.betweenYear(year) && e !== this.IRRELEVANT,
        );
      }
    
      // 해당 JobLevel의 연차 범위내에 포함되는지 확인
      betweenYear(year: number): boolean {
        return this.startYear <= year && this.endYear >= year;
      }
      ...
    }
    
    it.each([
      [0, '인턴/신입'],
      [1, '주니어'],
      [5, '미들'],
      [11, '시니어'],
    ])('연차 %s인 경우 역량 등급은 %s이다', (year, grade) => {
      const result = JobLevel.findByYear(Number(year));
    
      expect(result.name).toBe(grade);
    });
    ```
    

## 메소드

```tsx
import {Enum, EnumType} from "ts-jenum";

@Enum("text")
export class State extends EnumType<State>() {

    static readonly NEW = new State(1, "New");
    static readonly ACTIVE = new State(2, "Active");
    static readonly BLOCKED = new State(3, "Blocked");

    private constructor(readonly code: number, readonly text: string) {
        super();
    }
}

// Usage example
console.log("" + State.ACTIVE);        // "Active"
console.log("" + State.BLOCKED);       // "Blocked"
console.log(State.values());           // [State.NEW, State.ACTIVE, State.BLOCKED]
console.log(State.valueOf("New"));     // State.NEW
State.valueOf("Unknown")               // throw Error(...)
console.log(State.valueByName("NEW")); // State.NEW
console.log(State.ACTIVE.enumName);    // ACTIVE

const first = state => state.code === 1;
console.log(State.find("New"));        // State.NEW
console.log(State.find(first));        // State.NEW
console.log(State.find("Unknown"));    // null
const last = state => state.code === 3;
console.log(State.filter(last))        // [State.BLOCKED]
console.log(State.keys())              // ["NEW", "ACTIVE", "BLOCKED"]

// be "NEW" | "ACTIVE" | "BLOCKED"
type StateNameUnion = EnumConstNames<typeof State>;
```

## 참고 자료

- https://jojoldu.tistory.com/621
- https://overcome-the-limits.tistory.com/706?category=1006727
- [https://velog.io/@leeseonseonje/타입스크립트-Enum](https://velog.io/@leeseonseonje/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Enum)
- https://github.com/reforms/ts-jenum