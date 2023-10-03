## 문자열이 숫자인지 확인

### 일반 Java 라이브러리 사용

```java
public static boolean isNumeric(String strNum) {
    if (strNum == null) {
        return false;
    }
    try {
        double d = Double.parseDouble(strNum);
    } catch (NumberFormatException nfe) {
        return false;
    }
    return true;
}
```

- `Integer.parseInt()` 사용

### 정규 표현식 사용

```java
private Pattern pattern = Pattern.compile("-?\\d+(\\.\\d+)?");

public boolean isNumeric(String strNum) {
    if (strNum == null) {
        return false; 
    }
    return pattern.matcher(strNum).matches();
}
```

- [추가] `\d+`을 쓰는 방식이 `[0-9]+`를 쓰는 방식보다 더 빠르다.
    - `\d+` 방식은 Character.isDigit보다 더 빠를 때도 있다.
    
    ```java
    public static boolean isNumberString(String s) {
      if (s == null || s.isBlank()) {
        return false;
      }
      return s.matches("\\d+");
    }
    ```
    

### allMatch, anyMatch 사용하기

```java
public static boolean isNumberString(String s) {
	if (s == null || s.isBlank()) {
	  return false;
	}
	return s.chars()
	        .allMatch(Character::isDigit);
}

public static boolean isNumberString(String s) {
	if (s == null || s.isBlank()) {
	  return false;
	}
	return !s.chars()
	         .anyMatch(c -> !Character.isDigit(c));
}
```

- `allMatch`를 사용하는 방식이 `anyMatch`를 사용하는 방식보다 빠르다.

### char 크기로 비교하기

```java
public static boolean isNumericArray_c_style(String s) {
  if (s == null) {
    return false;
  }
  for (char c : s.toCharArray()) {
    if (c < '0' || c > '9') {
      return false;
    }
  }
  return true;
}
```

### Character.isDigit()

```java
public static boolean isNumberString(String s) {
  if (s == null || s.isBlank()) {
    return false;
  }
  for (int i = 0; i < s.length(); i++) {
    if (!Character.isDigit(s.charAt(i))) {
      return false;
    }
  }
  return true;
}
```

- 명시된 char 값이 숫자인지 여부를 판단하여 true / false 값으로 리턴한다.

## 문자열에 특정 문자열이 포함되어 있는지 확인

### contains() 메서드

```java
String str = "Java Programming";

System.out.println(str.contains("Java"));
System.out.println(str.contains("java"));
```

- 특정 문자열이 존재하는지 확인할 수 있다.
- 대소문자를 구분한다.

### indexOf() 메서드

```java
String str = "Java Programming";

System.out.println(str.indexOf("Programming"));
System.out.println(str.indexOf("C#"));
```

- 특정 문자열이 존재하면 해당 문자열의 첫번째 인덱스가 반환된다.
- 해당 문자열이 없으면 -1을 반환한다.

### lastIndexOf() 메서드

```java
String str = "Java1 Java2 Java3";

System.out.println(str.indexOf("Java"));
System.out.println(str.lastIndexOf("Java"));
```

- 문자열의 마지막 위치에서 특정 문자열을 검색한다.

### 정규식(Regex)

```java
Pattern pattern = Pattern.compile(".*" + "Programming" + ".*");

Matcher matcher = pattern.matcher("Java Programming Hello!");

System.out.println(matcher.find());
```

- 문자열에 특정 패턴 또는 규칙이 존재하는지 검증하기 위해 사용된다.
- Pattern 클래스는 패턴을 컴파일하여 작동한다.
    - 특정 문자열이 정규식 패턴에 일치한지 검증하기 위해 컴파일된 패턴을 Matcher 클래스의 객체에 할당한다.
- Matcher 클래스의 객체에서 `find()` 메서드를 호출하여 패턴이 일치한지 검증한다.

### StringUtils 클래스

- Apache Commons의 StringUtils 클래스가 존재한다.
- `contains()` 메서드는 String 클래스의 contains() 메서드와 동일하게 동작한다.
    
    ```java
    String str = "Java Programming";
    
    System.out.println(StringUtils.contains(str, "Java"));
    System.out.println(StringUtils.contains(str, "C#"));
    ```
    
- `containsIgnoreCase()` 메서드는 대소문자를 무시하고 비교할 경우 사용한다.
    
    ```java
    String str1 = "Java Programming";
    String str2 = "JAVA proGRAmming";
    
    System.out.println(StringUtils.containsIgnoreCase(str1, str2));
    ```
    
- `indexOfAny()` 메서드는 단일 문자열이 아닌 여러 문자열이 포함되었는지 확인하고 싶을 경우 사용한다.
    
    ```java
    String str = "Java Programming";
    
    System.out.println(StringUtils.indexOfAny(str, new String[] {"Programming", "C#"}));
    System.out.println(StringUtils.indexOfAny(str, new String[] {"JavaScript", "C#"}));
    ```
    
    - 여러 문자열이 포함된 경우 시작 위치와 가장 가까운 인덱스가 반환된다.
- `indexOfDifference()` 메서드는 두 문자열을 비교하여 두 문자열이 달라지는 위치(인덱스)를 반환한다.
    
    ```java
    String str1 = "Java Programming";
    String str2 = "Java Programming";
    String str3 = "JavaScript Programming";
    
    System.out.println(StringUtils.indexOfDifference(str1, str2));   // -1
    System.out.println(StringUtils.indexOfDifference(str2, str3));   // 4
    ```
    
    - 두 문자열이 동일한 경우 -1이 반환된다.

## 숫자를 문자로 형변환하기

### String str = number + “”

### String.valueOf(int i)

### Integer.ToString(int i)

### String.Format(String format, Object …args)

```java
String str = "";
int i = 123;

str = String.Format("%d", i);
```

### NumberFormat.getInstance().Format(long number)

```java
String str = "";
int i = 123;

str = numberFormat.getInstance().Format(i);
```

- NumberFormat 클래스에는 각종 숫자를 형식화할 수 있는 메서드들이 포함되어 있다.

### new DecimalFormat(String pattern).Format(long number)

```java
String str = "";
int i = 123;

str = new DecimalFormat("#").Format(n);
str = DecimalFormat.getNumberInstance().Format(n);
```

## 특정 인덱스에서 배열 자르기

### 반복문 이용

```java
import java.util.Arrays;
 
public class ArraySplit {
    public static void main(String[] args) {
        
        // 1. 원본 배열
        int[] arr = {0, 1, 2, 3, 4, 5};
 
        // 2. 배열을 자를 index
        int position = 3;
 
        // 3. 자른 배열을 담을 새로운 배열
        int[] arr1 = new int[position];
        int[] arr2 = new int[arr.length - position];
 
        // 4. 배열 자르기
        for(int i = 0; i < arr.length; i++)  {
            if( i < position)   {
                arr1[i] = arr[i];
            }else   {
                arr2[i - position] = arr[i];
            }
        }
 
        // 5. 자른 배열 출력
        System.out.println(Arrays.toString(arr1));  // [0, 1, 2]
        System.out.println(Arrays.toString(arr2));  // [3, 4, 5]
        
    }
}
```

### Arrays.copyOfRange()

```java
import java.util.Arrays;
 
public class ArraySplit {
    public static void main(String[] args) {
        
        // 1. 원본 배열
        int[] arr = {0, 1, 2, 3, 4, 5};
 
        // 2. 배열을 자를 index
        int position = 3;
 
        // 3. 배열 자르기
        int[] arr1 = Arrays.copyOfRange(arr, 0, position);  // position - 1까지 배열을 자른다.
        int[] arr2 = Arrays.copyOfRange(arr, position, arr.length);
 
        // 4. 자른 배열 출력
        System.out.println(Arrays.toString(arr1));  // [0, 1, 2]
        System.out.println(Arrays.toString(arr2));  // [3, 4, 5]
 
    }
}
```

## 문자열 배열을 문자열로 합치기

### String.join()

```java
String[] company = { "Apple", "Amazon", "Google", "Microsoft"};
String joinString = String.join(", ", company);
 
System.out.println(joinString);   // Apple, Amazon, Google, Microsoft
```

- 구분 문자를 생략하면 공백으로 배열의 원소를 연결한다.
- List 컬렉션도 가능하다.

## char 배열을 문자열로 변환하기

### String 생성자를 사용하는 방법

```java
public void charArrayToString1() {
    char[] charArray = { 'H', 'e', 'l', 'l', 'o', 'W', 'o', 'r', 'l', 'd' };
    String str = new String(charArray);
    System.out.println(str);
}
```

### String.valueOf() 메소드를 사용

```java
public void charArrayToString2() {
    char[] charArray = { 'H', 'e', 'l', 'l', 'o', 'W', 'o', 'r', 'l', 'd' };
    String str = String.valueOf(charArray);
    System.out.println(str);
}
```

## 자바 소수점 버리기

### Math.ceil(): 소수점 올림, 정수 반환

```java
double num = 99.99;
System.out.println(Math.ceil(num)); // 100
```

### Math.floor(): 소수점 버림, 정수 반환

```java
double num = 99.99;
System.out.println(Math.floor(num)); // 99
```

### Math.round(): 소수점 반올림, 정수 반환

```java
double num = 99.99;
System.out.println(Math.round(num)); // 100
```

## 참고 자료

- https://recordsoflife.tistory.com/1463
- https://developer-talk.tistory.com/406
- https://chragu.com/entry/Java-int-to-String
- https://hianna.tistory.com/619
- https://covenant.tistory.com/259
- [https://codechacha.com/ko/java-convert-chararray-to-string/#4-stream--char를-문자열로-변환](https://codechacha.com/ko/java-convert-chararray-to-string/#4-stream--char%EB%A5%BC-%EB%AC%B8%EC%9E%90%EC%97%B4%EB%A1%9C-%EB%B3%80%ED%99%98)
- https://ssj9902.tistory.com/99
- https://jamesdreaming.tistory.com/157
- https://johngrib.github.io/wiki/problem/is-number-string/