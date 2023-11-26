## equals

- 어떤 두 참조 변수의 값이 같은지 동등 여부를 비교할 때 사용한다.
- 참조 변수를 비교할 땐 **일반적으로 해당 참조 값의 주소를 이용하여 비교한다.**
- `String`에서 `equals` 메소드로 비교할 땐 해당 문자열의 값을 비교하지만, 객체를 비교하는 경우 equals를 사용하더라도 객체의 주소값을 비교하기 때문에 `==` 이나 `equals`나 같은 결과가 출력된다.
    - 따라서 객체에서 객체의 필드값을 기준으로 비교하고자 한다면  `equals` 메소드를 오버라이딩해서 사용한다.
        
        ```java
        import java.util.Objects;
        
        class Person {
            String name;
        
            public Person(String name) {
                this.name = name;
            }
        
            // 객체 주소 비교가 아닌 Person 객체의 사람 이름이 동등한지 비교로 재정의 하기 위해 오버라이딩
        		@Override
            public boolean equals(Object o) {
                if (this == o) return true; // 만일 현 객체 this와 매개변수 객체가 같을 경우 true
                if (!(o instanceof Person)) return false; // 만일 매개변수 객체가 Person 타입과 호환되지 않으면 false
                Person person = (Person) o; // 만일 매개변수 객체가 Person 타입과 호환된다면 다운캐스팅(down casting) 진행
                return Objects.equals(this.name, person.name); // this객체 이름과 매개변수 객체 이름이 같을경우 true, 다를 경우 false
            }
        }
        
        public class Main {
            public static void main(String[] args) {
                Person p1 = new Person("홍길동");
                Person p2 = new Person("홍길동"); // 동명이인
        
                System.out.println(p1.equals(p2)); // true
            }
        }
        ```
        

### String 클래스가 정확히 값을 비교하는 이유?

- 문자열을 저장하는 String 클래스 역시 참조 값을 기준으로 동등 비교하기 때문에 주소 값을 기준으로 비교해야한다.
- 하지만 String 클래스의 경우 이미 `equals` 메서드가 재정의되어 있기 때문에 제대로 값을 비교할 수 있는 것이다.
    
    ```java
    /**
         * Compares this string to the specified object.  The result is {@code
         * true} if and only if the argument is not {@code null} and is a {@code
         * String} object that represents the same sequence of characters as this
         * object.
         *
         * <p>For finer-grained String comparison, refer to
         * {@link java.text.Collator}.
         *
         * @param  anObject
         *         The object to compare this {@code String} against
         *
         * @return  {@code true} if the given object represents a {@code String}
         *          equivalent to this string, {@code false} otherwise
         *
         * @see  #compareTo(String)
         * @see  #equalsIgnoreCase(String)
         */
        public boolean equals(Object anObject) {
            if (this == anObject) {
                return true;
            }
            return (anObject instanceof String aString)
                    && (!COMPACT_STRINGS || this.coder == aString.coder)
                    && StringLatin1.equals(value, aString.value);
        }
    ```
    

## hashCode

- `hashCode` 메서드는 객체의 주소 값을 이용해서 해싱(hashing) 기법을 통해 해시 코드를 만든 후 반환하기 때문에 **서로 다른 두 객체는 같은 해시 코드를 가질 수 없다.**
    - `Object` 클래스의 `hashCode` 메서드는 **객체의 고유한 주소 값을 `int`값으로 변환하기 때문에 객체마다 다른 값을 리턴한다.**
- 해시 코드는 주소 값은 아니며, 주소 값으로 만든 고유한 숫자값이라고 할 수 있다.
    
    ```java
    class Person {
        String name;
    
        public Person(String name) {
            this.name = name;
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            Person p1 = new Person("홍길동");
            Person p2 = new Person("홍길동");
    
            // 객체 인스턴스마다 각기 다른 주해시코드(주소))를 가지고 있다.
            System.out.println(p1.hashCode()); // 622488023
            System.out.println(p2.hashCode()); // 1933863327
        }
    }
    ```
    

```java
/**
     * {@return a hash code value for this object} This method is
     * supported for the benefit of hash tables such as those provided by
     * {@link java.util.HashMap}.
     * <p>
     * The general contract of {@code hashCode} is:
     * <ul>
     * <li>Whenever it is invoked on the same object more than once during
     *     an execution of a Java application, the {@code hashCode} method
     *     must consistently return the same integer, provided no information
     *     used in {@code equals} comparisons on the object is modified.
     *     This integer need not remain consistent from one execution of an
     *     application to another execution of the same application.
     * <li>If two objects are equal according to the {@link
     *     #equals(Object) equals} method, then calling the {@code
     *     hashCode} method on each of the two objects must produce the
     *     same integer result.
     * <li>It is <em>not</em> required that if two objects are unequal
     *     according to the {@link #equals(Object) equals} method, then
     *     calling the {@code hashCode} method on each of the two objects
     *     must produce distinct integer results.  However, the programmer
     *     should be aware that producing distinct integer results for
     *     unequal objects may improve the performance of hash tables.
     * </ul>
     *
     * @implSpec
     * As far as is reasonably practical, the {@code hashCode} method defined
     * by class {@code Object} returns distinct integers for distinct objects.
     *
     * @apiNote
     * The {@link java.util.Objects#hash(Object...) hash} and {@link
     * java.util.Objects#hashCode(Object) hashCode} methods of {@link
     * java.util.Objects} can be used to help construct simple hash codes.
     *
     * @see     java.lang.Object#equals(java.lang.Object)
     * @see     java.lang.System#identityHashCode
     */
    @IntrinsicCandidate
    public native int hashCode();
```

- `Object` 클래스에 정의된 `hashCode()`는 위과 같다.
- `native` 키워드는 OS가 가지고 있는 메소드임을 의미한다.
    - `native` 키워드가 있는 코드는 JVM의 JNI가 JVM에 적재시키고 실행한다.
    - 해당 네이티브 메서드는 **OS에 C언어로 작성되어 있어 내용은 볼 수 없고 사용만 가능하다.**

## equals와 hashCode 오버라이딩

- 일반적으로 객체의 주소 값이 아닌 필드 값을 비교하기 위해 `equals`를 오버라이딩 했다면, `hashCode`도 함께 오버라이딩 해야한다.
    - `equals()`의 결과가 `true`인 두 객체의 해시코드는 **반드시 같아야한다는 자바의 규칙 때문이다.**
- 또한, hash값을 사용하는 Collection(HashMap, HashSet, HashTable)의 경우, 객체가 논리적으로 같은지 비교할 때 아래와 같은 과정을 거친다.
    
    ![https://tecoble.techcourse.co.kr/post/2020-07-29-equals-and-hashCode/](https://tecoble.techcourse.co.kr/static/c248e8d79140c18ed9895d1c95dd7ad0/54e75/2020-07-29-equals-and-hashcode.png)
    
    https://tecoble.techcourse.co.kr/post/2020-07-29-equals-and-hashCode/
    
    - `hashCode` 메서드의 리턴 값이 우선 일치하고, `equals` 메서드의 리턴 값이 `true`여야 논리적으로 같은 객체라고 판단한다.
    - 만약 재정의를 하지 않는다면, `Object` 클래스의 `hashCode` 메서드는 객체의 고유한 주소값을 int로 변환하기 때문에, **객체마다 다른 값을 리턴하여 필드 값이 같아도 다른 객체로 인식하게 된다.**

## equals, hashCode 오버라이딩 예시

```java
public class Car {
    private final String name;

    public Car(String name) {
        this.name = name;
    }

    // intellij Generate 기능 사용
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Car car = (Car) o;
        return Objects.equals(name, car.name);
    }

    **@Override
    public int hashCode() {
        return Objects.hash(name);
    }**
}

class Person {
    public String name;

    public Person(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person p = (Person) o;
        return Objects.equals(name, p.name);
    }

    **@Override
    public int hashCode() {
        return Objects.hash(name); // name 필드의 해시코드를 반환한다.
    }**
}
```

- 재정의 시 반환되는 해시 코드 값을 객체의 주소값이 아닌 **비교하고자하는 필드 값으로 해시코드를 반환하도록 재정의하면 된다.**
- `Objects.hash` 메서드는 `hashCode` 메서드를 재정의하기 위해 간편히 사용할 수 있는 메서드지만, 속도가 느린 편이다.
    - 만약 성능에 민감한 경우라면 직접 재정의하는 것이 좋다.

## 참고 자료

- [https://inpa.tistory.com/entry/JAVA-☕-equals-hashCode-메서드-개념-활용-파헤치기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-equals-hashCode-%EB%A9%94%EC%84%9C%EB%93%9C-%EA%B0%9C%EB%85%90-%ED%99%9C%EC%9A%A9-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0)
- https://tecoble.techcourse.co.kr/post/2020-07-29-equals-and-hashCode/