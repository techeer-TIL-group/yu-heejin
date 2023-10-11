## 문자열이 특정 문자열로 시작하거나 끝나는지 확인

### startsWith()

```java
import java.util.*;

class Solution {
    
    public boolean solution(String[] phone_book) {
        Arrays.sort(phone_book);
        for (int i = 0; i < phone_book.length - 1; i++) {
            if (phone_book[i + 1].startsWith(phone_book[i])) {
                return false;
            }
        }
        return true;
    }
}
```

- 대상 문자열이 특정 문자열로 시작하는지 체크
- 공백도 하나의 문자열로 취급한다.

### endsWith()

```java
String endsWithT = "자바 코딩 테스트 ";

System.out.println( endsWithT.endsWith("테스트") );  // false
System.out.println( endsWithT.endsWith("테스트 ") );// true
System.out.println( endsWithT.endsWith("트 ") );// true
System.out.println( endsWithT.endsWith(" 테") );// false
```

- 대상 문자열이 특정 문자열로 끝나는지 체크

## Math.ceil 사용 시 주의사항

### Math.ceil의 결과는 실수형이다.

```java
Math.ceil(10.9);   // 11.0
```

- 결과는 11이 아니라 11.0이다.

### 정수형끼리 계산하면 원하는 결과값이 나오지 않을 수도 있다.

```java
Math.ceil(10/4);	// 2 -> 2.0
Math.ceil(10.0 / 4.0);	// 2.5 -> 3.0
```

## ArrayList.remove(), ArrayList.removeAll()

### remove(int index)

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
 
public class RemoveElementInArrayList {
    public static void main(String[] args) {
 
        List<Integer> list = new ArrayList<Integer>(Arrays.asList(5, 4, 3, 2, 1));
 
        // index 1 삭제
        list.remove(1);
 
        System.out.println(list); // [5, 3, 2, 1]
    }
}
```

- 파라미터로 int를 전달하면 해당 index의 값이 삭제된다.

### remove(Object obj)

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
 
public class RemoveElementInArrayList {
    public static void main(String[] args) {
 
        List<Integer> list = new ArrayList<Integer>(Arrays.asList(5, 4, 3, 2, 1, 1));
 
        // value가 1인 element 삭제
        list.remove(Integer.valueOf(1));
 
        System.out.println(list); // [5, 4, 3, 2, 1]
    }
}
```

- 해당 객체를 찾아 첫번째로 나오는 값만 삭제한다.
- 값을 삭제하면 true, 값이 없으면 false를 리턴한다.

### [응용] 특정 값 전체 삭제

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
 
public class RemoveElementInArrayList {
    public static void main(String[] args) {
 
        List<Integer> list = new ArrayList<Integer>(Arrays.asList(5, 4, 3, 2, 1, 1));
 
        // value가 1인 element 삭제
        while (list.remove(Integer.valueOf(1))) {
        };
 
        System.out.println(list); // [5, 4, 3, 2]
    }
}
```

### removeAll(Collection<?> c)

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
 
public class RemoveElementInArrayList {
    public static void main(String[] args) {
 
        List<Integer> list = new ArrayList<Integer>(Arrays.asList(5, 4, 3, 2, 1, 1));
 
        // value가 1, 2인 element 모두 삭제
        list.removeAll(Arrays.asList(Integer.valueOf(1), Integer.valueOf(2)));
 
        System.out.println(list); // [5, 4, 3]
    }
}
```

- 파라미터로 넘겨준 Collection 객체에 담긴 element를 모두 삭제한다.

## String[] ↔ List<String>

```java
// String[] -> List<String>
String[] arry = {"a", "b", "c" };
List<String> list = new ArrayList<String>(Arrays.asList(arry));

// List<String> -> String[]
List<String> list = new ArrayList<String>();
String[] arry = list.toArray(new String[list.size()]);
```

## TreeMap - lastKey, firstKey

```java
TreeMap<Integer,String> map = new TreeMap<Integer,String>(){{//초기값 설정
    put(1, "사과");//값 추가
    put(2, "복숭아");
    put(3, "수박");
}};
		
System.out.println(map); //전체 출력 : {1=사과, 2=복숭아, 3=수박}
System.out.println(map.firstKey());//최소 Key 출력 : 1
System.out.println(map.lastKey());//최대 Key 출력 : 3
```

### map.firstKey()

- 최소 key값 출력

### map.lastKey()

- 최대 key값 출력

## Map - containsKey, containsValue

### containsKey(T key)

```java
public class test {
	public static void main(String[] args) {
		HashMap<String, String> hashMap = new HashMap<String, String>();
		hashMap.put("A","APPLE");
		hashMap.put("B","BANANA");
		hashMap.put("C","CHERRY");
		hashMap.put("D","DURIAN");
		
		System.out.println(hashMap.containsKey("A"));
		System.out.println(hashMap.containsKey("E"));
	}
}
```

- 맵에서 인자로 보낸 키가 있으면 true, 없으면 false

### containsValue(T value)

```java
public class test {
	public static void main(String[] args) {
		HashMap<String, String> hashMap = new HashMap<String, String>();
		hashMap.put("A","APPLE");
		hashMap.put("B","BANANA");
		hashMap.put("C","CHERRY");
		hashMap.put("D","DURIAN");
        
		System.out.println(hashMap.containsValue("BANANA"));
		System.out.println(hashMap.containsValue("E????"));
	}
}
```

- 해당 값이 있으면 true, 없으면 false를 반환한다.

## 문자열 위치 찾기

### indexOf()

```java
String strValue = "Subject: Java, JavaScript, React, Node.JS";

int findIndex = strValue.indexOf("Java");

// 문자열 "Java"의 위치: 9
System.out.println("문자열 \"Java\"의 위치: " + findIndex);

String strValue = "Subject: Java, JavaScript, React, Node.JS";

int findIndex = strValue.indexOf("Java", 10);

// 문자열 "Java"의 위치: 15
System.out.println("문자열 \"Java\"의 위치: " + findIndex);
```

- 문자열의 처음 위치에서 특정 문자열 검색 후 위치(index)를 반환한다.
- 문자열에서 특정 문자열이 두 개 이상인 경우 시작 위치와 가장 가까운 위치를 반환한다.
- 특정 문자열이 존재하지 않는 경우 -1을 반환한다.
- 만약 특정 인덱스 위치부터 검색하고 싶은 경우 두번째 매개변수에 특정 위치를 전달한다.

### lastIndexOf()

```java
String strValue = "Subject: Java, JavaScript, React, Node.JS";

int findIndex = strValue.lastIndexOf("Java");

// 문자열 "Java"의 위치: 15
System.out.println("문자열 \"Java\"의 위치: " + findIndex);

String strValue = "Subject: Java, JavaScript, React, Node.JS";

int findIndex = strValue.lastIndexOf("Java", 14);

// 문자열 "Java"의 위치: 9
System.out.println("문자열 \"Java\"의 위치: " + findIndex);
```

- 문자열의 마지막 위치에서 특정 문자열 검색 후 위치를 반환한다.
- 문자열에서 특정 문자열이 두 개 이상인 경우 마지막 위치와 가장 가까운 위치를 반환한다.
- 특정 문자열이 존재하지 않는 경우 -1을 반환한다.
- 문자열을 특정 위치에서 반대 방향으로 검색하고 싶은 경우 두 번째 매개변수에 위치를 전달한다.

### 특정 문자열의 모든 위치 찾기

1. 정규식 사용
    
    ```java
    public static void main(String args[]) {
      String strValue = "Subject: Java, JavaScript, React, Node.JS";
    
      Matcher matcher = Pattern.compile("Java").matcher(strValue);
      while (matcher.find()) {
        System.out.println("문자열 \"Java\"의 위치: " + matcher.start());
      }
    }
    
    문자열 "Java"의 위치: 9
    문자열 "Java"의 위치: 15
    ```
    
2. 반복문 사용
    
    ```java
    public static void main(String args[]) {
      String strValue = "Subject: Java, JavaScript, React, Node.JS";
    
      int findIndex = strValue.indexOf("Java");
      
      while (findIndex >= 0) {
        System.out.println("문자열 \"Java\"의 위치: " + findIndex);
        findIndex = strValue.indexOf("Java", findIndex + 1);
      }
    }
    
    문자열 "Java"의 위치: 9
    문자열 "Java"의 위치: 15
    ```
    

## LocalTime 시간 다루기

### 객체 생성

```java
// 직접 지정하기
LocalTime time = LocalTime.of(19, 30, 20);

// String -> LocalTime
LocalTime timeParse = LocalTime.parse("19:30:20");

// 현재 시간
LocalTime timeNow = LocalTime.now();
```

- 객체는 **불변객체**이므로 값을 바꾸려면 새로운 객체를 생성하거나 기존 객체에 재할당해야한다.

### 속성 바꾸기

```java
LocalTime time = LocalTime.of(19, 30, 20);

// 특정 값으로 바꾸기
time = time.withHour(20);
time = time.withMinute(31);
time = time.withSecond(21);

// 상대 값으로 바꾸기
time = time.plusHours(1);
time = time.plusMinute(1);
time = time.plusSeconds(1);
```

### 값 읽기

```java
LocalTime time = LocalTime.of(19, 30, 20);

// 시간 값 읽기 (19)
int hour = time.getHour();
int hour = time.get(ChronoField.HOUR_OF_DAY);

// 분 값 읽기 (30)
int minute = time.getMinute();
int minute = time.get(ChronoField.MINUTE_OR_HOUR);

// 초 값 읽기
int minute = time.getSecond();
int minute = time.get(ChronoField.SECOND_OF_MINUTE);
```

### formatting

```java
DateTimeFormatter timeFormatter = DateTimeFormatter.ofPattern("H:mm:ss");
LocalTime time = LocalTime.parse("19:30:20", timeFormatter);

DateTimeFormatter timeFormatter = DateTimeFormatter.ofPattern("H:mm:ss");
time.format(timeFormatter);
```

## Duration, Period, ChronoUnit

### Duration

```java
LocalTime start = LocalTime.of(10, 35, 40);
LocalTime end = LocalTime.of(10, 36, 50, 800);

Duration duration = Duration.between(start, end);

System.out.println("Seconds: " + duration.getSeconds());
System.out.println("Nano Seconds: " + duration.getNano());
```

- 두 시간 사이의 간격을 초, 나노 초 단위로 나타낸다.

### Period

```java
LocalDate startDate = LocalDate.of(1939, 9, 1);
LocalDate endDate = LocalDate.of(1945, 9, 2);

Period period = Period.between(startDate, endDate);

System.out.println("Years: " + period.getYears());
System.out.println("Months: " + period.getMonths());
System.out.println("Days: " + period.getDays());
```

- 두 날짜 사이의 간격을 년/월/일 단위로 나타낸다.

### ChoronoUnit

```java
LocalDate startDate = LocalDate.of(1939, 9, 1);
LocalDate endDate = LocalDate.of(1945, 9, 2);

long months = ChronoUnit.MONTHS.between(startDate, endDate);
long weeks = ChronoUnit.WEEKS.between(startDate, endDate);
long days = ChronoUnit.DAYS.between(startDate, endDate);

System.out.println("Months: " + months);
System.out.println("Weeks: " + weeks);
System.out.println("Days: " + days);

LocalTime startTime = LocalTime.of(10, 35, 40);
LocalTime endTime = LocalTime.of(10, 36, 50, 800);

long hours = ChronoUnit.HOURS.between(startTime, endTime);
long minutes = ChronoUnit.MINUTES.between(startTime, endTime);
long seconds = ChronoUnit.SECONDS.between(startTime, endTime);

System.out.println("Hours: " + hours);
System.out.println("Minutes: " + minutes);
System.out.println("Seconds: " + seconds);
```

- Duration, Period 객체를 생성하지 않고도 간단하게 특정 시간 단위로 시간의 길이를 구하는 방법

## 참고 자료

- https://mine-it-record.tistory.com/128
- https://luanaeun.tistory.com/208
- https://hianna.tistory.com/564
- https://mcpaint.tistory.com/236
- https://arabiannight.tistory.com/65
- https://dev-coco.tistory.com/39
- https://dpdpwl.tistory.com/138
- https://developer-talk.tistory.com/650
- https://dev-cho.tistory.com/47
- https://www.daleseo.com/java8-duration-period/