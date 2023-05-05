```tsx
import { ApiProperty } from '@nestjs/swagger';
import { IsNotEmpty, IsNumber, IsOptional, IsString } from 'class-validator';

export class RequestDto {
  @ApiProperty({
    description: 'name1 field'
  })
  @IsNotEmpty()
  @IsString()
  public name1: string;

  @ApiProperty({
    description: 'name2 field'
  })
  @IsNotEmpty()
  @IsString()
  public name2: string;

  @ApiProperty({
    description: 'name3 field'
  })
  @IsOptional()
  @IsString()
  public name3: string;
}
```

- `@IsOptional()`을 이용하면 undefined 값을 받을 수 있다.
- 값이 존재하는 경우에는 `@IsString()`, `@IsNumber()` 등의 타입 체크도 가능하다.
- 프로젝트에서 쿼리를 받을 때 필수적이지 않은 쿼리인 경우에는 해당 데코레이터를 사용하자!

## @IsOptional()

- 해당 필드의 값을 체크하여 null이나 undefined인 경우, 해당 필드의 다른 데코레이터들을 무시한다.
- 값이 존재할 경우 유효성 검사를 하는 데코레이터가 실행되며, 아니라면 유효성 검사를 하지 않는다.

## 참고 자료

- [https://velog.io/@jay2u8809/NestJs-Class-Validator-로-DTO-타입-체크하면서-undefined를-받는-방법](https://velog.io/@jay2u8809/NestJs-Class-Validator-%EB%A1%9C-DTO-%ED%83%80%EC%9E%85-%EC%B2%B4%ED%81%AC%ED%95%98%EB%A9%B4%EC%84%9C-undefined%EB%A5%BC-%EB%B0%9B%EB%8A%94-%EB%B0%A9%EB%B2%95)