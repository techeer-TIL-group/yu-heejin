## compareTo

- 문자열 비교 혹은 숫자를 비교한다.
- 문자열 비교 같은 경우 `같다 = 0`, `그 외 양수/음수 값`을 반환한다.
- 숫자의 비교는 `크다 = 1`, `같다 = 0`, `작다 = -1` 값을 반환한다.

### 숫자형 비교

```java
public class CompareToTest{
    public static void main(String[] args){

        Integer x = 3;
        Integer y = 4;
        Double z = 1.0;

        System.out.println( x.compareTo(y) );  // -1
        System.out.println( x.compareTo(3) );  //  0
        System.out.println( x.compareTo(2) );  //  1
        System.out.println( z.compareTo(2.7) );  //  -1

    }
}
```

- 숫자형 비교는 참조형만 비교 가능하다.
    - `Byte`, `Double`, `Integer`, `Float`, `Long`, `Short` 등
- 반환되는 값은 아래와 같은 규칙을 따른다.
    - 기준 값과 비교 대상이 동일한 값일 경우 0
    - 기준 값이 비교 대상보다 작은 경우 -1
    - 기준 값이 비교 대상보다 큰 경우 1
- 만약 int 타입을 가지고 비교하고자 한다면 다음과 같이 사용한다.
    
    ```java
    int x = 4;  
    int y = 5;
    Integer.compare(x,y);
    ```
    

### 문자열 비교

```java
public class CompareToTest{
    public static void main(String[] args){

        String str = "abcd";

        // 1) 비교대상에 문자열이 포함되어있을 경우
        System.out.println( str.compareTo("abcd") );  // 0 (같은 경우는 숫자나 문자나 0을 리턴)
        System.out.println( str.compareTo("ab") );  //  2
        System.out.println( str.compareTo("a") );  //  3
        System.out.println( str.compareTo("c") );  //  -2       
        System.out.println( "".compareTo(str) );  //  -4

        // 2) 비교대상과 전혀 다른 문자열인 경우
        System.out.println( str.compareTo("zefd") );  //  -25
        System.out.println( str.compareTo("zEFd") );  //  -25
        System.out.println( str.compareTo("ABCD") );  //  32
    }

}
```

1. 비교 대상에 문자열이 포함되어 있을 경우
    - abcd에 ab가 포함되어 있으면, 즉 **기준 값에 비교 대상이 포함되어 있을 경우 서로의 문자열 길이의 차이값을 리턴**해준다.
        
        ```java
        "abcd"(4) - "ab"(2) = 2,
        "abcd"(4) - "a"(1) = 3,
        "abcd"(4) - ""(0) = 4
        ```
        
2. 비교 대상과 전혀 다른 문자열인 경우
    - `str.compareTo(”c”)`의 결과값이 -2가 나오는 이유는 `compareTo`는 같은 위치의 문자만 비교하기 때문에, 첫번째 문자부터 순서대로 비교해서 다를 경우 바로 아스키값을 기준으로 비교 처리를 하기 때문이다.
    - 비교 불가능한 지점의 각 문자열의 아스키값을 기준으로 비교한다.

## 특정 날짜의 요일 구하기

### LocalDateTime, LocalDate (Java 8)

- 숫자로 구하기
    
    ```java
    import java.time.DayOfWeek;
    import java.time.LocalDate;
     
    public class GetDayOfWeek {
        public static void main(String[] args) {
     
            // 1. LocalDate 생성
            LocalDate date = LocalDate.of(2021, 12, 25);
            // LocalDateTime date = LocalDateTime.of(2021, 12, 25, 1, 15, 20);
            System.out.println(date);  // // 2021-12-25
     
            // 2. DayOfWeek 객체 구하기
            DayOfWeek dayOfWeek = date.getDayOfWeek();
     
            // 3. 숫자 요일 구하기
            int dayOfWeekNumber = dayOfWeek.getValue();
     
            // 4. 숫자 요일 출력
            System.out.println(dayOfWeekNumber);  // 6
     
        }
    }
    ```
    
    - `LocalDate`는 날짜를 나타내는 클래스이며, `LocalDateTime`은 날짜와 시간을 나타내는 클래스이다.
    - 요일을 표현하는 `DayOfWeek` Enum을 가져와 요일을 구할 수 있다.
    - `DayOfWeek`의 `getValue()` 메소드를 사용하면 요일을 숫자로 가져올 수 있다.
        - 월요일부터 일요일까지 1 ~ 7의 숫자로 표현할 수 있다.
- 텍스트로 구하기 (영문, 한글)
    
    ```java
    import java.time.DayOfWeek;
    import java.time.LocalDate;
    import java.time.format.TextStyle;
    import java.util.Locale;
     
    public class GetDayOfWeek {
        public static void main(String[] args) {
     
            // 1. LocalDate 생성
            LocalDate date = LocalDate.of(2021, 12, 25);
            System.out.println(date); // 2021-12-25
     
            // 2. DayOfWeek 객체 구하기
            DayOfWeek dayOfWeek = date.getDayOfWeek();
     
            // 3. 텍스트 요일 구하기 (영문)
            System.out.println(dayOfWeek.getDisplayName(TextStyle.FULL, Locale.US));  // Saturday
            System.out.println(dayOfWeek.getDisplayName(TextStyle.NARROW, Locale.US));  // S
            System.out.println(dayOfWeek.getDisplayName(TextStyle.SHORT, Locale.US));  // Sat
     
            // 4. 텍스트 요일 구하기 (한글)
            System.out.println(dayOfWeek.getDisplayName(TextStyle.FULL, Locale.KOREAN));  // 토요일
            System.out.println(dayOfWeek.getDisplayName(TextStyle.NARROW, Locale.KOREAN));  // 토
            System.out.println(dayOfWeek.getDisplayName(TextStyle.SHORT, Locale.KOREAN));  // 토
     
            // 5. 텍스트 요일 구하기 (default)
            System.out.println(dayOfWeek.getDisplayName(TextStyle.FULL, Locale.getDefault()));  // 토요일
     
        }
    }
    ```
    
    - `public String getDisplayName(TextStyle style, Locale locale)`
        - 요일을 텍스트로 리턴한다.
        - TextStyle은 요일을 어떤 형식으로 보여줄지 결정한다.
            - `TextStyle.FULL (전체 텍스트)` / `TextStyle.NARROW (단축형)` / `TextStyle.SHORT (단축형)`

### Date, Calendar (Java 8 이전)

- **Date 클래스는 현재 많은 메소드들이 deprecated 되어 사용을 권장하진 않는다.**
- 숫자로 구하기
    
    ```java
    import java.util.Calendar;
    import java.util.Date;
     
    public class GetDayOfWeekForDate {
        public static void main(String[] args) {
            // 1. Date 생성 / 현재 날짜
            Date currentDate = new Date();
            System.out.println(currentDate); // Sat Jun 19 11:07:17 KST 2021
     
            // 2. Calendar 생성
            Calendar calendar = Calendar.getInstance();
            calendar.setTime(currentDate);
     
            // 3. 텍스트 요일 구하기 (숫자)
            int dayOfWeekNumber = calendar.get(Calendar.DAY_OF_WEEK);
     
            // 4. 요일 출력
            System.out.println(dayOfWeekNumber);  // 7
        }
    ```
    
    - `int dayOfWeekNumber = calendar.get(Calendar.DAY_OF_WEEK);`
        - `DAY_OF_WEEK`를 전달해 요일을 나타내는 숫자를 가져올 수 있다.
        - 리턴되는 숫자는 1~7로, 1은 일요일, 7은 토요일을 나타낸다.
- 텍스트로 구하기 (한글, 영문)
    
    ```java
    import java.util.Calendar;
    import java.util.Date;
    import java.util.Locale;
     
    public class GetDayOfWeekForDate {
        public static void main(String[] args) {
            // 1. Date 생성 / 현재 날짜
            Date currentDate = new Date();
            System.out.println(currentDate); // Sat Jun 19 11:07:17 KST 2021
     
            // 2. Calendar 생성
            Calendar calendar = Calendar.getInstance();
            calendar.setTime(currentDate);
     
            // 3. 텍스트 요일 구하기 (영문)
            System.out.println(calendar.getDisplayName(Calendar.DAY_OF_WEEK, Calendar.LONG, Locale.US)); // Saturday
            System.out.println(calendar.getDisplayName(Calendar.DAY_OF_WEEK, Calendar.NARROW_FORMAT, Locale.US)); // S
            System.out.println(calendar.getDisplayName(Calendar.DAY_OF_WEEK, Calendar.SHORT, Locale.US)); // Sat
     
            // 4. 텍스트 요일 구하기 (한글)
            System.out.println(calendar.getDisplayName(Calendar.DAY_OF_WEEK, Calendar.LONG, Locale.KOREAN)); // 토요일
            System.out.println(calendar.getDisplayName(Calendar.DAY_OF_WEEK, Calendar.NARROW_FORMAT, Locale.KOREAN)); // 토
            System.out.println(calendar.getDisplayName(Calendar.DAY_OF_WEEK, Calendar.SHORT, Locale.KOREAN)); // 토
     
            // 5. 텍스트 요일 구하기 (default)
            System.out.println(calendar.getDisplayName(Calendar.DAY_OF_WEEK, Calendar.LONG, Locale.getDefault())); // 토요일
        }
    }
    ```
    
    - `public String getDisplayName(int field, int style, Locale locale)`
        - 파라미터로 전달받은 field의 값을 style과 locale을 적용하여 텍스트로 리턴한다.
        - 요일을 구하기 위해 Calendar.DAY_OF_WEEK 상수를 전달한다.

## charAt()

- String으로 저장된 문자열 중에서 한 글자만 선택해서 char타입으로 변환해준다.
- `String.charAt(index);`

## ArrayList 내부 값 변경 방법

- `set()` 메서드를 사용하여 **특정 인덱스의 값을 변경**할 수 있다.
    
    ```java
    E set(int index, E element);
    ```
    
- `replaceAll()` 메서드를 사용하면 **특정 조건을 만족하는 모든 요소의 값을 변경**할 수 있다.
    
    ```java
    public static void main(String args[]) {
      List<Integer> intList = new ArrayList<>(Arrays.asList(1, 2, 3, 1, 2, 3));
      System.out.println("변경 전: " + intList);
    
      intList.replaceAll(num -> num == 1 ? num * 10 : num);
    
      System.out.println("변경 후: " + intList);
    }
    ```
    
    ```java
    default void replaceAll(UnaryOperator<E> operator)
    ```
    
    - 함수형 인터페이스 타입을 매개변수로 가지기 때문에 람다 표현식을 전달할 수 있다.
- `Collections.replaceAll()` 메서드로 특정 조건이 아닌 **특정 값을 다른 값으로 변경**할 수 있다.
    
    ```java
    public static <T> boolean replaceAll(List<T> list, T oldVal, T newVal);
    ```
    

## 형변환

### String ↔ int

```java
String s = "12345";
int i = Integer.parseInt(s);
int i = Integer.valueOf(s);

int i = 12345;
String s = Integer.toString(i);
String s = String.valueOf(i);
```

### char ↔ int

```java
char ch = '5';
int i = (int)(ch - '0');

int i = 5;
char ch = (char)(i + '0');
```

### char ↔ String

```java
char ch1 = '5';
char[] ch2 = {'a','b', 'c'};

String s1 = String.valueOf(ch1); // '5'
String s2 = String.valueOf(ch2); // 'abc'

String s1 = "1";
String s2 = "1234";

char ch1 = s1.charAt(0); // '1'
char[] ch2 = s2.toCharArray(); // 1234
```

### int ↔ double

```java
double d = 1010.10101010; // double타입은 64비트로 실수를 표현
float f = 1010.101010f; // float타입은 32비트로 실수를 표현, 리터럴에 f를 붙혀 실수임을 표기해야함.

int i;
i = (int)d; //Double To Int
i = (int)f; //Float To Int

int i= 1234;
	
double d = (double)i; //Int To Double
float f = (float)i; //Int To Float
```

## Math.sqrt()

```java
System.out.println(Math.sqrt(25));  // 5.0
```

- 특정 값의 제곱근(루트)를 구한다.
- `Math.sqrt()`의 입력값과 출력값은 모두 double 형이다.

## 문자열에 숫자만 존재하는지 확인

- 정규식과 `String.matches()`를 사용한다.
    
    ```java
    public class StringUtils {
    
    	public static void main(String[] args) {
    		final String REGEX = "[0-9]+";
    		String test = "20201119173455"; //년월일시분초
    		
    		if(test.matches(REGEX)) {
    			System.out.println("숫자만 있습니다.");
    		}else {
    			System.out.println("숫자외에 값이 존재합니다.");
    		}
    	}
    }
    ```
    

## 맵에 key, value가 있는지 확인

### containsKey(key)

- 맵에 해당 key가 있는지 확인하여 true, false를 리턴한다.
    
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
    /////Console/////
    true
    false
    ```
    

### containsValue(val)

- 맵에 해당 value가 있는지 확인하여 true, false를 반환한다.
    
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
    /////Console/////
    true
    false
    ```
    

## 자바 2차원 배열

### 2차원 배열 선언하기

```java
int[][] arrays;
int [][]arrays;

int array[][];   // X
```

### 2차원 배열 출력하기

```java
for (int i = 0; i < array.length; i++) {
	System.out.println(Arrays.toString(array[i]));
}
```

- `Arrays.toString()` 메서드를 사용한다.

## 참고 자료

- https://mine-it-record.tistory.com/133
- https://hianna.tistory.com/610
- https://colossus-java-practice.tistory.com/31
- https://developer-talk.tistory.com/737
- https://dodo-factory.tistory.com/9
- https://coding-factory.tistory.com/532
- https://myhappyman.tistory.com/183
- https://dpdpwl.tistory.com/138
- https://keichee.tistory.com/423