- 서버는 종종 클라이언트의 잘못된 요청에 대해서 응답해야하는 경우가 있다.
- 이러한 예외처리를 정확하게 해야한다.
- NestJS는 `ValidationPipe`로 **잘못된 요청들을 검증할 수 있는 툴**을 제공하곤 한다.

## Validation Pipe

- `class-validator` 패키지와 해당 패키지에서 제공하는 데코레이터를 활용하여 요청 데이터의 유효성을 검증하는 기능

## class-validator

- 데코레이터와 데코레이터를 사용하지 않는 방식 모두 validation을 할 수 있도록 지원하는 라이브러리
- 다음과 같은 validation 데코레이터들이 있다.
    
    `@IsEmpty()`, `@IsIn(values: any[])`, `@IsNumber(options: IsNumberOptions)`, `@IsEnum(entity: object)`, `@IsDateString()`, `@IsNumberString(options?: IsNumericOptions)`, `@IsCurrency(options?: IsCurrencyOptions)`, `@IsHexColor()`, `@IsISIN()`…
    

## NestJS에 class-validator 적용하기

### DTO 클래스에 데코레이터 추가

```tsx
import { IsIn, IsNumberString } from "class-validator";

export class CandleDto {
  @IsIn(MARKET_KEYS)
  market: MARKETS;

  @IsNumberString()
  minutes?: number;

  @IsNumberString()
  count?: number;
}
```

- @IsIn(): request로 온 market의 값이 MARKET_KEYS 배열에 없는 경우 에러 발생 (정해진 배열 값만 사용)
- @IsNumberString(): number로 변환할 수 있는 문자열인지 여부 확인
    - 123은 가능하고 abc나 123a는 오류가 발생한다.

### 유효성 제약 조건

```tsx
import {
  ArrayNotEmpty,
  Contains,
  IsArray,
  IsString,
  Matches,
  MaxLength
} from 'class-validator'

export class StringArray {
  @IsArray()
  @ArrayNotEmpty()
  // each: 배열의 모든 값이 유효한지 체크
  @IsString({ each: true }) 
  @MaxLength(6, { each: true })
  
  // 알파벳으로 이루어져있는지 체크
  @Matches('^[a-zA-Z\\s]+$', undefined, { each: true })
  
  // 'hello' 문자를 포함하고 있는 지 체크
  @Contains('hello', { each: true })
  stringArray: string[]
}
```

- each: 배열의 모든 값이 유효한지 체크
- @Matchs: 정규식 조건 사용
- @Contains: 문자 또는 숫자를 포함하는지 체크

```tsx
export class Test {
  @ValidateNested({ each: true })
  @Type(() => Foo)
  input: Foo[];
}
```

- dto 내 연관 객체에 `ValidateNested` 데코레이터와 `Type` 데코레이터를 사용
- 만약 연관 객체가 배열 객체인 경우 ValidateNested 내에 `{ each: true }` 옵션을 부여하면 된다.
    - `{ each: true }`는 요소별로 검사하는 옵션으로, 배열에서 사용한다.
- 중첩된 객체를 검증할 땐 `@Type`을 통해 리터럴 객체를 해당 클래스의 인스턴스로 변환을 해주어야 한다.

## 참고 자료

- [https://chaewonkong.github.io/posts/nestjs-validation-pipe-with-class-validator.html](https://chaewonkong.github.io/posts/nestjs-validation-pipe-with-class-validator.html)
- [https://velog.io/@wngud4950/NestJS-Validation-Pipe-적용기](https://velog.io/@wngud4950/NestJS-Validation-Pipe-%EC%A0%81%EC%9A%A9%EA%B8%B0)
- [https://www.zodaland.com/tip/8](https://www.zodaland.com/tip/8)
- [https://sub.isthislee.com/validate-nested-dto/](https://sub.isthislee.com/validate-nested-dto/)
- [https://dev.to/avantar/validating-nested-objects-with-class-validator-in-nestjs-1gn8](https://dev.to/avantar/validating-nested-objects-with-class-validator-in-nestjs-1gn8)