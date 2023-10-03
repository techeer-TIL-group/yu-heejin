## Comparable 인터페이스

- `Comparable`은 객체가 **임의의 기준으로 정렬될 수 있도록 만들고 싶을 때** 사용된다.
- 객체를 정렬 가능하도록 만들기 위해 사용되는 인터페이스이다.
- `Comparable` 인터페이스를 구현하는 클래스는 `compareTo` 메소드를 오버라이딩 해야한다.

### compareTo(T o)

- 인자로 객체를 받는다.
- 반환값은 다음과 같고, 정렬 기준은 오름차순이다.
    - 현재 객체가 인자로 받아온 객체보다 앞이라면 양수를 반환한다.
    - 현재 객체가 인자로 받아오는 객체보다 뒤라면 음수를 반환한다.
    - 두 객체가 같으면 0을 반환한다.

## Comparator 인터페이스

- `Comparable`이 객체 자신이 정렬 가능하도록 구현하는데 목적이 있다면, `Comparator`는 **타입이 같은 객체 두 개를 매개변수로 전달받아 우선순위를 비교할 수 있는 정렬자를 만드는데 목적**이 있다.
- `Comparator`를 구현하기 위해서는 `compare(T o1, T o2)` 메소드를 오버라이딩하면 된다.

### compare(T o1, T o2)

- o1이 o2보다 앞에 온다면 양수를 반환한다.
- o1이 o2보다 뒤에 온다면 음수를 반환한다.
- 두 객체가 같다면 0을 반환한다.

## Comparator를 사용하는 이유?

> 객체 자기 자신이 우선순위를 계산할 수 있는 `Comparable` 인터페이스가 존재하는데 왜 `Comparator`를 쓰는걸까?
> 
- 첫번째 이유는 **정렬 대상 클래스의 코드를 수정할 수 없는 경우**에 사용한다.
- 두번째는 정렬 대상 클래스에 이미 정의된 `compareTo`와 **다른 기준으로 정렬하고 싶을 경우 사용**한다.
- 마지막 이유로는 **여러 정렬 기준을 적용하고 싶을 경우 사용**한다.

## Comparator 활용편

- 두 객체를 비교하기 위해 객체를 별도로 만들 필요 없이 익명 객체를 활용해 `Comparator` 기능만 따로 둘 수도 있다.
    
    ```java
    // 익명 객체 구현방법 1
    Comparator<Student> comp1 = new Comparator<Student>() {
    	@Override
    	public int compare(Student o1, Student o2) {
    		return o1.classNumber - o2.classNumber;
    	}
    };
    
    // 익명 객체 구현 2
    public static Comparator<Student> comp2 = new Comparator<Student>() {
    	@Override
    	public int compare(Student o1, Student o2) {
    		return o1.classNumber - o2.classNumber;
    	}
    };
    ```
    
- 익명 객체를 사용하기 때문에 다양한 비교 기준을 생성할 수 있다.
- `Comparable`은 **‘자기 자신’과 하나의 매개변수를 비교하기 때문에 익명 객체로 만들기 복잡해진다.**

## 두 인터페이스의 공통점과 차이점

- 두 인터페이스 모두 **객체를 비교**할 수 있도록 만든다.
    - 본질적으로 객체는 사용자가 기준을 정해주지 않는 이상 어떤 객체가 더 높은 우선순위를 갖는지 판단할 수 없기 때문에 이러한 인터페이스를 사용한다.
- `Comparable`은 자기 자신과 매개변수 객체를 비교하고, `Comparator`는 두 매개변수 객체를 비교한다.
    - `Comparable`은 **자기 자신과 파라미터로 들어오는 객체**를 비교하는 것이고, `Comparator`는 자기 자신의 상태가 어떻던 상관없이 **파라미터로 들어오는 두 객체를 비교**한다.
- `Comparable`은 `lang` 패키지에 있기 때문에 `import`를 해줄 필요가 없지만, `Comparator`는 `util` 패키지에 있다.

## 참고 자료

- https://hudi.blog/java-comparable-and-comparator/
- https://st-lab.tistory.com/243