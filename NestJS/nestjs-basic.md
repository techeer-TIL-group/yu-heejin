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

## 애플리케이션 구동하기

```jsx
npm run start
```

- 운영 단계가 아닌 개발 단계에서는 `npm run start:dev`

## localhost:3000 접속

<img width="382" alt="스크린샷 2023-04-11 오후 4 08 09" src="https://user-images.githubusercontent.com/96467030/231087671-6594ab96-db10-463d-89f4-bc8e90264608.png">


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

## 참고 자료

- [https://www.daleseo.com/nestjs/](https://www.daleseo.com/nestjs/)