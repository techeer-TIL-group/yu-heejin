## Priority Queue(우선순위 큐)란?

- `Queue`는 FIFO 구조로 먼저 들어온 순서대로 데이터가 나가게 되지만, `Priority Queue`는 데이터의 우선순위를 정해 우선순위가 높은 순서대로 데이터가 나간다.
- 우선순위 큐는 우선순위 힙으로 구현할 수 있다.

## 특징

- 값을 비교해야하 하기 때문에 `null`을 허용하지 않는다.
- 비교할 수 없는 객체는 큐를 만들 수 없다.
- 내부 구조는 이진트리 힙으로 구성되어 있다.
- 내부 구조가 이진트리로 되어 있어서 `add()`, `poll()` 메서드의 시간복잡도는 `O(log n)`이다.

## Priority Queue 선언하기

```java
// Integer 타입 - 오름차순으로 우선순위 결정
PriorityQueue<Integer> pq1 = new PriorityQueue<>();

// Integer 타입 - 내림차순으로 우선순위 결정
PriorityQueue<Integer> pq2 = new PriorityQueue<>(Collections.reverseOrder());
```

## 값 추가하기

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();

pq.add(1);
pq.add(3);
pq.offer(2);
```

- `offer()`, `add()` 메서드로 값을 추가할 수 있다.

## 값 삭제하기

- `poll()`, `remove()` 메서드를 사용하면 우선순위가 가장 높은 값을 제거한다.
    - 삭제된 값을 반환한다.
- `remove(index)` 메서드를 사용하면 index 순위에 해당하는 값을 제거한다.
- `clear()` 메서드를 사용하면 우선순위 큐의 모든 값을 삭제한다.

## 참고 자료

- https://crazykim2.tistory.com/575