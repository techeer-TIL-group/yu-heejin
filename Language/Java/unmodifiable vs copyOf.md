https://github.com/techeer-TIL-group/yu-heejin/blob/main/Language/Java/collections-copy.md 다음 글과 이어서 진행된다.

## 해시코드 살펴보기

원본과 각 복사본의 해시코드를 비교하여 변수가 같은 객체를 가리키는지 볼 수 있다.

```java
System.out.println("원본: " + System.identityHashCode(original));
System.out.println("주소값만 복사: " + System.identityHashCode(copy1));
System.out.println("생성자 복사: " + System.identityHashCode(copy2));
System.out.println("addAll: " + System.identityHashCode(copy3));
System.out.println("stream: " + System.identityHashCode(copy4));
System.out.println("Collections.unmodifiableList: " + System.identityHashCode(copy5));
System.out.println("List.copyOf: " + System.identityHashCode(copy6));
```

실행 결과는 다음과 같다.

```java
원본: 1334202656
주소값만 복사: 1334202656
생성자 복사: 1885496748
addAll: 482062692
stream: 869947658
Collections.unmodifiableList: 447066754
List.copyOf: 2030693247
```

- **주소값만 복사한 경우 같은 객체를 가리키므로 해시 코드도 동일**하다.
- 나머지는 모두 **원본과 해시 코드가 다르기 때문에 다른 객체를 참조**하고 있다고 볼 수 있다.

## 깊은 복사(Deep Copy)와 얕은 복사(Shallow Copy)

- 깊은 복사란 **복사 시 각 컬렉션이 가지고 있는 모든 요소를 복사하여 원본의 요소와 다른 참조를 가지도록 하는것**
- 반대로 원본의 요소와 복사본의 요소가 같은 참조를 가진다면 얕은 복사이다.

```java
original.get(1).setNumber(100);
// 원본 컬렉션 요소 하나를 임의로 변경
```

- 위 코드처럼 원본 컬렉션의 요소를 임의로 변경한 후 실행 결과는 다음과 같다.
    
    ```java
    주소값만 복사: Number{number=100}
    생성자 복사: Number{number=100}
    addAll: Number{number=100}
    stream: Number{number=100}
    Collections.unmodifiableList: Number{number=100}
    List.copyOf: Number{number=100}
    ```
    
    - 모든 복사본이 원본 객체의 변화에 영향을 받았다.
    - 따라서 기본 제공되는 API를 사용하여 컬렉션을 복사한 경우, **외부로부터 변경에 취약하지 않은 컬렉션을 만들기 위해서는 내부 요소가 불변 객체여야한다.**

## 원본 컬렉션이 바뀐 경우

- 위에서 컬렉션의 요소가 변경되는 것은 얕은 복사된 모든 컬렉션에 영향을 끼치는 것을 확인할 수 있다.
- 복사본 컬렉션과 원본 컬렉션의 참조가 완전히 끊겼다는 것은 확신할 수 있는가?
- 아래와 같이 원본 컬렉션에 요소를 추가하면 어떻게 될까?
    
    ```java
    original.add(new Number(3));
    // 원본에 요소 추가
    // 즉, 원본 컬렉션이 변경됨
    ```
    

### 주소값만 복사

```java
System.out.println("주소값만 복사: " + copy1);
// 원본: [Number{number=1}, Number{number=2}, Number{number=3}]
```

- 원본히 변하면 당현히 변한다.

### 생성자, addAll, stream

```java
System.out.println("생성자 복사: " + copy2);
System.out.println("addAll: " + copy3);
System.out.println("stream: " + copy4);
/*
  생성자 복사: [Number{number=1}, Number{number=2}]
  addAll: [Number{number=1}, Number{number=2}]
  stream: [Number{number=1}, Number{number=2}]
*/
```

- 원본의 요소를 하나씩 꺼내 새로운 컬렉션으로 만드는 방법이므로 **원본 컬렉션의 변화에 영향을 받지 않는다.**
- **요소는 원본과 같은 참조를 갖지만, 복사본 컬렉션 자체는 원본 컬렉션과 관련이 없다**는 것을 알 수 있다.

### unmodifiable

```java
System.out.println("Collections.unmodifiableList: " + copy5);
// Collections.unmodifiableList: [Number{number=1}, Number{number=2}, Number{number=3}]
```

- `unmodifiable`로 복사한 컬렉션은 원본 컬렉션 변화에 영향을 받아 Number(3)이 추가됐다.
- `unmodifiableCollection`은 원본 객체와의 참조를 끊지 않는다.
- 단순히 값을 변경할 수 있는 수단을 제공하지 않아 read-only가 된 것이지, 복사본의 불변함을 보장하진 않는다.
- 즉, **정확하게 말하면 복사가 아닌 Wrapping이다.**

### copyOf

```java
System.out.println("List.copyOf: " + copy6);
// List.copyOf: [Number{number=1}, Number{number=2}]
```

- 변화가 없는 것으로 보아 `unmodifiable`과 다르게 `copyOf`의 복사본은 원본 컬렉션과의 참조가 끊긴다.
- `copyOf`는 얕은 복사를 수행하고 그것을 `unmodifiable`로 만들어 반환한다.
- 즉, 정확히는 `ImmutableCollection`을 반환한다.

## unmodifiable vs copyOf

![Untitled](https://hudi.blog/static/a8e81e2bd8dd12bbeffe2927657e5492/c1f10/unmodifiable.png)

- `unmodifiable`은 별도의 **독립적인 list를 가지고 있는 것이 아닌, 원본 컬렉션을 그대로 가리키고 있는 것을 알 수 있다.**

![Untitled](https://hudi.blog/static/42a923cb4f0f86919afb2da1ba204991/f6e13/copyOf.png)

- `immutableList`는 원본 컬렉션과 독립적으로 존재한다.
- 방어적 복사를 수행하기 위해서는 `copyOf`를 사용해야 완전한 불변함을 보장받을 수 있다.

## 참고 자료

- https://hudi.blog/ways-to-copy-collection/