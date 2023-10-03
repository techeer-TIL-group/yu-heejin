## NestJS란?

- 자바스크립트나 타입스크립트로 서버 애플리케이션을 개발할 수 있는 백엔드 웹 프레임워크(web framework)
- Spring이나 Django와 유사하다.
- 자바스크립트에서는 Express라는 웹 프레임워크가 서버 애플리케이션 개발에 있어서 압도적인 점유율을 차지했다.
    - Express가 경량화된 프레임워크이기 때문에 핵심적인 기능만 제공하다보니 간단한 서버 애플리케이션을 개발하는데는 큰 문제가 없었다.
    - 하지만 어느정도 규모가 있는 프로젝트에서는 직접 구현해야하는 기능이 너무 많고 다른 라이브러리를 추가로 필요로 하는 경우도 많아서 불편하다.
    - 위와 같은 문제를 해결하기 위해 등장한 것이 NestJS 프레임워크
- NestJS는 기업용 애플리케이션을 개발하기에 무리가 없을 정도로 많은 기능을 내장하고 있으며 플러그인을 통해 쉽게 확장도 가능하다.
- **OOP(객체지향 프로그래밍), DI(의존성 주입), AOP(과점 지향 프로그래밍)**과 같은 백엔드 개발 트렌드를 반영한다.

## NestJS CLI 설치

```jsx
npm i -g @nestjs/cli
```

## NestJS 프로젝트 생성

```jsx
nest new ${project_name}
```

- Typescript 엄격 모드를 활성화하여 프로젝트를 생성하려면 —strict 플래그를 함께 쓴다.

## 애플리케이션 구동하기

```jsx
npm run start
```

- 운영 단계가 아닌 개발 단계에서는 `npm run start:dev`

## localhost:3000 접속

<img width="382" alt="스크린샷 2023-04-11 오후 4 08 09" src="https://user-images.githubusercontent.com/96467030/231087671-6594ab96-db10-463d-89f4-bc8e90264608.png">

## 핵심 파일

| app.controller.ts | 단일 경로가 있는 기본 컨트롤러 |
| --- | --- |
| app.controller.spec.ts | 컨트롤러에 대한 단위 테스트 |
| app.module.ts | 응용 프로그램의 루트 모듈 |
| app.service.ts | 기본 서비스 코드 |
| main.ts | NestFactory 핵심 기능을 사용하여 Nest 애플리케이션 인스턴스를 생성하는 애플리케이션의 엔트리 파일 |

## main.ts

```tsx
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}

bootstrap();
```

- `src/main.ts` 파일은 NestJS 애플리케이션이 시작되는 진입 지점(entry point)이 된다.
- `create()` 메서드는 `INestApplication` 인터페이스를 충족하는 응용 프로그램 개체를 반환한다.
- 코드의 마지막 줄에는 `bootstrap()`을 호출하는데, `bootstrap()` 함수 안에서는 `app.module` 파일로부터 `AppModule`을 불러와 `NestFactory`가 애플리케이션 객체를 생성하고 3000번 포트로 HTTP 요청을 받는다.

## Module

```tsx
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

- `@Module()` 데코레이터(decorator)가 호출되고 있다.
    - 데코레이터는 일반적으로 클래스나 메서드에 어떤 정보를 추가해줄 때 많이 활용된다.
    - 자바의 어노테이션이라고 부르는 문법 요소랑 비슷하다.
- `@Module()` 데코레이터는 `imports`, `controllers`, `providers` 속성으로 이루어지느 객체를 인자로 받는다.
    - `controllers` 속성에는 **HTTP 요청을 받아서 응답을 보내는 컨트롤러 클래스**를 나열
    - `providers` 속성에는 **컨트롤러가 사용하는 다양한 일반 클래스(주로 서비스 클래스)**를 나열
    - `imports` 속성에는 **해당 모듈이 의존하고 있는 다른 모듈**을 나열할 수 있다.
- 하나의 NestJS 애플리케이션은 보통 여러 개의 모듈로 이루어지는데, **기능 단위로 애플리케이션을 쪼개 놓은 단위를 모듈**이라고 한다.
- 중요한 것은 **모듈은 서로 의존**할 수 있다는 것이다!
    - `@Module()` 데코레이터에 인자로 넘기는 **객체의 `imports` 속성을 통해 이 의존 관계를 명시**하도록 되어있다.
- 정리하자면 **NestJS는 일종의 `IoC(Inversion of Control)` 컨테이너의 역할을 하면서 여러 모듈을 `DI(의존성 주입)`를 통해 엮어준다고 보면 된다.**
    - 어떻게 엮어야 하는지는 개발자가 각 모듈에 @Module() 데코레이터의 imports 속성으로 NestJS에게 알려줘야 한다.
    

## Controller

```tsx
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```

- `@Controller()` 데코레이터를 호출해주면 NestJS가 해당 클래스를 컨트롤러로 인식하게 된다.
- 클래스 내의 각 메서드에는 `@Get()`, `@Post()`, `@Delete()`와 같은 HTTP Method에 해당하는 데코레이터를 붙여준다.
    - 이러한 데코레이터들은 **URL 경로를 나타내는 문자열을 인자로 받는다.**
- 예를 들어, `@Controller(”aaa”)`가 붙어있는 클래스의 `@Post(”bbb”)`가 붙어있는 메서드가 있었다면, Post 방식으로 `http://localhost:3000/aaa/bbb`를 요청했을 때 해당 메서드가 호출된다.

### response

| 표준(권장) | 기본 제공 메서드를 사용하면 request handler가 Javascript 개체 또는 배열을 반환할 때 자동으로 JSON으로 직렬화한다.
그러나, 이것이 자바스크립트 Primitive 타입(string, number, boolean 등)을 반환하면, Nest는 직렬화 없이 값만 반환한다.
또한, 응답 코드는 201을 사용하는 POST를 제외하고는 기본적으로 항상 200이다.@HttpCode(…) 데코레이터를 추가하여 해당 동작을 쉽게 변경할 수 있다. |
| --- | --- |
| 라이브러리별(Library-specific) | @Res() 데코레이터를 사용하여 주입할 수 있는 라이브러리별(ex. Express) 응답 객체를 사용할 수 있다. 해당 방법을 사용하면 해당 객체에 의해 노출된 기본 응답 처리 방법을 사용할 수 있다.
예를 들어, Express에서는 response.status(200).send()와 같은 코드를 사용하여 응답을 구성할 수 있다. |

## Service

```tsx
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}
```

- 비즈니스 로직을 수행하는 역할을 담당한다.
- 해당 클래스 위에는 `@Injectable()` 데코레이터가 사용된다.
    - 해당 데코레이터가 붙어있는 클래스는 **NestJS가 인스턴스를 생성하여 다른 클래스에 생성자를 통해 주입할 수 있다.**
- 위 `AppModule` 코드에서 `@Module()` 데코레이터를 호출할 때 `providers` 속성에 `AppService` 클래스를 명시해주었다.
    - 이 덕분에 `AppController` 클래스의 생성자의 인자로 `AppService` 클래스의 인스턴스가 주입되었고, `AppController` 클래스의 `getHello` 메서드 내에서 `AppService` 클래스의 `getHello` 메서드를 호출할 수 있다.

## Platform

- NestJS는 플랫폼에 구애받지 않는 프레임워크를 목표로 한다.
- 플랫폼 독립성을 통해 개발자는 여러 유형의 응용 프로그램에서 활용 가능하며 재사용이 가능한 논리적 파트를 만들 수 있다.

| platform-express | Express는 노드용으로 잘 알려진 미니멀리스트 웹 프레임워크이다. 커뮤니티에서 구현한 많은 리소스를 갖추고 있으며, 실무에서 테스트된 프로덕션용 라이브러리이다.
@nestjs/platform-express 패키지가 기본으로 사용된다. |
| --- | --- |
| platform-fastify | Fastify는 최대의 효율성과 속도를 제공하는데 중점을 둔 고성능의 오버헤드가 낮은 프레임워크이다. |
- 어떤 플랫폼을 사용하든 자체 애플리케이션 인터페이스를 노출한다.
- 이들은 각각 `NestExpressApplication`과 `NestFastifyApplication`으로 표시된다.
- 다음과 같이 플랫폼 유형을 `NestFactory.create()` 메소드에 전달하면 app 객체는 해당하는 특정 플랫폼에만 사용할 수 있는 메서드를 갖게 된다.
    
    ```tsx
    const app = await NestFactory.create<NestExpressApplication>(AppModule);
    ```
    
    - 그러나 기본 플랫폼 API에 실제로 액세스 하려는 경우가 아니라면 유형을 지정할 필요가 없다.

## 참고 자료

- [https://www.daleseo.com/nestjs/](https://www.daleseo.com/nestjs/)
- NestJS 공식 문서