코딩 테스트를 풀다가 의문점이 생겨서 TreeSet과 Comparable에 대해 정리해보기로 했다.

## 문제 링크

[21939번: 문제 추천 시스템 Version 1](https://www.acmicpc.net/problem/21939)

## 전체 코드

```java
import java.io.*;
import java.util.*;

class Problem implements Comparable<Problem> {
    int number;
    int difficulty;
    
    public Problem(int number, int difficulty) {
        this.number = number;
        this.difficulty = difficulty;
    }
    
    @Override
    public int compareTo(Problem o) {
        return this.difficulty - o.difficulty;
    }
}

public class Main {
    public static void main(String args[]) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        int n = Integer.parseInt(br.readLine());    // 문제의 개수
        
        // 오름차순 기준
        TreeSet<Problem> problems = new TreeSet<>((o1, o2) -> {
            if (o1.difficulty == o2.difficulty) {
                return o1.number - o2.number;
            }
            
            return o1.difficulty - o2.difficulty;
        });
        
        // 문제 번호와 난이도를 관리하기 위한 map
        Map<Integer, Integer> map = new HashMap<>();
        
        String[] input;
        
        for (int i = 0; i < n; i++) {
            input = br.readLine().split(" ");
            
            int number = Integer.parseInt(input[0]);
            int difficulty = Integer.parseInt(input[1]);
            
            problems.add(new Problem(number, difficulty));
            map.put(number, difficulty);
        }
        
        int m = Integer.parseInt(br.readLine());
        
        for (int i = 0; i < m; i++) {
            input = br.readLine().split(" ");
            
            switch (input[0]) {
                case "add":
                    int number = Integer.parseInt(input[1]);
                    int difficulty = Integer.parseInt(input[2]);
            
                    problems.add(new Problem(number, difficulty));
                    map.put(number, difficulty);
                    break;
                case "recommend":
                    int x = Integer.parseInt(input[1]);
                    
                    if (x == 1) {
                        // 가장 어려운 문제 출력
                        Problem dp = problems.last();
                        System.out.println(dp.number);
                    } else {
                        Problem ep = problems.first();
                        System.out.println(ep.number);
                    }
                    
                    break;
                case "solved":
                    // 문제 번호 삭제
                    number = Integer.parseInt(input[1]);
                    
                    Problem sp = new Problem(number, map.get(number));
                    problems.remove(sp);
                    
                    break;
            }
        }
    }
}
```

## 풀이 과정

문제를 풀면서 문제 객체를 난도를 기준으로 정렬하기 위해 `Comparable` 인터페이스를 implements 해 구현했다.

```java
class Problem implements Comparable<Problem> {
    int number;
    int difficulty;
    
    public Problem(int number, int difficulty) {
        this.number = number;
        this.difficulty = difficulty;
    }
    
    @Override
    public int compareTo(Problem o) {
        return this.difficulty - o.difficulty;
    }
}
```

그리고 해당 객체에 대해 정렬된 상태를 유지하고 싶어 TreeSet 자료구조를 사용했다.

```java
// 오름차순 기준
TreeSet<Problem> problems = new TreeSet<>((o1, o2) -> {
    if (o1.difficulty == o2.difficulty) {
        return o1.number - o2.number;
    }
    
    return o1.difficulty - o2.difficulty;
});
```

결과는 정답이었으나 아래 코드에서 의문이 들었다.

```java
case "solved":
    // 문제 번호 삭제
    number = Integer.parseInt(input[1]);
    
    Problem sp = new Problem(number, map.get(number));
    problems.remove(sp);
    
    break;
```

- 해당 객체에 대해 equals를 따로 구현하지 않았는데, **어떻게 같은 값이 들어갔을 때 같은 객체라고 생각하고 삭제했을까?**

## TreeSet 작동 원리

일반적으로 객체는 참조값이기 때문에, 내부 데이터(필드) 값이 같더라도 다른 객체로 인식한다.

- 이러한 문제를 해결하기 위해 일반적으로 equals와 hashCode를 override한다.

하지만 Oracle Java 공식 문서를 살펴보면 다음과 같이 나와있다.

- Note that the ordering maintained by a set (whether or not an explicit comparator is provided) must be *consistent with equals* if it is to correctly implement the `Set` interface. (See `Comparable` or `Comparator` for a precise definition of *consistent with equals*.) **This is so because the `Set` interface is defined in terms of the `equals` operation, but a `TreeSet` instance performs all element comparisons using its `compareTo` (or `compare`) method, so two elements that are deemed equal by this method are, from the standpoint of the set, equal. The behavior of a set *is* well-defined even if its ordering is inconsistent with equals; it just fails to obey the general contract of the `Set` interface.**
- 집합(set)이 Set 인터페이스를 올바르게 구현하기 위해서는 집합이 유지하는 순서(명시적 비교자가 제공되는지 여부에 상관없이)가 equals와 일관성을 유지해야 합니다. (equals와 일관된 정의에 대해서는 Comparable 또는 Comparator를 참조하세요.) 이는 **Set 인터페이스가 equals 연산에 대해 정의되어 있기 때문인데, TreeSet 인스턴스는 모든 요소 비교를 compareTo(또는 compare) 메소드를 사용하여 수행하므로, 이 메소드에 의해 동일하다고 간주되는 두 요소는 집합의 관점에서 동일합니다.** 집합의 순서가 equals와 일치하지 않더라도 집합의 동작은 잘 정의되어 있습니다. 다만, 이는 Set 인터페이스의 일반 계약을 따르지 않는 것에 불과합니다.

즉, TreeSet의 경우 모든 객체 비교를 `compareTo()` 함수를 기준으로 비교하기 때문에 두 원소가 같으면 equals를 implement하지 않아도 동일한 객체로 간주된다.

## 참고 자료

- https://xlos.tistory.com/1481
- https://docs.oracle.com/javase/6/docs/api/java/util/TreeSet.html