## String → ArrayList

1. for문을 사용하지 않고 addAll을 사용하는 방법
    
    ```java
    String[] strArr = { "hello", "world" };
    ArrayList<String> strList = new ArrayList<>();
    
    strList.addAll(Arrays.asList(strArr));
    ```
    
2. `Arrays.asList`를 사용하여 바로 대입하는 방법
    
    ```java
    String[] strArr = { "hello", "world" };
    ArrayList<String> strList = new ArrayList<>(Arrays.asList(strArr));
    ```
    

## List Null 체크하기

- `List.isEmpty()`를 사용한다.
    
    ```java
    if (list == null || list.isEmpty()){
    	System.out.println("list is empty");
    }
    ```
    
    - 단, `isEmpty()` 함수 사용 시 list 객체가 null인 경우 `NullPointException`이 발생하기 때문에 조건문에 `null`체크도 같이 해주어야 한다.
- `List.size()`는 요소의 개수를 반환하는데, `null`인 경우 0이 리턴된다.
    
    ```java
    if (list == null || list.size() == 0 ){
    	System.out.println("list is empty");
    }
    ```
    

### [추가] 정적 배열 Null 체크하기

- `length()` 함수는 요소 개수를 리턴하는데, `null`인 경우 0을 반환한다.
    
    ```java
    String[] arr = new String[5];
    
    if (arr.length == 0) {
        System.out.println("The arr is Empty");
    }
    ```
    

## String[] → String

1. `toString()` 메서드
    
    ```java
    String[] strArray = {"Hello", " ", "Java", " ", "Programming"};
    String strArrayToString  = Arrays.toString(strArray);
    
    System.out.println(strArrayToString);
    // result: [Hello,  , Java,  , Programming]
    ```
    
2. `join()` 메서드
    
    ```java
    String[] strArray = {"Hello", " ", "Java", " ", "Programming"};
    String strArrayToString = String.join("_", strArray);
    
    System.out.println(strArrayToString);
    // result: Hello_ _Java_ _Programming
    ```
    

## 배열 오름차순/내림차순 정렬

### 오름차순

`int[]`, `String[]` 모두 `Arrays.sort()`를 사용하면 쉽게 오름차순으로 정렬할 수 있다.

### 내림차순

- `String[]`
    
    ```java
    Arrays.sort(strArr, Collections.reverseOrder());
    ```
    
- int[]
    1. `Integer[]`로 박싱한 후 `Arrays.sort`로 정렬하는 방법
        
        ```java
        int[] arr = {1, 2, 3, 4, 5};
        Integer[] arr2 = Arrays.stream(arr).boxed().toArray(Integer[]::new);
        Arrays.sort(arr, Collections.reverseOrder());
        ```
        
    2. stream 사용하기
        
        ```java
        Arrays.stream(arr)
        	.boxed()
        	.sorted(Collections.reverseOrder())
        	.mapToInt(Integer::intValue)
        	.toArray();
        ```
        

### 내림차순 시 int형은 불가능한 이유

- 간단하게 **primitive type이라 안되는 것**이다.
- `Arrays.sort`의 내부 코드를 살펴보면, 제네릭 클래스를 받아오는 것을 알 수 있다.

## 뒤에서부터 문자열 위치 찾기 - lastIndexOf()

`indexOf()`가 파라미터로 전달받은 문자열을 원본 문자열의 앞에서부터 찾는다면, `lastIndexOf()`는 파라미터로 전달받은 문자열을 **원본 문자열의 뒤에서부터 탐색하여 처음으로 파라미터의 문자열이 나오는 index를 리턴**한다.

## String to Enum

```java
enum Food {
	PIZZA, CANDY, RAMEN;
}

// String to enum
Food food = Food.valueOf("PIZZA");
```

## Arrays.sort()의 반환 타입

Arrays.sort()는 void 타입을 반환하기 때문에 아래와 같은 코드가 불가능하다.

```java
String text1 = "check";

char c[] = Arrays.sort(text1.toCharArray());
```

아래와 같이 수정하면 된다.

```java
String text1 = "check";
char c[] = text.toCharArray();
Arrays.sort(c);
```

## ArrayList 거꾸로 뒤집기 - Collections.reverse()

```java
// 원본 배열        
ArrayList<String> list = new ArrayList<>(Arrays.asList("H", "e", "l", "l", "o"));        
System.out.println(list);  // [H, e, l, l, o]         

// reverse        
Collections.reverse(list);        
System.out.println(list);  // [o, l, l, e, H]
```

## 참고 자료

- https://gayoung78.tistory.com/59
- https://devjeon.tistory.com/entry/12
- https://developer-talk.tistory.com/451
- https://changheesjunk.tistory.com/15
- https://hianna.tistory.com/660
- https://learn-hyeoni.tistory.com/16
- https://stackoverflow.com/questions/20286913/void-error-in-arrays-sort-in-java
- https://hianna.tistory.com/570