```java
public class MyClass {
    
    /**
     * JDK11 기준
        1 2 3 4 5 6 7 8 9 0 
        11코드 실행 시간:                   29ms
        1 2 3 4 5 6 7 8 9 0 
        22코드 실행 시간:                    1ms
    */
    
    static int[] arr = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 0 };
    
    public static void main(String args[]) {
        long startTime1 = System.currentTimeMillis();
        
        // size를 직접 선언한 경우
        int arrSize = arr.length;
        
        for (int i = 0; i < arrSize; i++) {
            System.out.print(arr[i] + " ");
        }
        
        System.out.println();
        long endTime1 = System.currentTimeMillis();

        System.out.println(String.format("11코드 실행 시간: %20dms", endTime1 - startTime1));
        
        
        long startTime2 = System.currentTimeMillis();
        
        // size를 매 반복문마다 참고하는 경우
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
        
        
        System.out.println();
        long endTime2 = System.currentTimeMillis();

        System.out.println(String.format("22코드 실행 시간: %20dms", endTime2 - startTime2));
    }
}
```

- 자바스크립트 성능 블로그 글에서 배열 길이를 직접 참조하면 성능이 좋지 않다고 한다.
    - https://joshua1988.github.io/web-development/javascript/javascript-best-practices/
- 그래서 직접 실험해봤는데 진짜였다…