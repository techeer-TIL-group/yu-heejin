## @Modifying 어노테이션

- `@Query` 어노테이션(JPQL Query, Native Query)을 통해 작성된 INSERT, UPDATE, DELETE 쿼리에서 사용된다.
- `clearAutomatically`, `flushAutomatically` 속성을 변경할 수 있으며, 주로 bulk 연산과 같이 사용된다.
- INSERT, UPDATE, DELETE 문을 실행할 때 해당 어노테이션을 사용하지 않으면 오류가 발생한다.
- JPA Entity Life-cycle을 무시하고 쿼리가 실행되기 때문에 영속성 컨텍스트 관리에 중요하다.

## 예시 코드

```java
@Modifying(clearAutomatically = true)
@Query(
    value = "UPDATE ChatRoomMember " +
            "SET isExited = true " +
            "WHERE id.chatRoomId = :chatRoomId AND id.memberId = :memberId"
)
void deleteChatRoomMember(Long chatRoomId, Long memberId);
```

## 사용 시 주의 사항

- `@Query`로 정의된 bulk 연산 JPQL은 기존 JPA처럼 영속성 컨텍스트를 거쳐 쓰기 지연 SQL로 동작하는 것이 아닌 바로 Database에 질의한다.
- 변경 후 **데이터가 영속성 컨텍스트에 바로 반영되지 않고, `find` 시 기존 데이터가 출력되는 문제**가 발생한다.

### clearAutomatically

- `clearAutomatically` 옵션을 true로 설정하면, `**@Query`로 정의된 JPQL을 실행 후 자동으로 영속성 컨텍스트를 비워주는 옵션이다.**
- JPA에서는 영속성 컨텍스트에 있는 1차 캐시를 통해 엔티티를 캐싱하고, DB의 접근 횟수를 줄임으로써 성능을 개선한다.
    - 1차 캐시란 **영속성 컨텍스트에 있는 1차 캐시를 통해 엔티티를 캐싱하고 DB의 접근 횟수를 줄임으로써 성능을 개선**한다.
    - 1차 캐시는 `@Id` 값을 key 값으로 엔티티를 관리한다.
    - 따라서 `findById` 등을 통해 엔티티를 조회하면, `@Id` 값이 1차 캐시에 존재한다면 DB에 접근하지 않고 캐싱된 엔티티를 반환한다.
- bulk 연산의 경우 **1차 캐시를 포함한 영속성 컨텍스트를 무시하고 바로 쿼리를 실행하기 때문에 영속성 컨텍스트는 데이터 변경을 알 수 없어 DB와의 싱크가 맞지 않게된다.**
- 그러나 `clearAutomatically`를 `true`로 변경한다면, 벌크 연산 직후 자동으로 영속성 컨텍스트를 clear하기 때문에 1차 캐시 대신 DB 조회 쿼리를 실행하여 데이터 동기화 문제를 해결한다.

## 참고 자료

- https://joojimin.tistory.com/71
- https://frogand.tistory.com/174
- [https://hstory0208.tistory.com/entry/JPA-Modifying이란-그리고-주의할점-벌크-연산](https://hstory0208.tistory.com/entry/JPA-Modifying%EC%9D%B4%EB%9E%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%A3%BC%EC%9D%98%ED%95%A0%EC%A0%90-%EB%B2%8C%ED%81%AC-%EC%97%B0%EC%82%B0)
- https://devhyogeon.tistory.com/4