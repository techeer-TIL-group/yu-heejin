## @NoArgsConstructor(access = AccessLevel.PROTECTED)

```java
protected Class() {}
```

- 일반적으로 파라미터가 없는 기본 생성자에 접근하는 것을 막기 위함이다.
- 해당 제한을 걸지 않으면, 클래스 안에 `@Setter`를 사용하여 값을 변경할 수 있기 때문이라고 한다.
- **아무런 매개변수가 없는 생성자를 생성하되, 다른 패키지에 소속된 클래스는 접근을 불허한다.**

## Entity 클래스에 기본 인스턴스를 생성하는 이유?

- JPA는 DB 값을 객체 필드에 주입할 때 기본 생성자로 객체를 생성한 후 이러한 Reflection API를 사용하여 값을 매핑하기 때문에 기본 생성자를 꼭 가져야한다.
- Reflection은 클래스 이름만 알면 생성자, 필드, 메서드 등 클래스의 모든 정보에 접근이 가능하다.
- 하지만 Reflection이 가져올 수 없는 정보가 있는데, 바로 **생성자의 매개변수 정보** 이다.
    - 때문에 Reflection으로 생성할 객체에 모든 필드를 받는 생성자가 있더라도 Reflection은 해당 생성자를 호출할 수가 없다.
    - 따라서 Reflection은 기본 생성자로 객체를 생성하고 필드 값을 강제로 매핑해주는 방식을 사용한다.

[Reflection API 간단히 알아보자.](https://tecoble.techcourse.co.kr/post/2020-07-16-reflection-api/)

[JPA의 엔티티에 protected, public 기본 생성자가 필요한 이유](https://velog.io/@ohzzi/JPA의-엔티티에-protected-public-기본-생성자가-필요한-이유)

## Protected로 접근 권한을 설정하는 이유?

- 일반적으로 파라미터가 없는 기본 생성자에 접근하는 것을 막기 위함과 동시에 Entity의 Proxy 조회 때문이라고 한다.
- 정확히는 Entity의 연관 관계에서 **지연 로딩의 경우에는 실제 엔티티가 아닌 프록시 객체를 통해 조회한다.**
- 프록시 객체를 사용하기 위해 **JPA 구현체는 실제 엔티티의 기본 생성자를 통해 프록시 객체를 생성하는데, 이 때 접근 권한이 private이면 프록시 객체를 생성할 수 없다.**
- 공식 문서에서도 다음과 같이 설명이 나와있다.
    
    ```java
    The entity class must have a no-arg constructor. The entity class may have other constructors as well.
    The no-arg constructor must be public or protected.
    // Entity 클래스는 매개변수가 없는 생성자의 접근 레벨이 public 또는 protected로 해야 한다.
    
    ... 
    
    An instance variable must be directly accessed only from within the methods of the entity by the entity instance itself.
    // 인스턴스 변수는 직접 접근이 아닌 내부 메소드로 접근해야 한다.
    ```
    
- 결론적으로 public으로 설정하면 무분별한 객체 생성 및 setter를 통한 값 주입이 가능하고, private로 하면 프록시 객체 생성에 문제가 생길 수 있기 때문에 접근 권한을 protected로 설정하는 것이다.
- 그래서 Entity단에는 Protected 접근 제한자를 설정해주는 것이 맞고, DTO와 같은 레벨에서는 설정하지 않아도 된다.

## 참고 자료

- [https://velog.io/@kevin_/내가-NoargsConstructor-access-AccessLevel.PROTECTED를-왜-작성했을까](https://velog.io/@kevin_/%EB%82%B4%EA%B0%80-NoargsConstructor-access-AccessLevel.PROTECTED%EB%A5%BC-%EC%99%9C-%EC%9E%91%EC%84%B1%ED%96%88%EC%9D%84%EA%B9%8C)
- https://oh-sh-2134.tistory.com/107
- https://erjuer.tistory.com/106
- https://hungseong.tistory.com/70
- https://hyeonic.tistory.com/191