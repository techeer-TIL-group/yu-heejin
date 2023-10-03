## Arrays.asList()

- 일반 배열(정적 배열)을 ArrayList로 변환한다.
- `Arrays`의 private 정적 클래스인 `ArrayList`를 리턴한다.
    - 즉, `java.util.ArrayList` 클래스와는 다른 클래스이다
- `java.util.Arrays.ArrayList`는 `set()`, `get()`, `contains()` 메서드를 가지고 있지만, 원소를 추가하는 메서드는 가지고 있지 않기 때문에 **사이즈를 바꿀 수 없다.**

## Arrays.asList와 List.of

### 리스트 변경 가능 여부

- 생성자로 리스트를 만드는 것과 달리 메서드를 통해 리스트를 만들 경우 반환되는 리스트들은 **불변 리스트**이다.
    - 따라서 요소 추가 및 삭제가 불가능하다.
- `Arrays.asList()`는 요소를 변경하는 `set()`이 동작하지만, `List.of()`는 불가능하다.
    - `List.of()`를 통해 생성된 리스트는 완전한 불변 리스트이다.

### 반환 리스트

- `Arrays.asList()` 메서드의 구현부를 보면 `ArrayList`는 `Arrays` 클래스 내부에 구현된 또다른 별개의 `ArrayList`임을 알 수 있다.
- `List.of()`로 반환되는 리스트도 `ArrayList`가 아닌 새로운 `ImmutableCollections` 객체의 내부 클래스인 `ListN`객체를 생성하는 것을 볼 수 있다.
    - 해당 클래스에도 add, remove, set에 대해 구현되어있지 않다.

### 불변 리스트인 이유?

- 불변 객체만의 이점을 이용한 다른 컬렉션 자료구조로 변환이 용이하기 때문이다.
- 불변 객체의 이점
    1. 스레드 안전성: 불변 객체는 동기화 없이도 여러 스레드에서 안전하게 공유하고 액세스 할 수 있다.
    2. 코드 간소화: 불변 객체는 동시성을 위해 설계할 필요가 없으므로 코드가 간소화되고 버그 가능성이 낮다.
    3. 향상된 성능: 변경 불가능한 객체는 항상 동일한 상태를 유지하므로 캐시하고 재사용할 수 있다.

### 리스트 내부 배열 참조 여부

- `List.of`는 참조한 원본 배열의 값이 바뀌어도 리스트의 값은 바뀌지 않는다.
    - 인자로 받은 배열 원소를 일일이 순회해 새로 만든 배열 변수에 대입하고 넘겨준다.
- 반면, `Arrays.asList`는 참조한 원본 배열의 값이 바뀌면 리스트의 값도 바뀌고, 반대로 List의 값을 수정해도 원본 배열의 값도 바뀌게 된다.
    - `Arrays.asList`는 인자로 받은 **배열 레퍼런스 변수를 그대로 참조(얕은 복사)하고 있기 때문이다.**

### Null 값 여부

- `Arrays.asList`는 원소에 null을 가질 수 있지만, `List.of`는 가질 수 없다.
- `List.of`는 null여부를 `contains()`로 확인할 수도 없다.

## 참고 자료

- https://m.blog.naver.com/roropoly1/221140156345
- [https://inpa.tistory.com/entry/JAVA-☕-ArraysasList-와-Listof-차이-한방-정리](https://inpa.tistory.com/entry/JAVA-%E2%98%95-ArraysasList-%EC%99%80-Listof-%EC%B0%A8%EC%9D%B4-%ED%95%9C%EB%B0%A9-%EC%A0%95%EB%A6%AC)
- https://bepoz-study-diary.tistory.com/349