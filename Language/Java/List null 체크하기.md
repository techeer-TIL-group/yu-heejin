# List.isEmpty()

```java
boolean isEmpty()
```

- 해당 리스트 객체가 아무런 요소도 가지고 있지 않을 때(리스트가 비어 있을 때) `true`를 반환한다.

```java
List<String> list = null;
if (list.isEmpty()) { ... }     // NullPointerException 발생
```

- List 객체가 null일 경우, `isEmpty()` 호출 시 오류가 발생하기 때문에 List가 null인지 여부도 함께 확인해야 한다.

# List.size()

```java
int size()
```

- 해당 리스트의 요소 개수를 반환한다.
- List가 비어 있는 경우 0을 반환하며, 요소의 개수가 `Integer.MAX_VALUE` 보다 많을 경우 `Integer.MAX_VALUE`를 반환한다.

```java
List<String> list = null;
if (list.size()) { ... }     // NullPointerException 발생
```

- `isEmpty()`와 마찬가지로 List 객체가 null일 경우 오류가 발생한다.

# CollectionUtils.isEmpty()

```java
//Return true if the supplied Collection is null or empty. Otherwise, return false;
public static boolean isEmpty(@Nullable Collection<?> collection) {
    return (collection == null || collection.isEmpty());
}

//Return true if the supplied Map is null or empty. Otherwise, return false;
public static boolean isEmpty(@Nullable Map<?, ?> map) {
    return (map == null || map.isEmpty());
}
```

- 스프링 프레임워크를 사용하는 경우 해당 메서드를 사용할 수 있다.
- 해당 메서드는 **인자로 전달된 객체가 null이거나 empty일 때 true를 반환한다.**

# 참고 자료

- https://wildeveloperetrain.tistory.com/279#google_vignette
