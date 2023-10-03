## compareTo

```java
String s1 = "abc";
String s2 = "def";
System.out.println("abc compareTo def : " + s1.compareTo(s2));
System.out.println("def compareTo abc : " + s2.compareTo(s1));
System.out.println("abc compareTo abc : " + s1.compareTo(s1));
```

- 문자열 사전순 비교
- 매개변수 문자열이 더 크면 음수, 같으면 0, 작으면 양수가 나온다.
- 실제 `compareTo`의 로직은 두 문자열을 문자 단위로 비교한 후, **다른 문자가 있으면 문자끼리 뺀 값을 리턴한다.**
    - 두 문자가 같으면 두 문자의 길이를 뺀 값이 리턴된다.

## 문자열 대문자, 소문자 확인

### 문자인 경우

```java
public static void main(String args[]) {
  char charValue = 'A';

  if(Character.isUpperCase(charValue)) {
    System.out.println("A는 대문자입니다.");
  }

  if(Character.isLowerCase(charValue)) {
    System.out.println("A는 소문자입니다.");
  }
}
```

- `isUpperCase()`
    - 매개변수로 char 타입의 값 또는 int 타입의 값을 전달한다.
    - 전달된 값이 대문자인 경우 true, 아니라면 false
- `isLowerCase()`
    - 매개변수로 char 타입의 값 또는 int 타입의 값을 전달한다.
    - 전달된 값이 소문자인 경우 true를 반환하고 그렇지 않으면 false를 반환한다.

### 문자열인 경우 - 반복문

```java
private static boolean isStringUpperCase(String str){
  char[] charArray = str.toCharArray();

  for(int index = 0; index < charArray.length; index++){
    if( !Character.isUpperCase( charArray[index] ))
      return false;
  }
  return true;
}

public static void main(String args[]) {
  String strValue1 = "ABC";
  String strValue2 = "abc";
  String strValue3 = "A1B2";

  System.out.println("\"" + strValue1 + "\"는 대문자인가? " + isStringUpperCase(strValue1));
  System.out.println("\"" + strValue2 + "\"는 대문자인가? " + isStringUpperCase(strValue2));
  System.out.println("\"" + strValue3 + "\"는 대문자인가? " + isStringUpperCase(strValue3));
}
```

- 문자열을 char타입의 배열로 변환한 후 배열의 요소를 순회하며 `isUpperCase` or `isLowerCase`를 호출한다.

### 문자열인 경우 - 대문자 또는 소문자로 변경 후 비교

```java
private static boolean isStringUpperCase(String str){
  if(!str.equals(str.toUpperCase())) return false;
  return true;
}

public static void main(String args[]) {
  String strValue1 = "ABC";
  String strValue2 = "abc";

  System.out.println("\"" + strValue1 + "\"는 대문자인가? " + isStringUpperCase(strValue1));
  System.out.println("\"" + strValue2 + "\"는 대문자인가? " + isStringUpperCase(strValue2));
}
```

- `toUpperCase()`
    - 대문자로 변환된 문자열을 반환한다.
- `toLowerCase()`
    - 소문자로 변환된 문자열을 반환한다.

## 문자열 붙이기

### concat

```java
String str1 = "첫번째 텍스트입니다 "; 
String str2 = "두번째 텍스트입니다"; 
System.out.println("결과: " + str1.concat(str2)); 

//결과 : 첫번째 텍스트입니다 두번째 텍스트입니다
```

- String 클래스에서 제공하는 기본 메서드
- 합친 문자열을 String으로 생성한다.
- `concat()` 메서드를 이용해 문자열을 추가할 때마다 새로운 인스턴스를 생성하기 때문에 성능이나 속도면에서 좋지 않다.

### ‘+’ 연산자

- jdk 1.5 이전에는 `concat()` 메서드처럼 문자열을 추가할 때마다 새로운 인스턴스를 생성했지만, **StringBuilder로 변환해서 처리하는 것으로 변경되었다.**

### StringBuilder

```java
StringBuilder sb = new StringBuilder();
sb.append("str1");
sb.append("str2");
String concat = sb.toString();
```

- `append()` 함수를 통해 문자열을 덧붙일 수 있다.
- String과의 차이점은 **‘수정 가능’** 하다는 것이다.
    - String은 immutable한 객체이기 때문에 값을 수정하려면 다른 값을 가진 String을 다시 대입하는 식으로 처리해야 한다.
- StringBuilder는 새로운 String 객체를 생성하여 메모리에 할당하는 과정 없이도 수정이 가능하다.
- 복잡하거나 반복적인 문자열 수정 시 사용하는 것이 좋다.

### StringBuffer

```java
StringBuffer sbf = new StringBuffer();
sbf.append("str1");
sbf.append("str2");
String concat = sbf.toString();
```

- StringBuilder와 호환 가능하기 때문에 사용법은 동일하다.
- StringBuffer는 StringBuilder와 달리 thread-safe하다는 점이다.
    - StringBuilder는 동기화를 보장하지 않는다.
    - StringBuffer는 동시에 해당 객체에 접근했을 때 동시성을 제어하는 기능이 존재하고, StringBuilder 클래스는 동시성 기능을 제외하여 상대적으로 동작 속도가 빠르다.
- 쓰임새는 비슷하나 하나의 문자열을 멀티 스레드를 통해 수정할 필요가 있다면 이 클래스를 사용하는 것이 좋다.

## 참고 자료

- https://priming.tistory.com/7
- https://developer-talk.tistory.com/684
- [https://junghn.tistory.com/entry/JAVA-문자열-붙이는-방법concat-StringBuilder-StringBuffer](https://junghn.tistory.com/entry/JAVA-%EB%AC%B8%EC%9E%90%EC%97%B4-%EB%B6%99%EC%9D%B4%EB%8A%94-%EB%B0%A9%EB%B2%95concat-StringBuilder-StringBuffer)