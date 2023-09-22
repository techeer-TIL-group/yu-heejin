## Kotlin에서 늦은 초기화하기

- 예를 들어 변수 a를 정의할 때 첫 상태를 정의하기 어렵다고 가정하면 다음과 같이 선언할 수 있다.
    
    ```kotlin
    var a: String? = null
    ```
    
- 하지만 썩 좋은 방법은 아니다.
- 따라서 `lateinit`과 `lazy`를 사용할 수 있다.

## lateinit

```kotlin
fun main() {
	lateinit var name: String

	val text = "hello"
	name = "My name is $name"
}
```

- `lateinit`을 사용하면 어떤 동작 결과 값을 기반으로 값을 나중에 초기화할 수 있다.
- 단, `lateinit`을 사용할 경우 계속 값이 변경될 수 있다는 속성인 `var`만 사용해야하며, Primitive Type(Int, Float, Double…)에는 사용할 수 없다.

## by lazy

```kotlin
fun main() {
    lateinit var text: String
    val textLength: Int by lazy {
        text.length
    }

    // 대충 중간에 뭔가 했음
    text = "H43RO_Velog"
    println(textLength)
}
```

- `lateinit`으로 아직 초기화되지 않은 `text`의 속성을 `by lazy`안에서 사용할 수 있다.
- `by lazy`는 선언 당시 초기화할 방도가 없지만, **이후 의존하는 값들이 초기화된 이후에 값을 채워넣고 싶을 때 사용한다.**
    - 즉, **호출시에 이를 어떻게 초기화해줄지에 대해 정의할 수 있는 구문**이다.
- 위 코드에서는 **text가 초기화된 후 textLength를 출력할 때, textLength에 text.length 속성값을 저장하여 초기화**한다.
- `val`을 사용하는데, **단 한번의 초기화가 이루어지고 이후 값이 불변함을 보장하기 때문이다.**

## 참고 자료

- [https://velog.io/@haero_kim/Kotlin-lateinit-vs-lazy-정확히-아세요](https://velog.io/@haero_kim/Kotlin-lateinit-vs-lazy-%EC%A0%95%ED%99%95%ED%9E%88-%EC%95%84%EC%84%B8%EC%9A%94)