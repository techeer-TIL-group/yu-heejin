## 페이지 교체 알고리즘

> 페이징 기법으로 메모리를 관리하는 운영체제에서, 페이지 부재가 발생하여 새로운 페이지를 할당하기 위해 현재 할당된 페이지 중 어떤 것과 교체할 지 결정하는 방법
> 

### FIFO

- 페이지가 **주기억장치에 적재된 시간을 기준으로 교체될 페이지를 선정**하는 기법
- 중요한 페이지가 오래 있었다는 이유만으로 교체될 수 있다.

### LFU

- **가장 적은 횟수로 참조되는 페이지를 교체**하는 기법
- 참조될 가능성이 많음에도 불구하고 횟수에 의한 방법이므로 최근에 사용된 프로그램을 교체시킬 가능성이 있다.
- 오버 헤드가 발생할 우려가 있다.

### LRU

- **가장 오랫동안 참조되지 않은 페이지를 교체**하는 기법
- 프로세스가 주기억장치에 접근할 때마다 참조된 페이지에 대한 시간을 기록해야한다.
- 오버헤드가 발생할 우려가 있다.

## LRU (Least Recently Used) 알고리즘 개념

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F998933375C7F78A428)

- LRU 알고리즘은 가장 오랫동안 참조되지 않은 페이지를 교체하는 기법이다.

### cache hit, cache miss

- cache miss: 해야 할 작업이 캐시에 없는 경우
- cache hit: 해야 할 작업이 캐시에 있는 경우

### 장점

- 빠른 액세스 - 가장 최근에 사용한 아이템부터 가장 이전에 사용한 아이템까지 정렬되므로 아이템에 접근할 경우 `O(n)`의 시간 복잡도를 가진다.
- 빠른 업데이트 - 하나의 아이템에 엑세스할 때마다 업데이트되며, `O(n)`의 시간 복잡도를 가진다.

### 단점

- `n`개의 아이템을 저장하는 LRU는 `n`개의 크기를 가지는 한 개의 리스트와 이를 추적하기 위해 `n`개의 크기를 가지는 한 개의 맵이 필요하다.
- `O(n)`의 복잡도를 가지지만 2개의 데이터 구조를 사용해야 한다는 단점이 있다.

## 코드

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/17680)

- 코딩 테스트 문제라 지저분하게 구현했지만,, 구글링을 하지 않고 풀었다는 것에 의의를 둡니다,,,

```java
import java.util.*;

// LRU 알고리즘, 맨 뒤에 있는 값이 가장 최근에 참조되었다고 가정한다.

class Solution {
    
    private final int CACHE_HIT = 1;
    private final int CACHE_MISS = 5;
    
    public int solution(int cacheSize, String[] cities) {
        if (cacheSize == 0) {
            return cities.length * CACHE_MISS;
        }
        
        List<String> cache = new ArrayList<>();
        int time = 0;
        
        for (String city : cities) {
            city = city.toLowerCase();
            int index = cache.indexOf(city);
            
            if (cache.size() == 0) {
                cache.add(city);
                time += CACHE_MISS;
            } else if (index != -1) {
                // 해당 캐시 안에 값이 존재하는 경우
                if (cacheSize > 1) {
                    for (int i = index; i < cache.size() - 1; i++) {
                        cache.set(i, cache.get(i + 1));
                    }
                    
                    cache.set(cache.size() - 1, city);
                }
                
                time += CACHE_HIT;
            } else {
                // 해당 캐시 안에 값이 존재하지 않는 경우
                if (cacheSize == 1) {
                    cache.set(0, city);
                } else if (cache.size() < cacheSize) {
                    cache.add(cache.size(), city);
                } else {
                    for (int i = 0; i < cache.size() - 1; i++) {
                        cache.set(i, cache.get(i + 1));
                    }
                    
                    cache.set(cache.size() - 1, city);
                }
                
                time += CACHE_MISS;
            }
        }
        
        return time;
    }
}
```

## 참고 자료

- [https://nack1400.tistory.com/entry/AlgorithmJava-알고리즘-자바-LRU-코딩테스트-Sorting-Searching-정렬-검색-Least-Recently-Used](https://nack1400.tistory.com/entry/AlgorithmJava-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%9E%90%EB%B0%94-LRU-%EC%BD%94%EB%94%A9%ED%85%8C%EC%8A%A4%ED%8A%B8-Sorting-Searching-%EC%A0%95%EB%A0%AC-%EA%B2%80%EC%83%89-Least-Recently-Used)
- [https://velog.io/@ddyy094/LRULeast-Recently-Used-알고리즘-이란](https://velog.io/@ddyy094/LRULeast-Recently-Used-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%9D%B4%EB%9E%80)
- https://j2wooooo.tistory.com/121