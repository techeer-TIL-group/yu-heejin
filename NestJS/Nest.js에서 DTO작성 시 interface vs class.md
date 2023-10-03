## DTO(Data Transfer Object)

- DTO는 **계층간 데이터 교환을 위해 사용하는 객체**이다.
- 계층간 데이터를 주고받을 때 어떤 모양의 데이터 객체로 주고 받을지를 DTO에서 결정하게 된다.
- 쉽게 말해 정보를 교환하는데 있어서 타입을 체크하기 위한 구조이다.

## Interface

- 두 시스템 사이 상호간에 정의한 약속 혹은 규칙을 포괄하여 의미한다. (객체의 껍데기 또는 설계도)
    - 커스텀 타입으로 사용된다.
- **Typescript에서 변수, 함수, 클래스의 타입 체크**를 위해 사용된다.
- 클래스와 유사하지만 인스턴스 생성이 불가능하고 모든 메소드는 추상 메소드로 이루어져 있다.
- **ES6에서 지원하지 않고 Typescript에서만 지원한다.**
    - 인터페이스는 타입이며, 컴파일 후에 사라진다.
- 데이터 타입을 체크하기 위한 용도로는 DTO와 유사하나 ES6에서는 지원하지 않는다.

## 클래스와 인터페이스 차이(Typescript)

- 클래스와 달리 interface는 Typescript의 컨텍스트 내에서만 존재하는 가상 구조이다.
- Typescript 컴파일러는 타입 체크 목적으로만 인터페이스를 사용한다.
- 코드가 자바스크립트 언어로 트랜스 파일되면 인터페이스에서 제거된다.

## class DTO를 추천하는 이유

- NestJS 공식 문서에서는 DTO를 사용할 것을 권장한다.
- **Typescript의 클래스는 Javascript의 ES6 표준을 따르기 때문에 컴파일된 Javascript에서 실제 엔티티로 보존되지만, 컴파일된 Javascript에서 인터페이스는 컴파일 중 제거된다.**
    - 인터페이스는 ES6 표준이 아니다.
    - 따라서 **NestJS에서 런타임에 인터페이스를 참조할 수 없다.**
- 따라서 **NestJS에서는 런타임 이후 Input 값에 대한 유효성 검증이나 그 외의 데이터 타입을 추적해야하는 경우 DTO를 추천**하고 있다.
    - DTO는 클래스 형태이기 때문에 ES6 표준이라 참조가 가능하기 때문이다.

## 참고 자료

- https://blog.jh8459.com/2022-06-24-TIL/
- [https://www.inflearn.com/questions/526436/dto-를-interface가-아닌-class로-해주는-이유가-있나요](https://www.inflearn.com/questions/526436/dto-%EB%A5%BC-interface%EA%B0%80-%EC%95%84%EB%8B%8C-class%EB%A1%9C-%ED%95%B4%EC%A3%BC%EB%8A%94-%EC%9D%B4%EC%9C%A0%EA%B0%80-%EC%9E%88%EB%82%98%EC%9A%94)
- [https://velog.io/@kysung95/짤막글-Interface와-Class의-차이점](https://velog.io/@kysung95/%EC%A7%A4%EB%A7%89%EA%B8%80-Interface%EC%99%80-Class%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)