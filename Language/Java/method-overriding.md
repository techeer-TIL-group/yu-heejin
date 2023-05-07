## Method Overriding

- 상속 관계에 있는 **부모 클래스에 정의된 메소드를 자식 클래스에서 다시 정의**한 것
- 자바에서 자식 클래스는 부모 클래스의 private 멤버를 제외한 모든 것을 갖추고 있다.
    - 정학히 말하면 **private도 상속은 가능하지만 접근은 불가능**하다.
- 상속 받은 메서드는 그대로 사용할 수 있지만, **자식 클래스에서 필요로 한다면 같은 이름과 같은 매개변수의 메서드로 다시 정의해서 사용할 수 있다.**

## 오버라이딩 조건

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbMpqou%2FbtqGAwZfAia%2FmnA0EMxGK2ZunxOGS7KK61%2Fimg.png)

- 오버라이딩은 **메서드의 동작만을 재정의**하는 것이다.
    - 따라서 **메서드의 선언(반환 타입, 메서드 이름, 매개 변수)은 부모 클래스의 메서드와 같아야 한다.**
- 접근 지정자의 경우 **부모 클래스의 메서드보다 같거나 더 넓은 범위로만 지정**할 수 있다.
- 예외 클래스의 경우 **부모 클래스의 메서드보다 같거나 더 좁은 범위로만 지정**할 수 있다.

## 사용 예시

```java
// Employee.java - 부모 클래스
public class Employee {
    private String name;
    private int salary;

    public Employee() { // 기본 생성자
    }

    public Employee(String name, int salary) { // name과 salary를 인자로 받는 생성자 #2
        this.name = name;
        this.salary = salary;
    }
    
    public String getEmployee() {
        return name + " " + salary;
    }
}

// Manager.java - 자식 클래스
public class Manager extends Employee { // Employee 클래스 상속
    private String depart; // 자식 클래스만의 멤버 변수

    public Manager(String name, int salary, String depart) {
        super(name, salary); // 부모의 생성자 #2 를 호출하여 부모 객체 먼저 생성
        this.depart = depart; // 자식 클래스만의 멤버 변수 초기화
    }
    
    @Override
    public String getEmployee() { // 부모 클래스의 getEmployee() 메서드 재정의
    
        // 부모 클래스의 getEmployee() 메서드 반환 값에 자신의 멤버 변수 값 추가 후 반환
        return super.getEmployee() + " " + depart;
    }
}

// Main.java
public class Main {
    public static void main(String[] args) {
        Employee e = new Employee("홍길동", 2000); // 부모 객체 생성
        System.out.println(e.getEmployee()); // 부모 객체의 getEmployee() 메서드 호출

        // 다형성에 의해 부모 클래스 타입이 자식 객체를 가리킬 수 있음
        e = new Manager("이순신", 4000, "개발"); // 자식 객체 생성
        
        // 동적 바인딩, 자동으로 오버라이딩 된 자식 객체의 getEmployee() 메서드 호출
        System.out.println(e.getEmployee());
    }
}

// 실행 결과
홍길동 2000
이순신 4000 개발
```

## 참고 자료

- [https://dev01.tistory.com/73](https://dev01.tistory.com/73)