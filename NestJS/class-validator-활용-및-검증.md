## class-validator

- joi의 타입스크립트 버전
- 데코레이터를 이용해서 편리하게 **오브젝트의 프로퍼티를 검증할 수 있는 라이브러리**이다.

## 기본 사용법

```tsx
import { IsEmail, IsOptional, IsString, Min } from 'class-validator';

class CreateUserDto {
  @IsString()
  name: string;

  @Min(1)
  age: number;

  @IsEmail()
  @IsOptional()
  email?: string;
}
```

```tsx
import { validateOrReject } from 'class-validator';

const createUserDto = new CreateUserDto();
createUserDto.name = 'Kim';
createUserDto.age = 28;

validateOrReject(createUserDto); // 통과
```

## 웹 프레임워크에서의 응용

- express나 Nest.js같은 웹 프레임워크에서는 request JSON body를 검증하는데 사용한다.
- `class-transformer`와 `class-validator`를 이용해 라우터나 컨트롤러에 도달하기 전에 **body를 클래스의 인스턴스로 변환한 뒤 검증할 수 있다.**

### Nest.js

- 기본 ValidationPipe를 사용하면 된다.
    - Global pipe로 등록하거나 App 모듈의 APP_PIPE라는 Provider로 등록할 수 있다.
    - App 모듈의 provider로 등록한다면 의존성 주입을 활용할 수 있다는 장점이 있다.

```tsx
import { ..., ValidationPipe } from '@nestjs/common';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  
  // app 인스턴스 생성 시 Global pipe로 등록
  app.useGlobalPipes(
    new ValidationPipe()
  );
}

bootstrap();
```

```tsx
import { ..., ValidationPipe } from '@nestjs/common';
import { APP_PIPE } from '@nestjs/core';

// App 모듈에서 provider로 등록
@Module({
  providers: [
    {
      provide: APP_PIPE,
      useClass: ValidationPipe, // 옵션설정을 위해 useValue, useFactory 등 사용 가능
    },
  ],
})
export class AppModule {}
```

### Express

- express를 사용할 땐 이 부분을 검증하는 클래스 스키마를 인자로 받는 미들웨어로 만들어서 처리할 수 있다.

```tsx
import { validateOrReject } from 'class-validator';
import { plainToClass } from 'class-transformer';

// 미들웨어 정의
export function validateBody(schema: { new (): any }) {
  return async function (req: Request, res: Response, next: NextFunction) {
    const target = plainToClass(schema, req.body);
    try {
      await validateOrReject(target);
      next();
    } catch (error) {
      next(error);
    }
  };
}
```

## 검증 규칙

### skipMissingProperties

```tsx
const createUserDto = new CreateUserDto();
createUserDto.name = 'Kim';
// createUserDto.age = 28;

validateOrReject(createUserDto, { skipMissingProperties: true }); // 통과
validateOrReject(createUserDto); // ValidationError: age must not be less than 1
```

- `class-validator`는 검증 데코레이터가 붙은 프로퍼티가 대상 오브젝트에 존재하지 않으면 오류를 발생한다.
    - `@IsOptional()` 제외
- 검증 데코레이터가 있고, IsOptional이 아닌 프로퍼티가 대상 오브젝트에 존재하지 않더라도 에러가 나지 않게 해주는 옵션이다.

### forbidUnknownValues

```tsx
class UnknownValue {
  name: string;
  age: number;
}

const unknownValue = new UnknownValue();
unknownValue.name = 'Kim';
unknownValue.age = 28;

validateOrReject(unknownValue); // 통과
validateOrReject(unknownValue, { forbidUnknownValues: true }); // ValidationError: an unknown value was passed to the validate function
```

- 프로퍼티에 검증 규칙이 정의되어 있지 않은 클래스의 인스턴스나 plain object를 검증할 때 오류가 나도록하는 옵션이다.
- 해당 옵션이 없으면 프로퍼티에 규칙이 정해지지 않은 클래스 스키마나 plain object도 검증을 통과하기 때문에 **공식 문서에서 해당 옵션을 true로 설정하는 것을 권장한다.**

### whitelist

```tsx
const createUserDto = new CreateUserDto();
createUserDto.name = 'Kim';
createUserDto.age = 28;
(createUserDto as any).blog = 'Tistory';

await validateOrReject(createUserDto);
console.log(createUserDto); // CreateUserDto { name: 'Kim', age: 28, blog: 'Tistory' }

await validateOrReject(createUserDto, { whitelist: true });
console.log(createUserDto); // CreateUserDto { name: 'Kim', age: 28 }
```

- `class-validator`는 검증을 수행할 때 대상 객체에 검증 규칙이 정의되어 있지 않은 프로퍼티가 있더라도 오류를 내지 않고 그대로 통과시켜준다.
- `whitelist` 옵션은 **검증을 통과한 뒤 대상 객체에서 검증 규칙이 정의되어 있지 않은 프로퍼티를 모두 제거해주는 옵션**이다.
    - 어떤 형태의 JSON body가 들어올지 모르는 요청을 검증 이후에 예측 가능한 객체 형태로 만들어줄 수 있기 때문에 유용하다.

### forbidNonWhitelisted

```tsx
const createUserDto = new CreateUserDto();
createUserDto.name = 'Kim';
createUserDto.age = 28;
(createUserDto as any).blog = 'Tistory';

validateOrReject(createUserDto, { whitelist: true, forbidNonWhitelisted: true }); // ValidationError: 'property blog should not exist'
```

- `whitelist` 옵션과 같이 사용하는 옵션으로, **대상 객체에 검증 규칙이 정의되어 있지 않은 프로퍼티가 있으면 오류를 발생시키는 옵션이다.**

## 참고 자료

- https://seungtaek-overflow.tistory.com/13