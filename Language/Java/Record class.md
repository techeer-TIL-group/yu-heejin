> 불변(immutable) 객체를 쉽게 생성할 수 있도록 하는 클래스
> 

# 일반 불변 클래스

```java
public class Student {
	private final String name;
	private final int age;
	
	// 생성자, getter/setter
	...
}
```

불변 객체를 생성하려면 다음과 같이 코드를 작성해야 한다.

1. 모든 필드에 `final` 선언
2. 필드 값을 모두 포함한 생성자
3. 접근자 메서드(getter)
4. 클래스의 상속을 제한하려면 클래스 레벨에도 `final` 선언

# 레코드 클래스

```java
public record Student(String name, int age) {}
```

`record`를 사용하면 아래 내용들을 직접 구현하지 않아도 컴파일 타임에 컴파일러가 코드를 추가해주기 때문에 자동으로 생성된다.

1. 필드 캡슐화
2. 모든 field를 포함하는 생성자 메서드
3. getter
    - 기존 클래스와의 차이점이 있다면, getter를 사용할 때 `getFieldName()`이 아니라 `fieldName()`을 사용한다.
4. `equals`, `hashcode`
5. `toString`

## 특징

- 생성자는 모든 field를 포함한다.
- `toString()`도 모든 field를 포함한다.
- `equals()`, `hashCode()`는 invokedynamic based mechanism을 사용한다.
- getter는 field 이름과 같은 이름으로 생성된다.
- 기본적으로 `java.lang.Record` class를 상속받기 때문에 다른 class를 상속받을 수 없다.
- class가 `final`이기 때문에 다른 subclass를 생성할 수 없다.
- 모든 field는 불변이기 때문에 setter는 제공하지 않는다.

# 참고 자료

- https://mr-popo.tistory.com/243
- [https://velog.io/@rmswjdtn/JAVA-문법-Record-Class](https://velog.io/@rmswjdtn/JAVA-%EB%AC%B8%EB%B2%95-Record-Class)
