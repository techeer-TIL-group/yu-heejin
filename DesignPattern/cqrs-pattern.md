## Command와 Query

### Command

- `Command`는 시스템에 어떠한 변경을 가하는 행위이다.
- `Command`성 함수는 **시스템의 상태를 변경시키는 대신 값을 반환하지 않아야한다.**

### Query

- 시스템의 상태를 관찰할 수 있는 행위
- `Query`성 함수는 **시스템의 상태를 단순히 반환하기만 하고 상태를 변경시키지 않아야한다.**

## CQS(Command Query Separation)

- 어떠한 함수가 있다면 그 함수는 `Command` 또는 `Query` 중 하나의 역할만 수행해야한다.
- 만약 하나의 함수에서 `Command`와 `Query`가 모두 동시에 일어나게 된다면, 이는 소프트웨어의 3가지 원칙 중 복잡하지 않아야한다는 KISS가 지켜지지 않을 것이다.
- 코드 레벨에서의 분리를 의미한다.

## CQRS(Command and Query Responsibility Segregation)

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FHA7Ma%2FbtrPjG5Yh4C%2FwNFBqHZMBV11llAHtKC7TK%2Fimg.png)

- CQS에 비해 조금 더 큰 레벨에서의 `Command` 모듈과 `Query` 모듈을 분리하자는 개념
- 명령과 조회의 책임 분리
- 시스템에서 **명령을 처리하는 책임과 조회를 처리하는 책임을 분리**한다.
    - 명령(Command)은 시스템의 상태를 변경하는 작업을 의미하며, 조회(Query)는 시스템의 상태를 반환하는 작업을 의미한다.
- 시스템의 상태를 변경하는 작업과 상태를 반환하는 작업의 책임을 분리하는 것
    - 시스템 데이터를 변경하는 코드와 시스템 데이터를 조회하는 코드를 따로 만드는 것
- 구현 방식이나 시스템 규모에 따라 DB를 나누기도 하고 프로세스를 나누기도 한다.

## CQRS 패턴이 필요한 경우

### 복잡성 문제

- CUD에 비해 Query는 단순 데이터 조회이기 때문에 많은 요구사항들이 필요하지 않다.

### 성능

- 대부분의 write 연산에서 일관성에 대해 많은 신경을 써야한다.
- 대부분 잘 알려진 consistency를 지키기 위한 해법으로 DB Locking 기법을 사용한다.
    - write 연산에서 한번 lock을 잡게 되면 그 뒤에 read 연산이 모두 대기하기 때문에 전반적인 성능이 낮아질 수 있다.

### 확장성

- 대부분 쓰기 작업과 읽기 작업의 비율은 1:1000 정도이다.
    - 즉, read side와 write side의 서버는 서로 다른 기준으로 설계되어야 한다.
    - 독립적으로 확장 가능해야하고 각각 목적에 맞는 다른 솔루션이 필요하다.
- 만약 read와 write가 분리되지 않고 하나의 컴퓨팅 엔진만을 사용한다면 독립적인 확장이 어려울 수 있다.

## 참고 자료

- https://mslim8803.tistory.com/73
- https://freedeveloper.tistory.com/399
- https://wonit.tistory.com/628