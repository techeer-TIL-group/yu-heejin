## DTO (Data Transfer Object)

```java
public class MemberDto {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

- DTO는 **데이터를 전달하기 위한 객체**이다.
- 계층간 데이터를 주고받을 때, 데이터를 담아서 전달하는 바구니로 생각할 수 있다.
- 여러 레이어 사이에서 DTO를 사용할 수 있지만, 주로 **View와 Controller 사이에서 데이터를 주고 받을 때 활용한다.**
- DTO는 `getter/setter` 메소드를 포함하며, **이 외의 비즈니스 로직은 포함하지 않는다.**
    - setter를 가지는 경우는 가변 객체로 활용한다.
- setter가 아닌 생성자를 이용해 초기화하는 경우에는 불변 객체로 활용할 수 있다.
    
    ```java
    public class MemberDto {
        private final String name;
        private final int age;
    
        public MemberDto(String name, int age) {
            this.name = name;
            this.age = age;
        }
    
        public String getName() {
            return name;
        }
    
        public int getAge() {
            return age;
        }
    }
    ```
    
    - 불변 객체로 만들면 **데이터를 전달하는 과정에서 데이터가 변조되지 않음을 보장할 수 있다.**

## VO (Value Object)

```java
// Money.java
public class Money {
    private final String currency;
    private final int value;

    public Money(String currency, int value) {
        this.currency = currency;
        this.value = value;
    }

    public String getCurrency() {
        return currency;
    }

    public int getValue() {
        return value;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Money money = (Money) o;
        return value == money.value && Objects.equals(currency, money.currency);
    }

    @Override
    public int hashCode() {
        return Objects.hash(currency, value);
    }
}
```

- VO는 **값 자체를 표현**하는 객체이다.
- VO는 **객체들의 주소가 달라도 값이 같으면 동일한 것으로 여긴다.**
    - 예를 들어, 고유 번호가 서로 다른 만원 2장이 있다고 가정하면, 이 둘은 고유번호(주소)는 다르지만 10000원(값)은 동일하다.
- VO는 `getter` 메소드와 함께 비즈니스 로직도 포함할 수 있다.
    - **단, `setter`는 가지지 않는다. (값 자체를 표현하므로 무조건 불변)**
    - 값 비교를 위해 `**equals()`와 `hashCode()` 메소드를 오버라이딩** 해줘야 한다.
        - VO는 주소가 아닌 값을 비교하기 때문이다.

## Entity

```java
public class Member {
    private final Long id;
    private final String email;
    private final String password;
    private final Integer age;

    public Member() {
    }

    public Member(Long id, String email, String password, Integer age) {
        this.id = id;
        this.email = email;
        this.password = password;
        this.age = age;
    }
}
```

- **DB 테이블과 매핑되는 핵심 클래스**이며, 이를 기준으로 테이블이 생성되고 스키마가 변경된다.
    - 따라서 절대로 **Entity를 요청이나 응답값을 전달하는 클래스로 사용해서는 안된다.**
- Entity는 `id`로 구분되며 비즈니스 로직을 포함할 수 있다.
- DTO처럼 `setter`를 가지는 경우 가변 객체로 활용할 수 있다.

## 세 객체 비교

| DTO | VO | Entity |
| --- | --- | --- |
| 레이어간 데이터 전송용 객체 | 값 표현용 객체 | DB 테이블 매핑용 객체 |
| 가변 또는 불변 객체 | 불변 객체 | 가변 또는 불변 객체 |
| 로직을 포함할 수 없다. | 로직을 포함할 수 있다. | 로직을 포함할 수 있다. |

## 참고 자료

- https://tecoble.techcourse.co.kr/post/2021-05-16-dto-vs-vo-vs-entity/
- https://hyeon9mak.github.io/DTO-vs-VO/