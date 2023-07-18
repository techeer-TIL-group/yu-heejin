## 프록시 패턴(Proxy Pattern)

- Proxy란 사전적으로 ‘대리인’이라는 뜻을 가지고 있다.
- 자바에서 프록시는 RealSubject는 자신의 기능에만 집중하고, 그 이외의 **부가 기능을 제공하거나 접근을 제어하는 역할을 Proxy 객체에게 위임**한다.
- 프록시는 **실제 객체를 호출하면 행위를 중간에 가로채서 다른 동작을 수행하는 객체로 변경**한다.
    - 어떤 객체를 사용할 때 객체를 직접 참조하는 것이 아닌 해당 객체를 대항하는 객체를 통해 대상 객체에 접근하는 방식을 사용하면 **해당 객체가 메모리에 존재하지 않아도 기본적인 정보를 참조하거나 설정**할 수 있고, **실제 객체의 기능이 필요한 시점까지 객체의 생성을 미룰 수 있다.**
- 객체를 정교하게 제어해야 하거나 객체 참조가 필요한 경우 프록시 패턴을 사용한다.
- 프록시 패턴의 특징은 **투명성을 이용해 객체를 분리하여 재위임**한다는 것이다.
    - 분리된 객체를 위임함으로써 대리 작업을 중간 단계에 삽입할 수도 있고, 분리된 객체를 동적으로 연결함으로써 객체의 실행 시점을 관리할 수 있다.
- 인터페이스를 사용하고 실행시킬 클래스에 대해 객체가 들어갈 자리에 대리자 객체를 대신 투입하고, 클라이언트는 실제 실행시킬 클래스에 대한 메소드를 호출하는지 대리 객체의 메소드를 호출하는지 모르게 하는 것
- 다만 흐름을 제어하는 것만 담당하고 **결과를 조작하거나 변경할 순 없다.**

## 프록시 패턴의 구조

![Untitled](https://user-images.githubusercontent.com/74294325/158002498-69ce449e-cb04-4b36-8d98-4f490b257b14.png)

### subject

- Proxy와 RealSubject를 하나로 묶는 인터페이스 (다형성)
- 대상 객체와 프록시 역할을 동일하게 하는 추상 메소드 `DoAction()`을 정의한다.
- 인터페이스가 존재하기 때문에 클라이언트는 프록시 역할과 RealSubject 역할의 차이를 의식할 필요가 없다.

### RealSubject

- 원본 대상 객체

### Proxy

- 대상 객체를 중계할 대리자 역할
- 프록시는 대상 객체를 합성한다.
- 대상 객체와 같은 이름의 메서드를 호출하며, 별도의 로직을 수행할 수 있다.
- 흐름 제어만 할 뿐 결과값을 조작하거나 변경시키면 안된다.

### Client

- Subject 인터페이스를 이용하여 프록시 객체를 생성해 이용한다.
- 클라이언트는 프록시를 중간에 두고 프록시를 통해 실제 객체와 데이터를 주고 받는다.

## 프록시 패턴 종류

### 원격 프록시

- 프록시 패턴을 가장 많이 응용하는 사례로 주로 데이터의 전달을 목적으로 사용한다.
- 프록시 클래스는 로컬에 있고, 대상 객체는 원격 서버에 존재하는 경우
- 프록시 객체는 네트워크를 통해 클라이언트의 요청을 전달하여 네트워크와 관련된 불필요한 작업들을 처리하고 결과값만 반환

### 가상 프록시

```java
class Proxy implements ISubject {
    private RealSubject subject; // 대상 객체를 composition

    Proxy() {
    }

    public void action() {
    	// 프록시 객체는 실제 요청(action(메소드 호출)이 들어 왔을 때 실제 객체를 생성한다.
        if(subject == null){
            subject = new RealSubject();
        }
        subject.action(); // 위임
        /* do something */
        System.out.println("프록시 객체 액션 !!");
    }
}

class Client {
    public static void main(String[] args) {
        ISubject sub = new Proxy();
        sub.action();
    }
}
```

- 프로그램의 실행 속도를 개선하기 위한 패턴
- 프록시 패턴을 이용해 **무거운 객체 생성을 유보**한다.
    - **꼭 필요로하는 시점까지 객체의 생성을 연기**하고, 해당 객체가 생성된 것처럼 동작하도록 만들고 싶을 때 사용하는 패턴
    - 실제 객체 생성 시 많은 자원이 소모되지만 사용 빈도가 낮을 때 쓰는 방식이다.
- 프록시 클래스에서 작은 단위의 작업을 처리하고, 리소스가 많이 요구되는 작업들이 필요할 경우만 주체 클래스를 사용하도록 구현한다.
- 지연 초기화 방식

### 보호 프록시

```java
class Proxy implements ISubject {
    private RealSubject subject; // 대상 객체를 composition
    boolean access; // 접근 권한

    Proxy(RealSubject subject, boolean access) {
        this.subject = subject;
        this.access = access;
    }

    public void action() {
        if(access) {
            subject.action(); // 위임
            /* do something */
            System.out.println("프록시 객체 액션 !!");
        }
    }
}

class Client {
    public static void main(String[] args) {
        ISubject sub = new Proxy(new RealSubject(), false);
        sub.action();
    }
}
```

- 통제 제어 목적으로 사용하는 프록시
- 객체 접근 제어를 위해 객체의 대리자 혹은 표시자를 제공한다.
- 주체 클래스에 대한 접근을 제어하기 위한 경우 **객체에 대한 접근 권한을 제어하거나 객체마다 접근 권한을 달리하고 싶을 경우 사용하는 패턴**
    - 프록시 클래스에서 클라이언트가 주체 클래스에 대한 접근을 허용할지 말지 결정하도록 할 수 있다.

### 스마트 참조자 프록시

- 실제 객체에 접근할 때 추가 행위를 부여하여 호출한다.
- 장식자 패턴과 유사하게 객체를 동적으로 확장할 때 응용한다.

### 로깅 프록시

```java
class Proxy implements ISubject {
    private RealSubject subject; // 대상 객체를 composition

    Proxy(RealSubject subject {
        this.subject = subject;
    }

    public void action() {
        System.out.println("로깅..................");
        
        subject.action(); // 위임
        /* do something */
        System.out.println("프록시 객체 액션 !!");

        System.out.println("로깅..................");
    }
}

class Client {
    public static void main(String[] args) {
        ISubject sub = new Proxy(new RealSubject());
        sub.action();
    }
}
```

- 대상 객체에 대한 로깅을 추가하려는 경우
- 프록시는 서비스 메서드를 실행하기 전에 로깅하는 기능을 추가하여 재정의한다.

### 캐싱 프록시

- 데이터가 큰 경우 캐싱하여 재사용을 유도
- 클라이언트 요청의 결과를 캐시하고 이 캐시의 수명 주기를 관리

## 프록시 패턴 사용 이유

### 흐름 제어

- 동기적인 처리를 최대한 비동기적으로 처리하기 위함이다.
- 프록시 객체를 사용하지 않는 경우를 생각해보자.
    
    ![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcRpTV4%2FbtrbvcbAts1%2FKwTMInBCA4Xv3uPKKu6KWK%2Fimg.png)
    
    - 많은 양의 리소스를 필요로하는 상황에서 디비 쿼리가 매우 느려질 수 있다.
    - 이럴 때 지연 초기화를 위한 코드를 작성해야하는데, 이를 모든 클래스마다 직접 넣어버리면 코드 중복이 발생한다.
- 다음과 같이 프록시 객체를 이용하면, 요청을 프록시 객체가 먼저 받은 후 흐름을 제어하여 DB에 쿼리를 날릴 수 있다.
    
    ![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdiJe86%2Fbtrbvbqbihc%2FdK8D61nvnzfzkW2ew4wYnK%2Fimg.png)
    

### 실제 메소드가 호출되기 이전에 필요한 기능(전처리 등)을 구현 객체 변경 없이 추가 가능하다.

- 코드 변경의 최소화

### 캐시를 사용할 수 있다.

- 프록시가 내부 캐시를 통해 데이터가 캐시에 존재하지 않는 경우에만 주체 클래스에서 작업이 실행되도록 할 수 있다.

## Dynamic Proxy

- Java JDK에서는 별도로 프록시 객체 구현 기능을 지원한다.
    - 이를 동적 프록시(Dynamic Proxy) 기법이라고 부른다.
- 개발자가 일일이 프록시 객체를 생성하는 것이 아니라 애플리케이션 실행 중 `java.lang.reflect.Proxy` 패키지에서 제공해주는 API를 이용하여 동적으로 프록시 인스턴스를 만들어 등록하는 방법으로서, 자바의 Reflection API 기법을 응용한 개념이다.

## 참고 자료

- https://hirlawldo.tistory.com/174
- [https://velog.io/@dev_leewoooo/Proxy-pattern이란-with-Java](https://velog.io/@dev_leewoooo/Proxy-pattern%EC%9D%B4%EB%9E%80-with-Java)
- [https://velog.io/@newtownboy/디자인패턴-프록시패턴Proxy-Pattern](https://velog.io/@newtownboy/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4-%ED%94%84%EB%A1%9D%EC%8B%9C%ED%8C%A8%ED%84%B4Proxy-Pattern)
- https://esoongan.tistory.com/180
- [https://inpa.tistory.com/entry/GOF-💠-프록시Proxy-패턴-제대로-배워보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%ED%94%84%EB%A1%9D%EC%8B%9CProxy-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90)