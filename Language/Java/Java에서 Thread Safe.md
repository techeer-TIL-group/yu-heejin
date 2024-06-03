## Thread

- 스레드는 프로세스의 하위 개념으로, 프로세스에 할당된 자원을 공유한다.
- 프로그램을 스레드로 분리하면 자연스럽게 병렬성을 이용할 수 있기 때문에 복잡한 애플리케이션의 성능을 향상시킬 수 있다.

## Thread Safe

- 여러 스레드에서 동시에 접근하더라도 **자료구조의 일관성과 정확성이 유지되는 자료구조**를 의미한다.
- 그러나 대부분의 자료구조는 성능 최적화를 위해 추가적인 동기화 오버헤드를 기본적으로 포함하지 않아 Thread Safe하지 않다.

---

## Confinement (제한)

- 지역 변수는 각 스레드의 자체 스택에 저장되고, 스택은 고유한 영역으로 동시 접근이 발생하지 않는다.
- 전역 변수는 스레드가 공유할 수 있지만, 지역 변수를 사용하면 Thread Safe를 유지할 수 있다.

### Stack 한정 프로그래밍

- Stack에 한정되도록 프로그래밍하는 방법이 있다.
- 스택은 스레드 별 별도의 스택과 지역변수를 갖도록 되어 있기 때문에 동시성을 보장하도록 할 수 있다.

## Immutable (불변)

- 변수에 변경 불가능한 참조를 사용한다.
- Java의 경우에는 `private final` 키워드를 사용할 수 있다.
- 이 때, 데이터를 변경할 수 있는 setter 메소드를 사용하지 않도록 한다.

## Thread-Safe한 자료구조

- `List`, `Set`, `Map`, `ArrayList`, `HashSet`, `HashMap` 등은 thread-safe를 보장하지 않는다.
- Java는 멀티 스레딩 환경을 위해 `Vector`, `HashTable`, `Collections.synchronizedXXX`를 제공한다.

### synchronized

```java
// 1. 자료구조에 사용하는 경우
List<String> list = Collections.synchronizedList(new ArrayList<>());

// 2. 메소드에 사용하는 경우
private synchronized void add(int a) { ... }
```

- 스레드가 `synchronized` 자료구조에 접근하면 lock을 걸고 다른 스레드의 접근을 제한한다.
- 하지만 lock 때문에 병렬적 요소를 처리할 수 없다는 단점이 있으며, 병렬 처리가 안되기 때문에 성능상의 문제가 있다.

### Atomic Object

- `AtomicInteger`, `AtomicLong` 등의 atomic 클래스를 제공한다.
- atomic 클래스는 멀티 스레드 환경에서 원자성을 보장한다.
- `synchronized`와 달리 blocking이 아닌 non-blocking 방식으로 동작하며, 핵심 동작 원리는 CAS 알고리즘이다.

### Concurrent

```java
Map<String, Integer> map = new ConcurrentHashMap<>();
```

- `Concurrent` 자료구조를 사용하면, 동시 작업할 수 있는 스레드의 양을 설정할 수 있다.
- 전체적인 동시성 처리를 제공하는 `synchronized`에 비해 아주 작은 단위에서 동시성 처리를 제공한다.
- read와 write를 구분해 `synchronized`보다 효율적인 처리를 진행한다.
    - 실제로 Java는 `HashTable`의 성능을 더 증진시키기 위해 `ConcurrentHashMap`을 도입해 더 세밀하게 동시성을 보장하도록 했다.
    - 병렬성을 최대한 확보한 자료구조로, 읽기 동작에서는 lock이 걸리지 않지만 쓰기 동작에서는 lock이 걸린다.

## ThreadLocal

- 스레드 영역에 변수를 설정할 수 있어, 특정 스레드가 실행하는 모든 코드에서 해당 스레드에 설정된 변수 값을 사용할 수 있다.
- 대표적인 사용 예로 Spring Security의 `SecurityContextHolder`가 적절한 예이다.

## 병렬 프로그램을 위한 어노테이션

```java
@ThreadSafe
public class Student {
	@GuardedBy("this") private int grade;
	
	public synchronized int increadGrade() {
		return grade++;
	}
}
```

### @Immutable

- 해당 클래스가 불변 클래스임을 나타낸다.
- 자동적으로 `@ThreadSafe` 하다.

### @GuardedBy

- 해당 필드나 메소드를 사용하려면 반드시 지정된 락을 확보한 상태에서 사용해야한다는 것을 의미한다.

## 참고 자료

- https://hojunking.tistory.com/69#google_vignette
- [https://iuboost.tistory.com/entry/Thread-Safe-자료구조](https://iuboost.tistory.com/entry/Thread-Safe-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0)
- https://sup2is.github.io/2021/05/03/thread-safe-in-java.html
- https://aroundck.tistory.com/3423
- https://velog.io/@mangoo/java-thread-safety