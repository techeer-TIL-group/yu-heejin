## 코틀린의 data class

- 데이터 보관을 목적으로 사용하는 클래스
- 클래스가 Data를 보유하면서 특별한 기능을 하지 않는다.

### Java에서 POJO(Plain Old Java Object) 클래스

```java
public class Person {
    String name;
    int age;
    String gender;
    
    public Person(String name, int age, String gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

		public String getName() {
			...
		}

		public int getAge() {
			...
		}
		
		// 기타 getter, setter 등 존재
}
```

- 별도의 비즈니스 로직은 없지만 `getter`, `setter`, `toString`과 같은 보일러플레이트 코드가 필요하다.

### Kotlin에서 data class

```kotlin
data class Person(
	var name: String, 
	var age: Int, 
	var gender: String
)
```

- Kotlin의 data 클래스는 생성자부터 `getter`, `setter`, `canonical methods` 등의 문법을 자동으로 생성해준다.
- canonical Methods
    - equals()
    - hashCode()
    - toString()

## data class의 특징

1. 상속받을 수 없다.
2. 기본 생성자에는 최소 하나의 파라미터가 있어야한다.
3. 기본 생성자의 파라미터는 val이나 var이어야한다.
4. 데이터 클래스는 `abstract`, `open`, `sealed`, `inner`가 되면 안된다.

## 일반 클래스와의 차이점

- println()을 했을 때 데이터 클래스는 입력한 정보가 나온다. (toString())
- 하지만 일반 클래스는 주소값이 나온다.

## data class의 copy() 함수

```kotlin
val james = Person("James", 20, "male")
val tom = james.copy(name = "Tom")  // 이름만 Tom으로 바꾸고 나이와 성별은 james에서 복사
```

- 데이터 클래스를 복사하는 경우 사용
- 코틀린 컴파일러가 데이터 클래스의 인스턴스를 더 쉽게 불변 객체로 활용할 수 있도록 메소드를 제공한다.
- 코틀린의 데이터 클래스는 `copy()` 메소드를 사용해 원하는 파라미터를 오버라이딩해서 데이터 클래스의 새로운 인스턴스를 생성할 수 있게 한다.
    - 즉, 객체를 복사하면서 일부 프로퍼티를 바꿀 수 있게 해준다.
- **깊은 복사에 해당하는 copy**

## Destructuring

```kotlin
val james: Person = Person("James", 20, "male")
val (name, age, gender) = james

// 필요하지 않은 경우 언더스코어 사용
val (name, _, gender) = james
```

- Javascript의 구조분해 할당 문법과 유사하다.

## 참고 자료

- [https://velog.io/@jonmad/Til.-kotlin-Data-class와-보일러-플레이트](https://velog.io/@jonmad/Til.-kotlin-Data-class%EC%99%80-%EB%B3%B4%EC%9D%BC%EB%9F%AC-%ED%94%8C%EB%A0%88%EC%9D%B4%ED%8A%B8)
- https://readystory.tistory.com/85
- [https://velog.io/@jonmad/Til.-kotlin-Data-class와-보일러-플레이트](https://velog.io/@jonmad/Til.-kotlin-Data-class%EC%99%80-%EB%B3%B4%EC%9D%BC%EB%9F%AC-%ED%94%8C%EB%A0%88%EC%9D%B4%ED%8A%B8)