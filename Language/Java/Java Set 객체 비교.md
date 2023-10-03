타입스크립트는 객체인 경우 set에 중복값이 들어가는 경우가 있었다. 자바도 그런가 싶어서 알아봤다!

## 객체 타입 처리 시 Set

- 객체 타입을 Set에 추가할 경우 같은 속성값을 가지고 있는 객체더라도 다른 객체로 인식한다.
- **객체를 비교해서 Set에 추가할 때 객체의 속성으로 비교하는 것이 아닌 객체의 주소값을 기준으로 비교**하기 때문에 다음 객체들은 각각 다른 객체로 인식되어 Set에 중복으로 저장되는 것이다.

## 사용 예시

```java
import java.util.HashSet;
import java.util.Set;
 
public class Main {
 
    static class Person {
        private String name;
        private int age;
 
        public Person(String name, int age) {
            this.name = name;
            this.age = age;
        }
    }
 
    public static void main(String[] args) throws IOException {
        Person p1 = new Person("홍길동", 20);
        Person p2 = new Person("홍길동", 20);
        
        Set<Person> set = new HashSet<>();
        set.add(p1);
        set.add(p2);
        
        for(Person p : set) {
            System.out.println(p.name + " " + p.age);
        }
    }
}

// 실행 결과
홍길동 20
홍길동 20
```

## 객체를 비교하기 위해서는?

```java
class Person{
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @Override
    public int hashCode() {
        return (this.name+this.age).hashCode();
    }
    
    @Override
    public boolean equals(Object obj) {
        //p1.equals(p2)
        if(obj instanceof Person) {
            Person p = (Person)obj;
            return this.hashCode()==p.hashCode(); 
            
        }
        return false;
    }
}
```

- 객체의 속성값으로 비교해서 set을 처리하려면 다음을 구현해야 한다.
- String도 같은 원리로 동작한다.

### hashCode()

- 원래 객체를 비교할 때 객체가 저장된 주소값을 비교하게 되는데, 이를 주소값 대신 문자열로 비교한다.

### equals(Object object)

- 클래스 내의 멤버변수 값을 비교하도록 멤버 변수 데이터를 문자열 결합 (+)을 통해 해쉬코드 값을 비교한다.

## 참고 자료

- [https://4ngeunlee.tistory.com/320](https://4ngeunlee.tistory.com/320)
- [https://haruhiism.tistory.com/160](https://haruhiism.tistory.com/160)