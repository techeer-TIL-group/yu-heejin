## 주소 값만 복사

```java
List<Number> copy1 = original;
```

- 복사라기 보다는 **동일한 주소값을 바라본다.**

## 생성자를 통한 복사

```java
List<Number> copy2 = new ArrayList<>(original);
```

- 생성자에 원본 배열을 넣어, **원본 배열 original이 가지고 있는 요소를 똑같이 가지고 있는 copy2를 만들 수 있다.**

## addAll()을 통한 복사

```java
List<Number> copy3 = new ArrayList<>();
copy3.addAll(original);
```

- 생성자를 통한 복사와 비슷하다.
- 비어있는 컬렉션을 만들고, 그 컬렉션에 original의 모든 요소를 추가한다.

## stream을 통한 복사

```java
List<Number> copy4 = original.stream()
        .collect(Collectors.toList());
```

- 생성자를 통한 복사와 비슷하다.

## unmodifiable을 통한 복사

```java
List<Number> copy5 = Collections.unmodifiableList(original);
```

- `unmodifiable`은 매개변수로 제공된 컬렉션의 읽기 전용인 Read-Only 컬렉션을 반환한다.
- `unmodifiableList`뿐만 아니라 `unmodifiableSet`, `unmodifiableMap`도 사용 가능하다.

## copyOf를 통한 복사

```java
List<Number> copy6 = List.copyOf(original);
```

- `unmodifiable`과 똑같이 읽기 전용 컬렉션을 반환한다.

## 참고 자료

- https://hudi.blog/ways-to-copy-collection/