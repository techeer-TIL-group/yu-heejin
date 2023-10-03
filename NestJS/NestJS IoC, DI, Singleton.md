## Provider

- provider는 Nest의 기본 개념
- 대부분의 Nest 클래스는 Service, Repository, Factory, Helper 등 provider로 취급될 수 있다.
- provider의 주요 아이디어는 dependency로 주입할 수 있다.
    - dependency로 주입할 수 있다는 의미는 object가 다른 object와 다양한 관계를 만들 수 있고, 객체의 instance를 ‘wiring up’하는 기능은 Nest runtime system에 위임될 수 있다.

## Inversion of Control(IoC)

- 제어의 역전이란 객체는 **객체가 의존하는 다른 객체를 스스로 생성하지 않고 외부로부터 얻음**을 의미한다.
- 즉, **개발자가 제어할 영역을 NestJS에게 넘겨준다.**
- 결합성(coupling)을 느슨하게 하고, 테스트와 재사용성을 용이하게 하는 패턴이다.
- Nest는 providers 사이의 관계를 해결해주는 내장된 Inversion of control(”IoC”) container를 가지고 있다.
- DI를 구현하기 위해서는 IoC 컨테이너 기술이 필요하며, IoC는 provider를 다른 컴포넌트에 주입할 때 사용하는 기술, Nest는 프레임워크에 IoC를 구현하고 있다.

### 예시

```tsx
export class UserController {
	constructor(private readonly userService: UserService) {}
}
```

- UserController는 UserService에 의존하고 있지만, UserService 객체의 라이프 사이클에는 전혀 관여하지 않는다.
    - 단지 컨트롤러의 생성자에 주어진 객체를 가져다 쓰고 있다.
    - 이러한 역할을 하는 것이 IoC
- IoC의 도움으로 객체의 라이프 사이클에 신경쓰지 않아도 된다.

### IoC 사용 예시

- A는 B에 의존하고 있으며, A 객체에서 B 객체가 필요하다고 할 때, A 클래스에는 B 클래스를 생성해서 사용 가능하다.
- 문제는 B의 구현체가 변경되었을 때 발생한다.
    - A는 B를 직접 참조하기 때문에 B가 변경될 때마다 compiler는 A를 다시 컴파일 해야하며, A와 B가 클래스가 아닌 모듈이면 변경의 크기는 더 커지고 컴파일 시간이 더 오래걸린다.
- 이를 해결하기 위해 B에 대한 interface를 정의하고, A에서는 IB 타입을 이용한다.
- 하지만 인터페이스 B의 구현체를 직접 생성해야 하는 것은 여전하기 떄문에 IoC의 강력함이 발휘된다.

### IoC Container

- IoC를 구현하는 프레임워크로, **객체를 관리하고 생성을 책임지고 의존성을 관리하는 컨테이너**
- 일반적인 라이브러리 호출과 달리 **외부 라이브러리 코드에서 프로그래머가 작성한 코드를 호출하는 형식**

## Dependency Injection(DI)

- dependencies는 클래스가 동작하기 위해 필요한 서비스나 객체를 의미한다.
- IoC 패턴의 구체적인 버전 중 하나이다.
- DI는 IoC 컨테이너가 직접 객체의 생명주기를 관리하는 방식이다.
- Dependency Injection(DI)는 IoC 기술로, **사용자 자신의 코드로 종속성을 인스턴스화 하는 대신 IoC 컨테이너(NestJS 런타임 시스템)로 위임**한다.
- class가 의존성 객체를 외부에 요청하고 외부에서 인스턴스를 생성해서 주입하는 디자인 패턴
- 객체가 작동하는데 필요한 구현체가 contructors, setters, service lookups를 통해 전달된다.

### @Injectable()

- `@Injectable()` 데코레이터는 이 클래스는 DI system에 활용하겠다는 것을 의미한다.
- `UserService`가 `NestIoC` 컨테이너에서 관리할 수 있는 클래스임을 선언하는 메티데이터를 첨부한다.
- 만약 `userService`에 해당 데코레이터를 사용하면 다른 어떤 Nest component에서도 주입할 수 있는 provider가 된다.
    - 별도의 scope를 지정하지 않으면 singletone 인스턴스가 된다.

### 사용 예시

```tsx
constructor(private userRepository: UserRepository)
```

- Nest에서는 일반적으로 DI로 알려진 디자인 패턴을 기반으로 구축되었다.
- Nest에서는 타입스크립트 기능 덕분에 dependency가 유형별로 해결되어 매우 쉽게 관리할 수 있다.
- UserRepository를 UserService에 Dependency Injection을 하려면 constructor에 접근 제어 지시자를 활용해 초기화를 해주면 된다.
    - `private`를 사용하면 동일한 위치에서 즉시 `userRepository` 멤버를 선언하고 초기화할 수 있다.
    - 위와 같은 코드를 생성자 기반 주입 → provider라는 생성자 메서드를 통해 주입되기 때문이다.
- 특정한 경우 속성 기반 주입(Property-based-injection)이 유용하다.
    - **최상위 클래스가 하나 또는 여러 프로파이더에 의존**하는 경우 생성자의 하위 클래스에서 `super()`를 호출하여 모든 프로바이더를 전달하는 것은 쉽지 않다.
    - 이를 방지하기 위해 property 수준에서 `@Inject()` decorator를 사용한다.

```tsx
import { UserRepository } from './../repository/user.repository';
import { PrismaService } from './../prisma/prisma.service';
import { Module } from '@nestjs/common';
import { UserService } from './user.service';
import { UserController } from './user.controller';

@Module({
	controllers: [UserController],
	providers: [UserService, PrismaService, UserRepository],
})
export class UserModule {}
```

- injection을 수행할 수 있게 Service를 Nest에 등록한다.
- `@Module()` 데코레이터의 providers 배열에 service들을 추가해서 이를 수행한다.
- 이렇게 해서 Nest는 userController class의 dependency를 해결할 수 있다.

## IoC in Nest

- nestJS에서 의존성의 인스턴스 생성은 IoC 컨테이너(NestJS 런타임)에 위임한다.

### 과정

1. 앱 시작 시 모든 클래스를 DI 컨테이너에 등록
    - class에 Injectable 데코레이터 붙이고 module의 providers에 등록, controller는 controller 데코레이터로 DI 컨테이너에 등록 및 인스턴스 생성
2. 컨테이너가 각 클래스의 의존성 파악
3. 컨테이너는 각 클래스의 인스턴스를 생성하고 제공(Nest가 자동으로 수행)
4. 생성된 인스턴스는 삭제되거나 재생성되지 않고 필요시 재사용된다.

## Singleton

- 싱글톤 패턴을 따르는 클래스는 **생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나**이며, 최초 생성 이후에 호출된 생성자는 **최초의 생성자가 생성한 객체를 리턴**한다.
- 주로 공통된 객체를 여러 개 생성해서 사용하는 DBCP(Database Connection Pool)와 같은 상황에서 많이 사용한다.
    - DBCP는 데이터 접근 패턴 중 하나로, 데이터베이스에 접속하여 작업하는데 과부하를 줄이는 것이다.
- NestJS는 싱글톤 패턴을 지향하기 때문에 instance를 직접 생성하지 않고 모듈을 통해 Injection하는 패턴을 권장한다.

## 참고 자료

- [https://velog.io/@jeong3320/NestJsIoC-와-DI](https://velog.io/@jeong3320/NestJsIoC-%EC%99%80-DI)
- [https://m.blog.naver.com/fbfbf1/222620699725](https://m.blog.naver.com/fbfbf1/222620699725)