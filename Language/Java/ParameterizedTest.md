## @ParameterizedTest

- 여러 개의 테스트를 한 번에 작성하기 위한 어노테이션
    - 여러 개의 테스트 케이스를 사용할 수 있다.
- `@Test` 어노테이션 대신 `@ParameterizedTest`라는 어노테이션을 사용한다.
- 이 때 **파라미터로 넘겨줄 값들을 지정**해주어야 하는데, 이 역시 **어노테이션을 사용해서 테스트에 주입해줄 수 있다.**

## @ValueSource

```java
@ParameterizedTest
@DisplayName("ValueSource를 이용한 User 생성 테스트")
@ValueSource(strings = {"", " "})
void createUserException(String text) {
    assertThatThrownBy(() -> new User(text))
            .isInstanceOf(IllegalArgumentException.class);
}
```

- 테스트에 주입할 값을 어노테이션에 배열로 지정한다.
- 테스트를 실행하면 배열을 순회하면서 **테스트 메소드에 인수로 지정된 배열 값들을 주입해 테스트한다.**
- **하나의 테스트에는 하나의 인수만 전달할 수 있다.**

## @CsvSource

```java
int multiplyBy2(int number) {
    return number * 2;
}

@ParameterizedTest
@CsvSource(value = {"1,2", "2,4", "3,6"})
void multiplyBy2Test(int input, int expected) {
    assertThat(multiplyBy2(input)).isEqualTo(expected);
}
```

- `input`에 따라 `expected` 값이 다르게 나오는 케이스를 여러 개 테스트할 수 있다.
- `value`에 값들을 넣어주며, `“{input},{expected}”`의 형태로 구분자가 있는 문자열을 입력해주어야 한다.
- 기본 구분자는 콤마(`,`)이며, 다른 구분자를 지정하고 싶다면 다음과 같이 수행한다.
    
    ```java
    @ParameterizedTest
    @CsvSource(value = {"1:2", "2:4", "3:6"}, delimiter = ':')
    void multiplyBy2Test(int input, int expected) {
        assertThat(multiplyBy2(input)).isEqualTo(expected);
    }
    ```
    
    - 이 떄 `delimiter`는 String이 아닌 char임을 주의해야한다.

## @NullSource, @EmptySource, NullAndEmptySource

```java
@ParameterizedTest
@DisplayName("null 값 또는 empty 값으로 User 생성 테스트")
@NullAndEmptySource
void createUserExceptionFromNullOrEmpty(String text) {
    assertThatThrownBy(() -> new User(text))
            .isInstanceOf(IllegalArgumentException.class);
}
```

- `@NullSource`는 테스트 메소드에 인수로 `null`을 주입한다.
- `@EmptySource`는 빈 값을 주입한다.
- `@NullAndEmptySource`는 `null`과 빈 값을 모두 주입한다.
- 원시값의 경우 `null` 값이 들어갈 수 없으므로, 메소드의 인수가 원시 값이라면 `@NullSource`, `@NullAndEmptySource`는 사용할 수 없다.
- `@ValueSource`와 같이 사용 가능하다.
    
    ```java
    @ParameterizedTest
    @DisplayName("null 값 또는 empty 값으로 User 생성 테스트")
    @NullAndEmptySource
    @ValueSource(strings = {""," "})
    void createUserExceptionFromNullOrEmpty(String text) {
        assertThatThrownBy(() -> new User(text))
                .isInstanceOf(IllegalArgumentException.class);
    }
    ```
    

## @EnumSource

```java
@ParameterizedTest
@DisplayName("6, 7월이 31일까지 있는지 테스트")
@EnumSource(value = Month.class, names = {"JUNE", "JULY"})
void isJuneAndJuly31(Month month) {
    assertThat(month.minLength()).isEqualTo(31);
}
```

- `Enum` 클래스의 모든 값을 사용하려면 `@EnumSource` 안에 `Enum` 클래스만 전달해주면 되고, 특정 값만 필요한 경우 `value`에 클래스를 넣어주고 `names`에 선택할 값의 이름을 전달해준다.
- 이 때 names까지 값을 넣으면 추가로 mode 값도 지정해줄 수 있다.
    
    
    | Mode | 설명 |
    | --- | --- |
    | INCLUDE: names.contains(name) | name과 일치하는 모든 Enum값 (default) |
    | EXCLUDE: !names.contains(name) | name을 제외한 모든 Enum값 |
    | MATCH_ANY: patterns.stream().anyMatch(name::matches) | 조건을 하나라도 만족하는 Enum값 |
    | MATCH_ANY: patterns.stream().allMatch(name::matches) | 조건을 모두 만족하는 Enum값 |

## @MethodSource

```java
@ParameterizedTest
@MethodSource("provideStringsForIsBlank")
void isBlank_ShouldReturnTrueForBlankStrings(String input, boolean expected) {
    assertThat(input.isBlank()).isEqualTo(expected);
}

private static Stream<Arguments> provideStringsForIsBlank() {
    return Stream.of(
            Arguments.of("", true),
            Arguments.of("  ", true),
            Arguments.of("not blank", false)
    );
}
```

- `method`를 인수로 전달하여 복잡한 `Object`를 전달할 수 있다.
- `@MethodSource`에 작성하는 메소드 이름은 인수로 제공하려는 메소드 이름과 같아야한다.
- 인수로 제공하려는 메서드는 `static`이어야 한다.
    - 단, `@TestInstance(Lifecycle.PER_CLASS)`를 사용하여 클래스 단위 생성주기일 경우 인스턴스 메소드 제공이 가능하다.
- `@MethodSource`에 메소드 이름을 작성해주지 않을 경우 JUnit이 테스트 메소드 네임과 같은 메소드를 찾아서 인수로 제공한다.
- 만약 테스트 호출 당 하나의 인수만 제공하고자 한다면 Arguments로 추상화할 필요가 없다.
- argument source method는 `Stream<Arguments>`를 반환하는 것이 기본이지만, `List`와 같은 컬렉션이나 인터페이스를 반환할 수도 있다.
- 정규화된 이름(FQN#methodName의 형식)으로 외부 정적 메소드를 참조할 수 있다.
    
    ```java
    class StringsUnitTest {
        @ParameterizedTest
        @MethodSource("parameterizedtest.StringParams#blankStrings")
        void isBlank_ShouldReturnTrueForBlankStringsExternalSource(String input) {
            assertThat(input.isBlank()).isTrue();
        }
    }
    
    // StringParams 클래스는 parameterizedtest 패키지에 있다고 가정
    public class StringParams {
        static Stream<String> blankStrings() {
            return Stream.of("", "  ");
        }
    }
    ```
    

## 참고 자료

- https://velog.io/@ohzzi/junit5-parameterizedtest