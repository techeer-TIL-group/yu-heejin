```java
public class Main {
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        BinarySearch bs = new BinarySearch();
        while (sc.hasNext()) {
            int number = sc.nextInt();
            bs.add(number);
        }
        
        bs.postOrder(bs.root);
    }
}
```

- Scanner를 사용하여 값을 읽을 때 값이 몇개 들어오는지 알 수 없는 경우, `Scanner.hasNext()`를 사용한다.

## 참고 자료

- https://frhyme.github.io/java/java_basic03_scan_unknownLength/