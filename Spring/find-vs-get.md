## Optional<T> findById(ID id)

- ‘탐색하다’
- 탐색 결과가 없을 수 있다.
- 내부 예외 발생 없음

## T getOne(ID id)

- ‘가져오다’
- 가져오려는 대상이 없는 경우 내부에서 예외 발생 (`EntityNotFoundException`)
- `getOne`은 deprecated 되었기 때문에 `getReferenceById`가 권장된다.
- `getReferenceById`는 `EntityManager#getReference`를 사용하며, 조회된 entity 내부값 접근 전까지 lazy loading 처리한다.

---

## findById

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbP9eCg%2FbtrCXyywK5e%2FZE88wdIlDq6mzPkJMIPbhk%2Fimg.png)

- 매개변수로 전달된 id에 해당하는 entity를 반환하거나, 해당 결과가 없는 경우 Optional.empty()를 반환한다.
    - 즉, 탐색 결과가 없더라도 내부에서 예외를 발생시키지 않는다.
- 단, `id == null` 인 경우 `IllegalArgumentException`이 발생할 가능성이 있다.

## GetOne

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJEtcK%2FbtrCXRSAmIg%2FHDOeuAKWNtY5fQ876TGxZ1%2Fimg.png)

- `Optional<T>`가 아닌 `T`가 반환값이다.
- 매개변수로 전달된 ID에 해당하는 entity를 반환하나 없을 경우 내부에서 예외를 발생시킨다.

## getReferenceById

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FQfSdu%2FbtrCXl7vocY%2FkjECoRD84JqazvzvUGnK41%2Fimg.png)

- getOne과 동일하다.

## getOne, getReferenceById의 lazy loading

- `getOne`, `getReferenceById` 메서드로 반환된 객체 내부의 정보를 요청하는 시점에 그제서야 DB를 조회한다.
    - 즉, 내부의 값을 필요로 하지는 않고, 다른 객체에게 할당하는 목적으로만 조회하는 경우 getReferenceById는 EntityManager의 getReference 메서드를 호출하여 참조값만 가져온 후, **조회된 entity의 내부 값이 필요해지는 시점에 lazy loading으로 DB를 조회해 값을 가져오도록 동작한다.**
- `getOne`보다는 `getReferenceById`가 참조값만 가져온다는 의미를 더 정확히 전달해준다.

## 참고 자료

- [https://creampuffy.tistory.com/162](https://creampuffy.tistory.com/162)