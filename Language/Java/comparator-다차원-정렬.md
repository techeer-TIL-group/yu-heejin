## Comparator를 이용한 다차원 배열 정렬

### 내림차순

```java
Arrays.sort(arr, new Comparator<Double[]>() {
    @Override
    public int compare(Double[] o1, Double[] o2) {
        return Double.compare(o2[1], o1[1]);
    }
});
```

- 배열의 **두번째 열을 기준**으로 내림차순 정렬하는 코드이다.
- `Arrays.sort`를 호출하며 매개변수로 배열과 `Comparator` 인터페이스를 구현하여 넘겨준다.
- `Double` 형 배열을 비교하기 위해 `Double.compare` 메서드를 사용하여 비교한다.
    - 첫번째 인자가 크면 양수를 반환한다.
    - 두 인자가 동일하면 0을 반환한다.
    - 두번째 인자가 더 크면 음수를 반환한다.

### 오름차순

```java
Arrays.sort(arr, new Comparator<Double[]>() {
    @Override
    public int compare(Double[] o1, Double[] o2) {
        return Double.compare(o1[1], o2[1]);
    }
});
```

- 오름차순으로 정렬하고자 한다면 위와 같이 순차적으로 매개변수를 전달하면 된다.

## 참고 자료

- https://you88.tistory.com/7