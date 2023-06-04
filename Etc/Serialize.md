![Untitled](https://hudi.blog/static/a7031b2a5f1cccef251b22eeb751c365/7e5f2/serialize-deserialize.png)

- Serial은 ‘연속된’이라는 뜻을 가지고 있다.
- Serialization은 객체를 ‘연속된’ Byte나 String으로 변환하는 과정을 의미한다.
- Object는 메모리에 올라와 있지만, Byte나 String은 파일로 저장될 수 있거나 통신에 사용될 수 있다.
- 즉, **직렬화는 객체를 파일의 형태 등으로 저장하거나, 통신하기 쉬운 포맷으로 변환하는 과정을 의미한다.**
    - 특정 포맷으로 직렬화된 데이터는 역직렬화라는 과정을 통해 다시 객체로 변환될 수 있다.
- 일반적으로 자바에서 직렬화라고 이야기하는 것은 객체를 Byte로 변환하는 과정을 의미한다.
    - 하지만 Byte외에도 표 형태로 직렬화할 땐 CSV, 구조적인 데이터로 직렬화 할 땐 JSON, XML 등 다양한 포맷으로 직렬화할 수 있다.
    - 특히, 구조적인 데이터를 사용할 땐 대부분 JSON 포맷을 사용한다.

## 직렬화/역직렬화

- 직렬화: 객체에 저장된 데이터를 I/O 스트림에 쓰기 위해 연속적인 데이터로 변환하는 것
- 역직렬화: I/O 스트림에서 데이터를 읽어서 객체를 만드는 것

## 자바의 직렬화

- 자바는 자바 시스템끼리의 객체 교환을 위해 Serializable 인터페이스를 구현하는 객체를 바이트 스트림으로 직렬화/역직렬화하는 기능을 제공한다.
- 하지만 이팩티브 자바에서는 자바에서 제공하는 직렬화하는 기능을 사용하지 않을 것을 강력히 권장한다.
    - 자바에서 제공하는 직렬화는 여러가지 문제가 있기 때문에, 그 대안으로 JSON 등의 포맷을 추천한다.

## JSON 직렬화/역직렬화 방법

1. `JSON.stringify()`
    - **객체 → 문자열**로 변환하며, 이를 **직렬화(serialize)**라고 한다.
    - 통신할 땐 문자열로 직렬화하여 주고받는 것이 안전하다.
2. `JSON.parse()`
    - **문자열 → 객체**로 변환하며, 이를 **역직렬화(deserialize)**라고 한다.
    - 통신으로 받은 문자열은 객체로 역직렬화하여 사용하는 것이 편리하다.

## 참고 자료

- https://hudi.blog/serialization/
- https://curryyou.tistory.com/359
- [https://atoz-develop.tistory.com/entry/JAVA의-객체-직렬화Serialization와-JSON-직렬화](https://atoz-develop.tistory.com/entry/JAVA%EC%9D%98-%EA%B0%9D%EC%B2%B4-%EC%A7%81%EB%A0%AC%ED%99%94Serialization%EC%99%80-JSON-%EC%A7%81%EB%A0%AC%ED%99%94)
- https://kcode-recording.tistory.com/74
- [https://velog.io/@youngminss/WEB-직렬화-역직렬화-JS](https://velog.io/@youngminss/WEB-%EC%A7%81%EB%A0%AC%ED%99%94-%EC%97%AD%EC%A7%81%EB%A0%AC%ED%99%94-JS)