## 파사드 패턴(facade pattern)

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99B6F54A5C68D4A91D)

> 하위 시스템을 보다 쉽게 사용할 수 있게 해주는 고급 인터페이스
> 
- Facade Pattern의 목적은 **복잡한 서브 시스템을 인터페이스로 감싸서 사용하기 쉽게 만드는 것이다.**
- 객체 지향 프로그래밍 분야에서 많이 사용되며, 제 3의 API(Third Party API)같은 외부 라이브러리를 추상화하는데도 사용된다.
- 파사트 패턴은 구조 패턴(Structural Patterns)에 속한다.
    - 구조 패턴이란 클라스나 객체를 조합하여 더 큰 구조를 만드는 패턴이다.
- 서로 다른 인터페이스를 지닌 2개의 객체를 묶어 단일 인터페이스를 제공하거나 객체들을 서로 묶어 새로운 기능을 제공할 수 있다.

## 사용 예시

- `FacadeService`는 서브 시스템들을 캡슐화하며, `Client`는 `FacadeService`의 `operate` 메서드만 이용할 수 있고 내부는 알 수 없다.
- Client는 1개라는 보장이 없으며, 100개의 클래스가 서브 시스템을 이용할 수 있다.
    - 이 때 `FacadeService`가 없으면 서브 시스템의 초기화와 100개의 클래스를 모두 구현해야 한다.
- 이와 같은 패턴을 사용하여 서브 시스템으로부터 분리되어 서브 시스템에 의존하지 않아도 된다.

### SubSystem

```java
public class SubSystem1 {
    public void doSomething(final String name) {
        System.out.println("Operate 1 " + name);
    }
}
```

```java
public class SubSystem2 {
    public void doSomething(final String name) {
        System.out.println("Operate 2 "  + name);
    }
}
```

```java
public class SubSystem3 {
    public void doSomething(final String name) {
        System.out.println("Operate 3 " + name);
    }
}
```

### FacadeService

```java
public class FacadeService {
    private final SubSystem1 subSystem1;
    private final SubSystem2 subSystem2;
    private final SubSystem3 subSystem3;

    public FacadeService() {
        subSystem1 = new SubSystem1();
        subSystem2 = new SubSystem2();
        subSystem3 = new SubSystem3();
    }

    public void operate(final String name) {
        subSystem1.doSomething(name);
        subSystem2.doSomething(name);
        subSystem3.doSomething(name);
    }
}
```

### Client

```java
public class Client {
    public static void main(String[] args) {
        FacadeService facadeService = new FacadeService();
        facadeService.operate("Client");
    }
}
```

## 참고 자료

- https://imasoftwareengineer.tistory.com/29
- https://break-over.tistory.com/47
- https://wildeveloperetrain.tistory.com/99