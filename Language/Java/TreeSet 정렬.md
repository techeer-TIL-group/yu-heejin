```java
Comparator<String> comp = new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
        if (o1.length() < o2.length()) {
            // 문자열 길이가 더 작은 쪽이 앞으로 온다.
            return -1;
        } else if (o1.length() == o2.length()) {
            // 길이가 같으면 사전순 비교
            // 이 때 두 문자열이 같은 경우 (0인 경우) 중복을 제거한다.
            return o1.compareTo(o2);
        } else {
            // 그게 아니면 뒤로 이동
            return 1;
        }
    }
};

int n = Integer.parseInt(br.readLine());
Set<String> input = new TreeSet<>(comp);
```

- TreeSet 안에 있는 원소들을 compare하면 해당 정렬 순서를 유지하면서 원소들을 저장한다.
- 이 때 `return 0;` 인 경우, **두 값이 같은 값이라고 보고 중복된 값이라 판단하고 값을 하나만 넣는다.**
    
    ```java
    입력 값:
    hello
    hello
    bye
    
    결과:
    bye
    hello
    ```
    

## 참고 자료

- https://cornswrold.tistory.com/203
- https://tworab.tistory.com/16