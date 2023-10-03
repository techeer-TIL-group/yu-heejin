## contains()

- 결과가 `contains`의 매개변수를 포함하고 있는지 검사한다.
- 중복 여부, 순서에 관계 없이 값만 일치하면 테스트가 성공한다.
- 결과에 포함되지 않은 값이 `contains`의 매개변수로 있으면 테스트는 실패한다.

## containsOnly()

- 원소의 순서, 중복 여부는 무시하지만 원소 값과 개수가 정확히 일치해야 테스트가 통과된다.

## containsExactly()

- 원소가 정확히 일치해야 한다.
- 중복된 값이 있어도 안되고, 순서가 달라져도 안된다.
- 특정 자료구조의 정확한 값을 테스트하고 싶은 경우 이 메소드를 사용한다.

## 참고 자료

- [https://velog.io/@woonyumnyum/우코테-JUnit-5와-AssertJ로-테스트코드-작성하기](https://velog.io/@woonyumnyum/%EC%9A%B0%EC%BD%94%ED%85%8C-JUnit-5%EC%99%80-AssertJ%EB%A1%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8%EC%BD%94%EB%93%9C-%EC%9E%91%EC%84%B1%ED%95%98%EA%B8%B0)