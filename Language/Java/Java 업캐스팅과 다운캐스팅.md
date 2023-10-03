## 자바의 참조형 캐스팅

- 하나의 데이터 타입을 다른 타입으로 바꾸는 것을 타입 변환 혹은 형변환(캐스팅)이라고 한다.
- 기본적으로 자바에서는 **대입 연산자 `=` 에서 변수와 값 서로 양쪽의 타입이 일치하지 않으면 할당이 불가능하다.**
    
    ```java
    long d = 10.233; // ERROR
    ```
    
- 따라서 다음과 같이 타입 캐스팅 연산자`(type)`를 사용하여 강제적으로 타입을 지정하여 변수에 대입하도록 설정할 수 있다.
    
    ```java
    long d = (long)10.233;
    ```
    

## 상속 관계에서의 참조형 캐스팅

<img width="671" alt="스크린샷 2023-05-03 오후 6 34 00" src="https://user-images.githubusercontent.com/96467030/235903836-30a5a83e-81a8-4515-879f-157833711998.png">

- 상속 관계의 클래스는 크게 **부모 클래스(슈퍼 클래스)와 자식 클래스(서브 클래스)**로 구분할 수 있다.
- 기본형 타입을 서로 형변환할 수 있듯이 **자바의 상속 관계에 있는 부모와 자식 클래스 간에는 서로 간의 형변환이 가능하다.**
- **클래스는 reference 타입으로 분류**되기 때문에 이를 **참조형 캐스팅(업캐스팅 / 다운캐스팅)**이라고 부른다.
- 자식 클래스의 객체는 부모 클래스를 상속하고 있기 때문에 부모 멤버를 모두 가지고 있지만, 부모 클래스의 객체는 자식 클래스의 멤버를 모두 가지고 있지 않다.
- 즉, **참조 변수의 형변환은 사용할 수 있는 멤버의 개수를 조절**하는 것이다.
    - 예를 들어, 기본형 타입의 형변환(실수 → 정수)은 값(3.6 → 3)이 바뀌게 된다.
    - 반면에 객체 형변환은 멤버 개수만 달라진다.

### 부모 / 자식 형변환 예제

```java
class Parent {
	String name;
    int age;
}

class Child extends Parent {
	/*
    String name;
    int age;
    */
	int number;
}

Parent p = new Parent(); 
Child c = new Child();

Parent p2 = (Parent)c; // 업캐스팅 - 자식에서 부모로
Child c2 = (Child)p2; // 다운캐스팅 - 부모에서 자식으로
```

- 자식 객체는 부모 객체의 상속을 받고 있으니 하위 요소로 판별된다.
    - 따라서 **자식 객체에서 부모 객체로 캐스팅하는 것을 업캐스팅**이라고 한다.
    - 반대로 **부모 객체를 자식 객체로 형변환 하는 것을 다운캐스팅**이라고 한다.

### 참조형 캐스팅 예제 - ArrayList 자료형 선언

```java
List<int> l = new ArrayList()<>;
```

- ArrayList를 List로 변수 타입을 선언해도 문제가 없는 이유는 ArrayList가 List를 부모 클래스로서 상속 받고 있기 때문이다.

## 업 캐스팅(Upcasting)

- 업캐스팅은 **자식 클래스가 부모 클래스 타입으로 캐스팅**되는 것이다.
- 업캐스팅은 **캐스팅 연산자 괄호를 생략할 수 있다.**
- 단, **부모 클래스로 캐스팅된다는 것은 멤버의 개수 감소**를 의미하며, 이는 **자식 클래스에만 있는 속성과 메서드는 실행하지 못한다.**
- 하지만 업캐스팅을 하고 메소드를 실행할 때 만약 자식 클래스에서 오버라이딩한 메서드가 있는 경우, **부모 클래스의 메서드가 아닌 오버라이딩 된 메서드가 실행**된다.

### 업캐스팅 예시

```java
class Unit {
    public void attack() {
        System.out.println("유닛 공격");
    }
}

class Zealot extends Unit {
    public void attack() {
        System.out.println("찌르기");
    }

    public void teleportation() {
        System.out.println("프로토스 워프");
    }
}

public class Main {
    public static void main(String[] args) {
    
      Unit unit_up;
      Zealot zealot = new Zealot();
        
      // * 업캐스팅(upcasting)
			unit_up = (Unit) zealot;
			unit_up = zealot; // 업캐스팅은 형변환 괄호 생략 가능
    }
}
```

- `Unit`을 상속하는 `Zealot` 자식 클래스가 있다.
- `zealot`을 객체 `unit_up`에 할당하는 것을 **업캐스팅**이라고 한다.

### 업캐스팅 멤버 제한

- 업캐스팅에서는 멤버 개수 감소로 인한 멤버 접근 제한을 주의해야한다.
- 부모를 상속해서 멤버가 많은 자식 클래스에서 부모 클래스로 업캐스팅 했으니 당연히 멤버 개수가 감소하게 되며, 이는 실행할 수 있는 속성과 메서드가 제한된다는 뜻이기도 하다.
- 위 코드에서 unit_up 변수에 zealot 변수를 할당하여 Unit 타입으로 업캐스팅 형변환이 되었기 때문에 오로지 부모 클래스에 속한 멤버만 접근이 허용되도록 제한된다.
- 즉, **메서드와 필드 모두 부모와 공통된 것만 사용할 수 있고, 자식 클래스에서 새로 만들어진건 사용할 수 없다.**

### 업캐스팅 오버라이딩 메서드

- 위 코드에서 자식 클래스는 `attack()` 메서드를 오버라이딩했다.
- 이러한 상황에서 업캐스팅 후 `attack()` 메서드를 실행하면 **부모 메서드가 아니라 오버라이딩한 자식 메서드를 사용한다.**
- 이는 오버라이딩 특성상 코드가 실행되는 런타임 환경에서 동적으로 바인딩되었기 때문이다.

## 다운 캐스팅(Downcasting)

- 다운 캐스팅은 거꾸로 **부모 클래스가 자식 클래스 타입으로 캐스팅**되는 것이다.
- 다운 캐스팅은 **캐스팅 연산자 괄호를 생략할 수 없다.**
- 다운 캐스팅의 목적은 **업캐스팅한 객체를 다시 자식 클래스 타입의 객체로 되돌리는데 목적을 둔다.**
- 다운캐스팅의 진정한 의미는 부모 클래스로 업캐스팅된 자식 클래스를 복구하며, 본인의 필드와 기능을 회복하기 위해 있는 것이다.
    - 즉, 원래 있던 기능을 회복하기 위해 다운캐스팅을 하는 것이다.

### 다운 캐스팅 예시

```java
class Unit {
    public void attack() {
        System.out.println("유닛 공격");
    }
}

class Zealot extends Unit {
    public void attack() {
        System.out.println("찌르기");
    }

    public void teleportation() {
        System.out.println("프로토스 워프");
    }
}

public class Main {
    public static void main(String[] args) {

        Unit unit_up;
        Zealot zealot = new Zealot();

        unit_up = zealot; // 업캐스팅
        
        // * 다운캐스팅(downcasting) - 자식 전용 멤버를 이용하기위해, 이미 업캐스팅한 객체를 되돌릴때 사용
        Zealot unit_down = (Zealot) unit_up; // 캐스팅 연산자는 생략 불가능. 반드시 기재
        unit_down.attack(); // "찌르기"
        unit_down.teleportation(); // "프로토스 워프"
    }
}
```

- 만약 업캐스팅된 객체 `unit_up`에서 자식 클래스에만 있는 `teleportation()` 메서드를 실행해야 하는 상황이 온다면, **다운캐스팅을 통해 회귀시킨 후 메서드를 실행하면 된다.**
- 이때, **다운 캐스팅은 캐스팅 연산자를 생략할 수 없다.**
    - 다운 캐스팅은 곧 사용할 수 있는 객체 멤버 증가를 의미하는데, 멤버의 증가는 불안전하다.
    - 실제 참조 변수가 가리키는 객체가 무엇인지 모르기 때문에 어떠한 멤버가 추가되는지 알 수 없기 때문이다.
    - 따라서 반드시 형변환 괄호를 기재함으로써 증가된 클래스의 멤버가 무엇인지 알게 하도록 개발자에게 알려줘야 한다.

### 다운 캐스팅 주의 사항

```java
Unit unit = new Unit();

// * 다운캐스팅(downcasting) 예외 - 다운캐스팅은 업스캐팅한 객체를 되돌릴때 적용 되는것이지, 오리지날 부모 객체를 자식 객체로 강제 형변환은 불가능
Zealot unit_down2 = (Zealot) unit; //! RUNTIME ERROR - Unit cannot be cast to Zealot
unit_down2.attack(); //! RUNTIME ERROR
unit_down2.teleportation(); //! RUNTIME ERROR
```

- 다운 캐스팅의 목적은 **업캐스팅한 객체를 되돌리는데 있다.**
- 따라서 다음처럼 **업캐스팅이 되지 않는 생 부모 객체 unit일 경우, 이를 그대로 다운캐스팅하면 오류가 발생한다.**
    
    ```java
    Zealot unit_down = new Unit(); // 참조 다형성 위배
    ```
    
    - 이러한 다운 캐스팅 특성은 원래 참조 다형성에서도 불가능했기 때문에 발생한다.
- 이러한 다운 캐스팅 특성에 대해 주의해야 하는 이유는 에디터에서 **컴파일 에러가 발생하지 않고 런타임 에러가 발생하는 위험성이 존재하기 때문이다.**

## instanceof

```java
class Unit {
    // ...
}

class Zealot extends Unit {
    // ...
}

public class Main {
    public static void main(String[] args) {

        // * 업캐스팅 유무
        Zealot zealot = new Zealot();

        if (zealot instanceof Unit) {
            System.out.println("업캐스팅 가능"); // 실행
            Unit u = zealot; // 업캐스팅
        } else {
            System.out.println("업캐스팅 불가능");
        }
        
        // * 다운스캐팅 유무
        Unit unit = new Unit();
        Unit unit2 = new Zealot();

        if (unit instanceof Zealot) {
            System.out.println("다운캐스팅 가능");
        } else {
            System.out.println("다운캐스팅 불가능"); // 실행
        }

        if (unit2 instanceof Zealot) {
            System.out.println("다운캐스팅 가능"); // 실행
            Zealot z = (Zealot) unit2; // 다운캐스팅
        } else {
            System.out.println("다운캐스팅 불가능");
        }
    }
}
```

- 어느 객체 변수가 어느 클래스 타입인지 판별해 `true / false`를 반환해준다.
- 사용시 주의할 점은 `instanceof` 연산자는 객체에 대한 클래스(참조형) 타입에만 사용할 수 있다. (primitive 타입 불가능)

## 참고 자료

- [https://inpa.tistory.com/entry/JAVA-☕-업캐스팅-다운캐스팅-한방-이해하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%97%85%EC%BA%90%EC%8A%A4%ED%8C%85-%EB%8B%A4%EC%9A%B4%EC%BA%90%EC%8A%A4%ED%8C%85-%ED%95%9C%EB%B0%A9-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)