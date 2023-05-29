## Set

- Set은 집합을 구현하기 위한 자료구조이다.
- 집합은 중복 값을 허용하지 않는 자료구조이다.
- 집합은 중복 데이터가 없어야할 때 유용하다.
- 집합이기 때문에 **동일한 값을 중복으로 포함할 수 없고, 요소 순서에 의미가 없다.**
    - 따라서 인덱스로 요소에 접근할 수 없다.
- has() 메서드를 사용하면 boolean으로 값을 확인할 수 있다.
    
    ```tsx
    const set = new Set([1, 2, 3]);
    
    set.has(4) // false
    set.has(2) // true
    ```
    

## Set.prototype.has() vs Array.prototype.includes()

![Untitled](https://velog.velcdn.com/images%2Fsozero%2Fpost%2Ff20bed4e-e4de-4f74-93ca-2f094ce63c6b%2Fimage.png)

- 대충 보면 Arry.includes()는 O(n), Set.has()는 O(1)의 시간복잡도가 있다고 한다.
- Set 자료구조는 해시 테이블 O(1)이나 선형 이하 시간 O(log n)을 사용할 것으로 예상한다.

## 참고 자료

- [https://velog.io/@sozero/TIL-220307-Set.has-와-Array.includes-시간복잡도](https://velog.io/@sozero/TIL-220307-Set.has-%EC%99%80-Array.includes-%EC%8B%9C%EA%B0%84%EB%B3%B5%EC%9E%A1%EB%8F%84)
- [https://www.tech-hour.com/javascript-performance-and-optimization](https://www.tech-hour.com/javascript-performance-and-optimization)