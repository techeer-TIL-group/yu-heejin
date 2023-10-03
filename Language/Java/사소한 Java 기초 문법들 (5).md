## replace()/replaceAll()

### replace()

```java
String replaceTest = "우리의 리플레이스 테스트";
System.out.println( replaceTest.replace("리플레이스","replace") );
//  우리의 replace 테스트
```

- 대상 문자열을 원하는 문자 값으로 변환하는 함수
- 첫번째 매개변수는 변환하고자 하는 대상이 될 문자열, 두번째 매개변수는 변환할 문자 값을 의미한다.

### replaceAll()

```java
String replaceAllTest = "우리의 리플레이스의 리플레이스테스트";
System.out.println( replaceAllTest.replaceAll("리플레이스","replaceAll") );
//  우리의 replaceAll의 replaceAll테스트
```

- 대상 문자열을 원하는 문자 값으로 변환하는 함수이다.
- 첫번째 매개변수는 변환하고자 하는 대상이 될 문자열, 두번째 매개변수는 변환할 문자 값을 의미한다.

### replace vs replaceAll

- 인자 값의 형태가 CharSequence와 String이라는 차이점이 있다.
    - **String이 인자 타입인 경우 정규표현식 형태의 인자값**을 사용할 수 있다.
    - replaceAll()의 경우 정규 표현식도 가능하다.
- 예제
    
    ```java
    String str = "abcdefghijk";
    
    // 1. replaceAll
    // 굿굿굿defg굿굿굿k (abchij 값 모두를 변경)
    System.out.println(str.replaceAll("[abchij]", "굿"));
    // abc굿굿굿굿hij굿 (abchij 값을 제외하고 변경)
    System.out.println(str.replaceAll("[^abchij]", "굿"));
    
    // 2. replace
    // 굿굿굿defg굿굿굿k (abchij 값 모두를 변경)
    System.out.println(str.replace("a", "굿")
    									.replace("b", "굿")
    									.replace("c", "굿")
    									.replace("h", "굿")
    									.replace("i", "굿")
    									.replace("j", "굿")
    )
    ```
    

### replaceFirst()

```java
String replaceFirstTest = "우리의 리플레이스의 리플레이스테스트";
System.out.println( replaceFirstTest.replaceFirst("리플레이스","replaceFirst") );
//  우리의 replaceFirst의 리플레이스테스트
```

- 위 메소드들과 사용법은 같으나, 처음 나오는 단어를 찾아서 바꿔주는 함수이다.

## Map 전체 출력하기

### entrySet()

```java
Map<String, String> map = new HashMap<>();
map.put("key01", "value01");
map.put("key02", "value02");
map.put("key03", "value03");
map.put("key04", "value04");
map.put("key05", "value05");

// 방법 01 : entrySet()
for (Map.Entry<String, String> entry : map.entrySet()) {
	System.out.println("[key]:" + entry.getKey() + ", [value]:" + entry.getValue());
}
```

- Map의 key와 value의 값이 모두 필요한 경우 사용한다.

### keySet()

```java
// 방법 02 : keySet()
for (String key : map.keySet()) {
	String value = map.get(key);
	System.out.println("[key]:" + key + ", [value]:" + value);
}
```

- key의 값만 필요한 경우 사용한다.

### entrySet().iterator(), keySet().iterator()

```java
// 방법 03 : entrySet().iterator()
Iterator<Map.Entry<String, String>> iteratorE = map.entrySet().iterator();
while (iteratorE.hasNext()) {
	Map.Entry<String, String> entry = (Map.Entry<String, String>) iteratorE.next();
   	String key = entry.getKey();
   	String value = entry.getValue();
   	System.out.println("[key]:" + key + ", [value]:" + value);
}

// 방법 04 : keySet().iterator()
Iterator<String> iteratorK = map.keySet().iterator();
while (iteratorK.hasNext()) {
	String key = iteratorK.next();
	String value = map.get(key);
	System.out.println("[key]:" + key + ", [value]:" + value);
}
```

- **Map은 iterator 인터페이스를 직접적으로 사용할 수 없기 때문**에 Map에 `entrySet()`, `keySet()` 메소드를 활용하여 Set객체를 반환받은 후 iterator 인터페이스를 사용할 수 있다.

### Lambda 사용

```java
// 방법 05 : Lambda 사용
map.forEach((key, value) -> {
	System.out.println("[key]:" + key + ", [value]:" + value);
});
```

### Stream 사용

```java
// 방법 06 : Stream 사용
map.entrySet().stream().forEach(entry-> {
	System.out.println("[key]:" + entry.getKey() + ", [value]:"+entry.getValue());
});
	        
// Stream 사용 - 내림차순
map.entrySet().stream().sorted(Map.Entry.comparingByKey()).forEach(entry-> {
	System.out.println("[key]:" + entry.getKey() + ", [value]:"+entry.getValue());
});

// Stream 사용 - 오름차순
map.entrySet().stream().sorted(Map.Entry.comparingByKey(Comparator.reverseOrder())).forEach(entry-> {
	System.out.println("[key]:" + entry.getKey() + ", [value]:"+entry.getValue());
});
```

## Set ↔ List

### List → Set

```java
List<Integer> list = Arrays.asList(0, 1, 2, 3, 4, 5);
Set<Integer> set = new HashSet<>(list);
```

- Set 생성시 인자로 List를 전달하면 된다.

### Set → List

```java
Set<Integer> set = new HashSet<>(Arrays.asList(0, 1, 2, 3, 4, 5));
List<Integer> list = new ArrayList<>(set);
```

- List 생성 시 인자로 Set을 전달하면 된다.

## Set.equals()

```java
// Creating object of Set
Set<String> arrset1 = new HashSet<String>();

// Populating arrset1
arrset1.add("A");
arrset1.add("B");
arrset1.add("C");
arrset1.add("D");
arrset1.add("E");

// print arrset1
System.out.println("First Set: " + arrset1);

// Creating another object of Set
Set<String> arrset2 = new HashSet<String>();

// Populating arrset2
arrset2.add("A");
arrset2.add("B");
arrset2.add("C");
arrset2.add("D");
arrset2.add("E");

// print arrset2
System.out.println("Second Set: " + arrset2);

// comparing first Set to another
// using equals() method
boolean value = arrset1.equals(arrset2);

// print the value
System.out.println("Are both set equal? " + value);

First Set: [A, B, C, D, E]
Second Set: [A, B, C, D, E]
Are both set equal? true
```

- 두 set이 같은지 확인

## n진수 ↔ 10진수

### 10진수 → n진수

```java
Integer.toString(int number, int n)
```

- 변환할 숫자와 바꾸고 싶은 진법 수를 넣으면 **해당 진수 문자열로 변환**된다.

### n진수 → 10진수

```java
Integer.parseInt(String number, int n)
```

- 변환할 진수 문자열과 해당 진수의 진법 수를 넣으면 10진수로 변환된다.

## 2진수 앞에 0 출력하기

```java
System.out.println(Integer.toBinaryString(14));
```

- 위 코드의 출력 결과는 1110이다
- 그러나 01110으로 출력하고 싶을 수 있다. (자리수 맞추기)

### String.format() 활용하기

```java
String b = String.format("**%05d**", Integer.parseInt(Integer.toBinaryString(14)));
System.out.print(b); // 01110
```

- %5d → 5자리 숫자로 표현
- %05d → 공백 대신 0으로 표현

```java
String c = String.format("%0"+n+"d", Integer.parseInt(Integer.toBinaryString(num)));
```

- 위처럼 표현하면 동적으로 포맷을 설정할 수 있다.

## 문자열 특정 문자 포함 여부 확인

### contains()

```java
boolean contains(CharSequence s)
```

- 파라미터로 입력받은 `CharSequence s`가 문자열에 포함되어 있는지 여부를 반환한다.

### indexOf()

```java
int indexOf(int ch)
int indexOf(int ch, int startIndex)
int indexOf(String str)
int indexOf(String str, int startIndex)
```

- 파라미터로 입력받은 문자나 문자열이 **원본 문자열에서 처음 나타나는 index를 찾아 index 위치를 반환**한다.
- 원본 문자열에 찾는 문자나 문자열이 없으면 -1을 리턴한다.

### matches()

```java
boolean matches(String regex)

public class StringContainsString {
    public static void main(String[] args) {
 
        String str = "hello Java!";
 
        System.out.println(str.matches("he")); // false
        System.out.println(str.matches("(.*)he(.*)")); // true
    }
}
```

- 파라미터로 정규식을 입력받는다.
- `“he”`의 경우 해당 문자열이 정확히 `“he”`여야만 true이다.

## split, substring

### split()으로 문자열 자르기

```java
public String[] split(String regex)
public String[] split(String regex, int limit)

String str = "Hi guys This is split example";

String[] result = str.split(" ");
String[] result2 = str.split(" ", 2);
String[] result3 = str.split(" ", 3);

System.out.println(Arrays.toString(result));
System.out.println(Arrays.toString(result2));
System.out.println(Arrays.toString(result3));

// 출력 결과
[Hi, guys, This, is, split, example]
[Hi, guys This is split example]
[Hi, guys, This is split example]
```

- regex(정규 표현식)를 기준으로 그 패턴과 일치하는 문자열을 기준으로 문자열을 자른다.
- limit은 문자열을 나눌 최대 개수이다.
    - 리턴되는 배열의 길이는 `limit - 1`이다.

### substring()으로 문자열 자르기

```java
public String substring(int beginIndex)
public String substring(int beginIndex, int endIndex)
```

- `endIndex - 1`까지 문자열을 자른다.
- `endIndex`를 주지 않으면 시작 인덱스부터 끝까지 자른다.

## Character.digit()

```java
public static int digit(char ch, int radix)

Character.isDigit('a');   // false
Character.isDigit('1');   // true
```

- char 값이 숫자인지 여부를 판단하여 `true`, `false` 값으로 리턴한다.

## Char 배열을 문자열로 변환

### String 생성자

```java
public void charArrayToString1() {
    char[] charArray = { 'H', 'e', 'l', 'l', 'o', 'W', 'o', 'r', 'l', 'd' };
    String str = new String(charArray);
    System.out.println(str);   // HelloWorld
}
```

### String.valueOf()

```java
public void charArrayToString2() {
    char[] charArray = { 'H', 'e', 'l', 'l', 'o', 'W', 'o', 'r', 'l', 'd' };
    String str = String.valueOf(charArray);
    System.out.println(str);   // HelloWorld
}
```

## 문자열 거꾸로 뒤집기

### 반복문 사용하기

```java
String str = "ABCDE";

String reverse = "";
for (int i = str.length() - 1; i >= 0; i--) {
	reverse += str.charAt(i);
}
```

### StringBuffer.reverse()

```java
String str = "ABCDE";

StringBuffer sb = new StringBuffer(str);
String reverse = sb.reverse().toString();
```

## 리스트, 배열 null 체크하기

### 리스트 null 체크 - isEmpty()

### 배열 null 체크 - length

- 배열의 길이가 0인 경우 null로 체크한다.

## List → Array

### List.toArray()

```java
// set List (String) - reference types
List<String> list = new ArrayList<>();
list.add("a");
list.add("b");
list.add("c");

// 1. toArray() - 배열 선언과 동시에 할당
String[] arr = list.toArray(new String[0]); // ["a", "b", "c"]
//String[] arr = list.toArray(String[]::new);   // java11~ 이상

// 2. toArray() - 배열 선언 후 채워 넣음
String[] arr2 = new String[list.size()];
list.toArray(arr2); // ["a", "b", "c"]
```

### Stream API 사용

```java
// 3. Stream API (Java8 이상)
String[] arr3 = list.stream().toArray(String[]::new);

// 5. int 기본형(primitive) 변환 - Stream API (Java8 이상)
int[] arr5 = list.stream().mapToInt(i->i).toArray();
```

## Array → List

### Arrays.asList() → fail

```java
import java.util.Arrays;
import java.util.List;
 
public class IntArrayConvertToList {
    public static void main(String[] args) {
        
        // int 배열
        int[] arr = { 1, 2, 3 };
 
        // Arrays.asList() 
        List<int[]> intList = Arrays.asList(arr);
 
        // 결과 출력
        System.out.println(intList.size()); // 1
        System.out.println(intList.get(0)); // I@71bb301
        System.out.println(Arrays.toString(intList.get(0)));  // [1, 2, 3]
 
    }
}
```

- `Arrays.asList()`에 정적 배열을 넣으면 list로 변환되는 것이 아니라 `List<int[]>`가 된다.
- **primitive 타입을 wrapper 클래스로 형변환해주지 않는다.**
- `**List<Integer>`로 변환시키고 싶다면 `int[]`를 `Integer[]`로 변환해야한다.**

### Stream API 사용

```java
// int -> List
List<Integer> intList 
        = Arrays.stream(arr)
                .boxed().  // primitive -> Wrapper
                .collect(Collectors.toList());
```

## 참고 자료

- https://mine-it-record.tistory.com/127
- https://www.delftstack.com/ko/howto/java/java-replace-multiple-characters-in-string/
- https://jamesdreaming.tistory.com/85
- https://tychejin.tistory.com/31
- https://codechacha.com/ko/java-convert-set-to-list-and-list-to-set/
- https://www.geeksforgeeks.org/set-equals-method-in-java-with-examples/
- https://cornarong.tistory.com/48
- https://nsmchan.tistory.com/15
- https://hianna.tistory.com/539
- https://d8040.tistory.com/93
- https://jamesdreaming.tistory.com/157
- [https://codechacha.com/ko/java-convert-chararray-to-string/#4-stream--char를-문자열로-변환](https://codechacha.com/ko/java-convert-chararray-to-string/#4-stream--char%EB%A5%BC-%EB%AC%B8%EC%9E%90%EC%97%B4%EB%A1%9C-%EB%B3%80%ED%99%98)
- https://hianna.tistory.com/543
- https://dlagusgh1.tistory.com/437
- https://ifuwanna.tistory.com/355
- https://hianna.tistory.com/552
- [https://www.blog.ecsimsw.com/entry/Java-배열을-리스트-변환](https://www.blog.ecsimsw.com/entry/Java-%EB%B0%B0%EC%97%B4%EC%9D%84-%EB%A6%AC%EC%8A%A4%ED%8A%B8-%EB%B3%80%ED%99%98)