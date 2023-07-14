하도 까먹어서 다시 정리해봤다.

## ArrayList 값 중 최대값, 최소값 구하기

- Collections.max(): List의 최대값 구하기
- Collections.min(): List의 최소값 구하기

## 배열에서 특정 값의 인덱스 구하기

```java
import java.util.Arrays;

public class IndexOfTest {

    public static void main(String[] args) {
			String[] arr = {"a","b","c"};
			System.out.println(Arrays.asList(arr).indexOf("b")); //1이 출력된다.
	}
}
```

- indexOf(): 자료구조에서 특정 문자의 인덱스를 찾기 위해 사용된다.
- 자바 정적 배열에서는 indexOf()를 지원하지 않고 List 자료구조만 지원한다.
- 해당 메소드는 String 문자열 내에서도 사용할 수 있다.

## ArrayList 메소드

- add(): 값 추가하기
- set(): 값 변경하기
- remove(): 매개변수로 들어온 값 중 일치하는 첫번째 값 삭제하기
- clear(): 전체 값 삭제하기
- get(): 값 가져오기

## String에서 원하는 문자열 추출하기

- indexOf(String a): a의 문자의 위치 값 얻기
- lastIndexOf(String a): a문자를 뒤에서부터 찾아 위치 값 숫자를 얻는다.
- subString(int a, int b): a부터 b전까지의 위치의 문자열을 가져온다.
- subString(int index): 문자열 index위치부터 끝까지
- charAt(index): String 문자열에서 index 번쨰 문자 값 1개를 가져온다.
- indexOf(string): 문자열을 찾아 존재하면 첫째 문자 위치 값을 반환, 없으면 -1 반환

## Arrays.fill()

- 배열의 모든 값을 같은 값으로 초기화하는 메서드

## Arrays.toString()

```java
double[] values = {1.0, 1.1, 1.2};

System.out.println(values.toString()); // 이렇게 하면 [D@46a49e6 같은 값이 나옵니다.
System.out.println(Arrays.toString(values)); // 이렇게 하면 [1.0, 1.1, 1.2] 이 출력됩니다.
```

- 자바 배열 내용 출력

## 배열 ↔ Set

### 배열 → Set

```java
import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;
 
public class ArrayToSet {
    
    public static void main(String[] args) {
        
        // Set으로 변환할 배열
        Integer[] arr = { 1, 1, 2, 3, 4 };
 
        // 배열 -> Set
				// HashSet의 생성자 파라미터로 List를 넘겨준다.
        Set<Integer> set = new HashSet<Integer>(Arrays.asList(arr));
 
        // Set 출력
        System.out.println(set); // [1, 2, 3, 4]
    }
}
```

### Set → 배열

```java
import java.util.Arrays;
import java.util.Set;
 
import com.google.common.collect.Sets;
 
public class SetToArray {
    public static void main(String[] args) {
        
        // Set 객체
        Set<Integer> set = Sets.newHashSet(1, 2, 3, 4);
 
        // Set -> 배열
				// 파라미터로 반환된 배열 객체를 넘겨준다.
				// 이때 배열의 크기를 0으로 지정하면 자동으로 배열의 크기가 지정된다.
        Integer[] arr = set.toArray(new Integer[0]);
 
        // 배열 출력
        System.out.println(Arrays.toString(arr)); // [1, 2, 3, 4]
    }
}
```

## Map 순회하기

### Iterator를 사용하여 접근하기

```java
Map<String, String> map = new HashMap<>();
 
Iterator<String> keys = map.keySet().iterator();
while (keys.hasNext()) {
    String key = keys.next();
    map.get(key);
}
```

### EntrySet으로 접근하기

```java
Map<String, String> map = new HashMap<>();
 
for (Map.Entry<String, String> entry : map.entrySet()) {
    String key = entry.getKey();
    String value = entry.getValue();
}
```

- Map은 하나의 원소로 Key-Value 묶음을 가지기 때문에 원소란 표현 대신 Entry라고 표현한다.

### KeySet을 이용해 접근하기

```java
Map<String, String> map = new HashMap<>();
 
for (String key : map.keySet()) {
    map.get(key);
}
```

## 참고 자료

- https://hianna.tistory.com/571
- [https://velog.io/@chosj1526/Java-배열에서-특정값의-인덱스-구하기](https://velog.io/@chosj1526/Java-%EB%B0%B0%EC%97%B4%EC%97%90%EC%84%9C-%ED%8A%B9%EC%A0%95%EA%B0%92%EC%9D%98-%EC%9D%B8%EB%8D%B1%EC%8A%A4-%EA%B5%AC%ED%95%98%EA%B8%B0)
- https://hianna.tistory.com/568
- [https://www.appletong.com/entry/자바-String-원하는-문자열-추출-indexOf-subString-chatAt-token-parseInt](https://www.appletong.com/entry/%EC%9E%90%EB%B0%94-String-%EC%9B%90%ED%95%98%EB%8A%94-%EB%AC%B8%EC%9E%90%EC%97%B4-%EC%B6%94%EC%B6%9C-indexOf-subString-chatAt-token-parseInt)
- https://crazykim2.tistory.com/545
- https://crmn.tistory.com/61
- https://hianna.tistory.com/616
- https://pangtrue.tistory.com/232