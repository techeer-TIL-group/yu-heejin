## JVM

- JVM이란 **OS의 메모리 영역에 접근하여 Java의 메모리를 관리**하는 가상의 프로그램이다.
- C/C++을 사용했을 땐 사용자가 직접 메모리 관리를 해줬다.
    - C의 경우 `calloc`, `realloc`, `malloc`으로 메모리를 할당하고, 사용 후 free로 직접 해제해야 한다.
- 자바의 경우 이러한 메모리 관리를 **GC(Garbage Collector)가 대신 수행**한다.
    - GC란 프로그램에서 동적으로 할당된 메모리 영역 중에서 **사용하지 않는 영역을 탐지해 해제하는 역할을 한다.**

## Java의 Stack, Heap

- 자바에서 메모리는 Stack과 Heap 영역으로 나뉜다.

### Stack

- Stack은 **정적으로 할당된 메모리 영역**이다.
- **Primitive 타입**인 `boolean`, `char`, `short`, `int` 등의 데이터가 할당된다.
- Heap 영역에 생성된 **Object타입의 데이터 참조 값**이 할당된다.
- Stack의 메모리는 **스레드 당 하나씩 할당**된다.
    - 만약 새로운 스레드가 생성되면 해당 스레드에 대한 Stack이 새롭게 생성되고, 각 스레드끼리는 Stack 영역에 접근할 수 없다.

### Heap

- **동적으로 할당된 메모리 영역**
- **모든 Object 타입의 데이터**가 할당된다.
- Heap 영역의 **Object를 가리키는 참조 변수가 Stack에 할당**된다.
- Heap영역의 데이터들은 생명주기가 긴데, 대부분 Object의 크기가 크고 서로 다른 코드블록에서도 공유되기 때문이다.
- 또한, **Heap은 Stack처럼 스레드마다 하나씩 있는 것이 아니라 여러개의 스레드가 있어도 Heap은 단 하나의 영역만 존재한다.**

## String

```java
public class Main {
    public static void main(String[] args) {
        String name = "kang";
        System.out.println("Before Name : " + name);
        changeName(name);
        System.out.println("After Name 1 : " + name);
        name += " babo";
        System.out.println("After Name 2 : " + name);
    }
    public static void changeName(String s) {
        s += " babo";
    }
}
```

- 위 코드의 결과는 다음과 같다.
    
    ![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbLbDPw%2Fbtq3mvRIUT6%2FrwVJlk5cMY6GojlmPIzb8k%2Fimg.png)
    
    - 이론적으로 `String`은 Heap 영역에 있기 때문에 After Name 1의 결과는 kang babo가 나와야한다.
- `String`은 immutable한 클래스, 즉 **변경할 수 없는 불변의 객체**이기 때문에 **메서드가 실행될 때 원래 값이 아닌 복사한 값을 가리키기 때문이다.**
- `String` 외에도 immutable한 클래스는 `Boolean`, `Integer`, `Long`, `Double` … 등이 있다.
- mutable한 객체는 `List`, `ArrayList` 등의 컬렉션들이 대표적이다.
- 만약 문자열을 mutable하게 쓰고 싶다면 `StringBuilder`를 사용하면 된다.
    - `StringBuilder`의 `append` 메서드를 사용하여 값을 붙여주면 같은 데이터를 그대로 참조한다.

```java
public static void changeName(String s) {
	s += " babo";
}
```

- changeName 로직은 문자열 s에 `“ babo”`라는 문자열을 더하는 것처럼 보이지만 **실제로는 새로운 String을 생성하는 것이다.**
    
    ![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcpjifX%2Fbtq3nCitTZX%2Fou4avmHxnbRQ6OJMYfAOO1%2Fimg.png)
    
    - 위 코드에서 print한 내용은 name이기 때문에 `“kang”`이라는 결과가 나온 것이고, s는 메서드가 끝나면서 pop되며 사라진다.
- name에 직접 “ babo”를 붙이면 다음과 같다.
    
    ![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbIjq6f%2Fbtq3sucCtb7%2Fv6UjcYDymgGiOqMewEcD01%2Fimg.png)
    
- 아래처럼 연결되지 않은 객체들은 GC가 처리한다.
    
    ![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FccJEdJ%2Fbtq3stdHTjZ%2FUM85vPDD7KpVc06J0TFsmk%2Fimg.png)
    

## 참고 자료

- https://devkingdom.tistory.com/226