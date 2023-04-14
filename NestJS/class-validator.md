## class-validator란?

- joi의 Typescript버전
- 데코레이터를 이용해서 편리하게 오브젝트의 프로퍼티를 검증할 수 있는 라이브러리

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

- class 스키마를 정의하고, **프로퍼티의 유효성을 검증할 적절한 데코레이터를 달아주는 방식**으로 사용한다.

## 웹 프레임워크에서 사용하기

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbEpqfe%2FbtrtaiZLfKS%2F0lKeLmKjusj8G4ekF8iJs1%2Fimg.png)

- express나 Nest.js같은 웹 프레임워크에서는 요청 JSON body를 검증하는데 사용한다.
- class-transformer와 class-validator를 이용해 라우터나 컨트롤러에 도달하기 전에 요청의 JSON body를 클래스 인스턴스로 변환한 뒤 검증한다.

### Nest.js

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

```tsx
// Controller에서 사용
@Post()
create(@Body() createUserDto: CreateUserDto) {
  // 검증이 완료된 request body 이용
}
```

- Nest.js에서는 기본 ValidationPipe를 사용하면 된다.
- Global pipe로 등록하거나 App 모듈의 APP_PIPE라는 Provider로 등록할 수 있다.
    - App 모듈의 provider로 등록한다면 의존성 주입을 활용할 수 있다는 장점이 있다.

### express

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

```tsx
// Router에서 사용
router.post('/users', validateBody(CreateUserDto), (req: Request, res: Response) => {
    // 검증이 완료된 request body 이용
  }
);
```

- express에서 사용할 땐 **이 부분을 검증할 클래스 스키마를 인자로 받는 미들웨어**로 만들어서 처리할 수 있다.
- 검증에 성공하면 plain object인 요청 body를 변환된 클래스의 인스턴스로 새로 할당해서 다음 미들웨어로 넘겨줄 수 있다.

## 검증 규칙

- 검증에 사용하는 옵션 중 자주 사용하는 주요 옵션 4가지가 있다.

### skipMissingProperties

```tsx
const createUserDto = new CreateUserDto();
createUserDto.name = 'Kim';
// createUserDto.age = 28;

validateOrReject(createUserDto, { skipMissingProperties: true }); // 통과
validateOrReject(createUserDto); // ValidationError: age must not be less than 1
```

- class-validator는 검증 데코레이터가 붙은 프로퍼티가 대상 오브젝트에 존재하지 않으면 오류를 낸다. (`@IsOptional()` 인 프로퍼티 제외)
- `skipMissingProperties` 옵션은 검증 데코레이터가 있고, **IsOptional이 아닌 프로퍼티가 대상 오브젝트에 존재하지 않더라도 에러가 나지 않게 해주는 옵션이다.**

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

- 프로퍼티에 검증 규칙이 정의되어 있지 않은 클래스의 인스턴스나 plain object를 검증할 때 오류가 나게 만드는 옵션
- 해당 옵션이 없으면 프로퍼티에 규칙이 정해지지 않은 class 스키마나 plain object도 검증을 통과하기 때문에 **공식 문서에서 해당 옵션을 true로 설정하는 것을 강력히 권장하고 있다.**

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

- class-validator는 검증을 수행할 때 대상 객체에 검증 규칙이 정의되어 있지 않은 프로퍼티가 있더라도 오류를 내지 않고 그대로 통과시켜준다.
- whitelist 옵션은 검증을 통과한 뒤 **대상 객체에서 검증 규칙이 정의되어있지 않은 프로퍼티를 모두 제거해주는 옵션이다.**
- 어떤 형태의 JSON body가 들어올지 모르는 request body를 검증 이후 예측 가능한 객체 형태로 만들어주기 때문에 유용하다.

### forbidNonWhitelisted

```tsx
const createUserDto = new CreateUserDto();
createUserDto.name = 'Kim';
createUserDto.age = 28;
(createUserDto as any).blog = 'Tistory';

validateOrReject(createUserDto, { whitelist: true, forbidNonWhitelisted: true }); // ValidationError: 'property blog should not exist'
```

- whitelist 옵션과 함께 사용한다.
- **대상 객체에 검증 규칙이 정의되어 있지 않은 프로퍼티가 있으면 오류를 내게 하는 옵션**

## Nest.js에서 검증 규칙 사용하기

```tsx
// app 인스턴스 생성 시 Global pipe로 등록할 때
app.useGlobalPipes(
  new ValidationPipe({
    whitelist: true,
  }),
);
```

```tsx
// App 모듈에서 provider로 등록할 때
@Module({
  providers: [
    {
      provide: APP_PIPE,
      useValue: new ValidationPipe({
        whitelist: true,
      }),
    },
  ],
})
export class AppModule {}
```

- Nest.js에서는 ValidationPipe의 인스턴스를 생성할 때 **class-validator의 옵션들을 생성자의 매개변수로 넘겨줄 수 있다.**

## 참고 자료

- [https://seungtaek-overflow.tistory.com/13](https://seungtaek-overflow.tistory.com/13)