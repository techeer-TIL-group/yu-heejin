## takeIf

```kotlin
inline fun <T> T.takeIf(predicate: (T) -> Boolean): T?
```

- 내부 식이 `true`이면 결과값을 반환하며, 그렇지 않으면 `null`을 반환한다.
- 파라미터로 자기 자신 `T` 객체를 전달 받고, 조건 함수 `predicate`에 따라 `T` 객체인 자기 자신이나 `null`을 반환한다.

## takeUnless

```kotlin
inline fun <T> T.takeUnless(predicate: (T) -> Boolean): T?
```

- 내부 식이 `false`이면 결과값을 반환하며, 만족하면 `null`을 반환한다.
- `takeIf` 함수와 반대로 동작하는데, 조건 함수가 `true`일 때 `null`을 반환하고 `false`일 때 자기 자신을 반환한다.

## takeIf / takeUnless

```kotlin
fun getBearerToken(request: HttpServletRequest): String {
  val token = request.getHeader(TOKEN_HEADER_NAME)
      ?: throw TokenNotFountException()

  return token.takeIf { it.startsWith(TOKEN_HEADER_PREFIX) }?.substring(7)
      ?: throw TokenNotFountException()
}
```

- takeIf, takeUnless 함수 모두 람다 인수 `it`을 사용할 수 있다.
- 사용 시 `null`을 반환할 가능성이 있기 때문에 `?.` 와 같은 null safety 연산자를 사용해야 한다.

## 참고 자료

- https://kotlinlang.org/docs/scope-functions.html#takeif-and-takeunless
- [https://medium.com/@limgyumin/코틀린-의-takeif-takeunless-는-언제-사용하는가-f6637987780](https://medium.com/@limgyumin/%EC%BD%94%ED%8B%80%EB%A6%B0-%EC%9D%98-takeif-takeunless-%EB%8A%94-%EC%96%B8%EC%A0%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EA%B0%80-f6637987780)