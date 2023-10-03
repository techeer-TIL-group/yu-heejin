## Optional Class

- NPE(Null Pointer Exception)를 방지할 수 있도록 도와준다.
- **null이 올 수 있는 값을 감싸는 Wapper 클래스**, 참조하더라도 NPE가 발생하지 않도록 도와준다.

## Optional 올바르게 사용하기

### Optional 변수에 null을 할당하지 말 것

```java
// Bad
Optional<Person> findById(Long id) {
    // find person from db
    if (result == 0) {
        return null;
    }
}

// Good
Optional<Person> findById(Long id) {
    // find person from db
    if (result == 0) {
        return Optional.empty();
    }
}
```

- 반환 값으로 null을 사용하지 않기 위해 등장한 클래스이기 때문에 `Optional` 객체에 null을 반환하는 것은 `Optional` 의도와 맞지 않는다.
- ‘결과 없음’을 표현해야 하는 경우라면 null 대신 `Optional.empty()`를 반환하는 것이 좋다.

### Optional.get() 호출 전 Optional 객체가 값을 가지고 있음을 확실히 할 것

```java
// bad
Optional<Person> maybePerson = findById(4);
String name = maybePerson.get().getName();

// not bad but...
Optional<Person> maybePerson = findById(4);
if (myabePerson.ifPresent()) {
    return maybeperson.get();
}
return UNKNOWN_PERSON;

// good
Person person = findById(4).orElseThrow(PersonNotFoundException::new);
String name = person.getName();
```

### orElse(new …)대신 orElseGet(() → new …)

```java
// good
// UNKNOWN_PERSON is pre-defined object for case that no person is found that id matches
Person person = findById(4).orElse(UNKNOWN_PERSON);

// warn
findById(4).orElse(new Person());
```

- 값이 없는 상황을 처리할 방법으로 **기본값 반환, 예외 던지기** 두 가지 방법을 사용한다.
- `Optional.orElse()`는 ‘기본값 반환’에 사용하는 메서드이다.
    - `Optional` 객체에 값이 없는 경우 `orElse`의 인자로 명시된 값을 대신 반환한다.
- `findById(4).orElse(new Person());` 코드의 경우 Optional 객체가 비어있지 않은 경우에도 Person 생성자를 호출한다.
- `orElse(…)`에서 **…는 `Optional`에 값이 있든 없는 무조건 실행된다.**
    - `orElse()`의 **경우 값의 유무와 관계 없이 실행되며, `Optional`에 값이 있으면  인자의 실행 값이 무시되고 버려진다.**
    - 따라서 새로운 객체를 생성하거나 새로운 연산을 수행하는 경우에는 `orElse()`대신 `orElseGet()`을 써야한다.
    - `orElse()`는 **이미 생성되었거나 이미 계산된 값일 때만 사용하는 것이 좋다.**
- orElseGet()은 Optional에 값이 없을 때만 실행되기 때문에 불필요한 오버헤드가 없다.

### 값이 없는 경우, Optional.orElseGet()을 통해 이를 나타내는 객체를 제공할 것

```java
findById(4).orElseGet(() -> new Person("UNKNOWN")); // construct with named 'UNKNOWN'
```

- 값이 없는 경우 매번 새로운 객체를 반환해야 하는 경우 `Optional.orElseGet()`을 사용할 수 있다.
    - `orElse`가 기본값으로 반환할 값을 인자로 받는 것과 달리, `orElseGet()`은 값이 없는 경우 이를 대신해 반환할 값을 생성하는 람다를 인자로 받는다.
- `orElse`에 비해 값이 없는 경우에만 인자로 전달된 코드가 실행된다.
    - 이 방법으로 매번 새로운 객체를 생성하는 경우에는 실제로 값이 없는 경우에만 객체를 생성한다는 의미이다.

### 값이 없는 경우 Optional.orElseThrow()를 통해 명시적으로 예외를 던질 것

```java
findById(4).orElseThrow(() -> new NoSuchElementException("Person not found"));
```

- 값이 없는 경우 기본 값을 반환하는 대신 예외를 던져야하는 경우도 있는데, 이 경우 `Optional.orElseThrow()`를 사용하면 된다.
- 근데.. NPE 안내려고 하는건데 예외를 던지는 이유는?

### isPresent()-get() 대신 orElse()/orElseGet()/orElseThrow()

```java
// 안 좋음
Optional<Member> member = ...;
if (member.isPresent()) {
    return member.get();
} else {
    return null;
}

// 좋음
Optional<Member> member = ...;
return member.orElse(null);

// 안 좋음
Optional<Member> member = ...;
if (member.isPresent()) {
    return member.get();
} else {
    throw new NoSuchElementException();
}

// 좋음
Optional<Member> member = ...;
return member.orElseThrow(() -> new NoSuchElementException());
```

- 가독성이 훨씬 좋아진다.

### 값이 있는 경우 이를 사용하고 없는 경우 아무 동작도 하지 않는다면 Optional.ifPresent()를 활용할 것

```java
findById(4).ifPresent((user) -> System.out.println(user.getName()));
```

- `Optional.ifPresent()`는 `Optional` 객체 안에 값이 있는 경우 실행할 람다를 인자로 받는다.
- 값이 있는 경우 실행되고 없는 경우 실행되지 않는 로직에 `ifPresent`를 활용할 수 있다.

### Optional을 필드의 타입으로 사용하지 말 것

```java
// bad
class Person {
    Optional<String> address;
}

// bad
void printUserName(Optional<Person> maybePerson) {
    // ...
}
```

- `Optional`은 반환 타입을 위해 설계된 타입이다.
- 또한 `Serializable`도 아니기 때문에 `Optional`을 메서드의 인자로 사용하거나 클래스의 인자로 선언하는 것은 `Optional` 도입 의도에 반하는 패턴이다.

### Optional을 빈 컬렉션이나 배열을 반환하는데 사용하지 말것

```java
// bad
Optional<List<Person>> findByLastName(String lastName) {
    // ...
}
```

- 컬렉션, 배열이 ‘결과 없음’을 가장 명확하게 나타내는 방법은 **빈 컬렉션이나 빈 배열을 반환하는 것이다.**
- 따라서 `Optional`을 사용할 때의 이점이 없다.

### Optional을 컬렉션의 원소로 사용하지 말 것

```java
// bad
class WordCounter {
  private Map<String, Optional<Integer>> wordCounts = new HashMap<>();

  void addCount(String word) {
    wordCounts.put(word, Optional.of(wordCounts.get(word)
      .orElse(0) + 1));
  }

  Optional<Integer> getCount(String word) {
   return wordCounts.get(word);
  }
}

// good
class WordCounter {
  private Map<String, Integer> wordCounts = new HashMap<>();

  void addCount(String word) {
    wordCounts.putIfAbsent(word, 0);
    wordCounts.computeIfPresent(word, (word, cnt) -> cnt + 1);
  }

  int getCount(String word) {
    return Optional.ofNullable(wordCounts.get(word))
      .orElse(0);
  }
}
```

- `Optional`은 불변 객체이기 때문에 bad 코드의 경우 매번 새로운 값을 감싸는 `Optional` 객체를 `addCount` 메서드가 호출될 때마다 생성하게 된다.
    - 이는 메모리 문제를 일으키는 원인이 될 수 있다.

### 원시타입의 Optional에는 OptionalInt, OptionalLong, OptionalDouble 사용을 고려할 것

```java
OptionalInt maybeInt = OptionalInt.of(2);
OptionalLong maybeLong = OptionalLong.of(3L);
OptionalDouble maybeDouble = OptionalDouble.empty();
```

- 원시 타입을 `Optional`로 사용해야할 때는 **박싱과 언박싱을 거치면서 오버헤드가 생긴다.**
- 반드시 `Optional<T>`를 사용해야하는 경우가 아니라면, `int`, `long`, `double` 등에는 `OptionalXXX` 타입을 사용하는 것이 좋다.
- 이들은 내부 값을 래퍼 클래스가 아닌 원시 타입으로 갖고, 값의 존재 여부를 나타내는 `isPresent` 필드를 함께 갖는 구현체들이다.

### 내부 값의 비교에는 Optional.equals 사용을 고려할 것

```java
// Optional.equals의 구현
@Override
public boolean equals(Object obj) {
  if (this == obj) {
      return true;
  }

  if (!(obj instanceof Optional)) {
      return false;
  }

  Optional<?> other = (Optional<?>) obj;
  return Objects.equals(value, other.value);
}
```

- 기본적인 참조 확인, 타입 확인 이후 **두 `Optional`의 동치성은 내부 값의 `equal` 구현이 결정한다.**
    - 즉, `Optional` 객체 a, b에 대해 `a.equals(b)`가 `true`이면 그 역도 성립한다.
- 굳이 내부 값의 비교를 위해 값을 꺼낼 필요가 없다.
    
    ```java
    // bad
    boolean comparePersonById(long id1, long id2) {
      Optional<Person> maybePersonA = findById(id1);
      Optional<Person> maybePersonB = findById(id2);
      if (!maybePersonA.isPresent() && !maybePersonB.isPresent()) { return false; }
      if (maybePersonA.isPresent() && maybePersonB.isPresent()) {
          return maybePersonA.get().equals(maybePersonB.get());
      }
      return false;
    }
    
    // good
    // returns false if both person objects are absent
    boolean comparePersonById(long id1, long id2) {
      Optional<Person> maybePersonA = findById(id1);
      Optional<Person> maybePersonB = findById(id2);
      if (!maybePersonA.isPresent() && !maybePersonB.isPresent()) { return false; }
      return findById(id1).equals(findById(id2));
    }
    ```
    

### of(), ofNullable() 혼동 주의

```java
// 안 좋음
return Optional.of(member.getEmail());  // member의 email이 null이면 NPE 발생

// 좋음
return Optional.ofNullable(member.getEmail());

// 안 좋음
return Optional.ofNullable("READY");

// 좋음
return Optional.of("READY");
```

- `of(x)`는 `x`가 null이 아님이 확실할 때만 사용해야하며, `x`가 null인 경우 NPE가 발생한다.
- `ofNullable(x)`는 `x`가 null일 수도 있을 때만 사용하며, `x`가 null이 아님이 확실하면 `of(x)`를 사용한다.

## 참고 자료

- https://www.latera.kr/blog/2019-07-02-effective-optional/
- [https://homoefficio.github.io/2019/10/03/Java-Optional-바르게-쓰기/](https://homoefficio.github.io/2019/10/03/Java-Optional-%EB%B0%94%EB%A5%B4%EA%B2%8C-%EC%93%B0%EA%B8%B0/)