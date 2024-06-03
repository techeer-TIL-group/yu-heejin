## ArrayList

| method | 시간 복잡도 |
| --- | --- |
| add | O(1) |
| remove | O(n) |
| get | O(1) |
| contains | O(n) |
| iterator.remove | O(n) |

### 특징

- 데이터 추가, 삭제를 위해 임시 배열을 생성해 데이터를 복사한다.
- 대량의 자료를 추가/삭제 시 복사가 일어나게 되어 성능 저하가 발생한다.
- 데이터의 인덱스를 가지고 있어 데이터 검색 시 빠르다.
- Java 1.2에 추가되며, thread-safe를 보장하지 않는다.

## LinkedList

| method | 시간 복잡도 |
| --- | --- |
| add | O(1) |
| remove | O(1) |
| get | O(n) |
| contains | O(n) |
| iterator.remove | O(1) |
- LinkedList의 삭제 연산은 삭제할 노드에 대한 참조를 가지고 있다는 가정하에 O(1)이다.
    - 만약 처음부터 지울 데이터를 찾는 경우 `O(n)`의 시간이 소요된다.

### 특징

- Java 1.2에 추가되며, thread-safe를 보장하지 않는다.
- 데이터를 저장하는 각 노드가 이전 노드와 다음 노드의 상태만 알고 있다.
- 데이터 추가/삭제 시 빠르다.
- 데이터 검색 시 처음부터 노드를 순화해야 되기 때문에 느리다.

## CopyOnWriteArrayList

| method | 시간 복잡도 |
| --- | --- |
| add | O(n) |
| remove | O(n) |
| get | O(1) |
| contains | O(n) |
| iterator.remove | O(n) |

### 특징

- Java 1.5에 추가, thread-safe를 보장한다.
- 처리에 여분의 오버로드를 가져오지만, 순회 작업의 수에 비해 수정 횟수가 최소일 때 효과적이다.
- add는 ArrayList, LinkedList보다 느리지만, get은 LinkedList보단 빠르고 ArrayList보단 살짝 느리다.

## HashSet

| method | 시간 복잡도 |
| --- | --- |
| add | O(1) |
| contains | O(1) |
| next | O(h / n) → h는 테이블 용량 |
- h는 해시 버킷의 사이즈, n은 HashSet에 저장되는 데이터의 사이즈를 의미한다.
    - `h/n` 이라는 값이 나오는 이유는 엘리먼트에 비해 해시 버킷의 수가 늘어나면, 해시 버킷으로 사용되는 배열의 대부분은 비어있게 되고, 엘리먼트가 담긴 해시 버킷을 찾기 위해 매번 비어있는 해시버킷을 방문해야 하기 때문이다.
    - 또한, 엘리먼트의 숫자가 늘어나면 해시 버킷이 비어있을 가능성이 줄어들고, `O(1)`에 근접한다.

### 특징

- Java 1.2, thread-safe를 보장하지 않는다.
- 객체들을 순서없이 저장하고, 동일한 객체를 중복 저장하지 않는다.
- 중복되지 않는 값을 등록할 때 용이하다.
- 순서없이 저장되는 것에 주의해야한다.
- null을 허용한다.

## LinkedHashSet

| method | 시간 복잡도 |
| --- | --- |
| add | O(1) |
| contains | O(1) |
| next | O(1) |

### 특징

- Java 1.4, thread-safe를 보장하지 않는다.
- 속도는 hashSet에 비해 느리지만, 좋은 성능을 보장한다.
- 등록한 순으로 정렬한다.
- null을 허용한다.

## CopyOnWriteArraySet

| method | 시간 복잡도 |
| --- | --- |
| add | O(1) |
| contains | O(1) |
| next | O(1) |

### 특징

- Java 1.5
- 적은 메모리를 사용한다.
- 빠르다.
- null을 사용할 수 없다.

## TreeSet

| method | 시간 복잡도 |
| --- | --- |
| add | O(log n) |
| contains | O(log n) |
| next | O(log n) |

### 특징

- Java 1.2, thread-safe를 보장하지 않는다.
- 객체 기준으로 정렬하기 때문에 느리다.
- null을 허용하지 않는다.

## ConcurrentSkipListSet

| method | 시간 복잡도 |
| --- | --- |
| add | O(log n) |
| contains | O(log n) |
| next | O(1) |

### 특징

- Java 1.6 병렬처리, thread-safe 보장, 병렬 보장
- null을 허용하지 않는다.
- TreeSet처럼 정렬한다.

## HashMap

| method | 시간 복잡도 |
| --- | --- |
| get | O(1) |
| containsKey | O(1) |
| next | O(h / n) → h는 테이블 용량 |

### 특징

- Java 1.2
- 순서에 상관없이 저장된다.
- null을 허용한다.
- thread-safe를 보장하지 않는다.

## LinkedHashMap

| method | 시간 복잡도 |
| --- | --- |
| get | O(1) |
| containsKey | O(1) |
| next | O(1) |

### 특징

- Java 1.4
- 순서대로 등록한다.
- null을 허용한다.
- thread-safe를 보장하지 않는다.

## TreeMap

| method | 시간 복잡도 |
| --- | --- |
| get | O(log n) |
| containsKey | O(log n) |
| next | O(log n) |

### 특징

- Java 1.2
- 정렬되면서 추가된다.
- null은 허용하지 않는다.
- thread-safe를 보장하지 않는다.

## ConcurrentHashMap

| method | 시간 복잡도 |
| --- | --- |
| get | O(1) |
| containsKey | O(1) |
| next | O(h / n) → h는 테이블 용량 |

### 특징

- Java 1.5
- thread-safe를 보장하면서 SynchronizedMap보다 속도가 빠르다.
- null을 허용하지 않는다.

## ConcurrentSkipListMap

| method | 시간 복잡도 |
| --- | --- |
| get | O(log n) |
| containsKey | O(log n) |
| next | O(1) |

### 특징

- Java 1.6
- thread-safe를 보장하면서 SynchronizedMap보다 속도가 빠르다.
- 메모리를 사용하여 O(log n)으로 데이터를 검색, 삽입, 삭제가 가능하다.
- lock이 적게 사용되어야하는 병렬 처리 시스템에 용이하다.

## PriorityQueue

| method | 시간 복잡도 |
| --- | --- |
| offer | O(log n) |
| peek | O(1) |
| poll | O(log n) |
| size | O(1) |
| natural order | JVM에서 제공하는 일반적인 순서와 다를 수 있다. |

### 특징

- Java 1.5
- 일반적인 큐는 FIFO의 구조를 가지지만, natural order에 따라 정렬된다.
- null을 허용하지 않는다.

## ConcurrentLinkedQueue

| method | 시간 복잡도 |
| --- | --- |
| offer | O(1) |
| peek | O(1) |
| poll | O(1) |
| size | O(n) |

### 특징

- Java 1.5, thread-safe 보장(결과에 문제가 발생할 여지가 있다.)
- FIFO 방식의 Queue
- 데이터의 추가/삭제가 빠르다.
- size는 O(1)이 아니다.
- null을 허용하지 않는다.

## ArrayBlockingQueue

| method | 시간 복잡도 |
| --- | --- |
| offer | O(1) |
| peek | O(1) |
| poll | O(1) |
| size | O(1) |

### 특징

- Java 1.5
- 고정 배열에 일반적인 Queue
- 배열이 고정된 사이즈, 생성되면 변경되지 않는다.

## 참고 자료

- https://www.grepiu.com/post/9
- https://hbase.tistory.com/185