## deleteAll()

```java
@Override
@Transactional
public void deleteAll() {

	for (T element : findAll()) {
		delete(element);
	}
}
```

- sql 레벨업 책에서 보았던 **반복계 코드로 delete를 수행한다**.
    - 반복계 코드란 간단한 코드를 for문과 같은 반복문으로 여러번 수행하는 경우를 의미한다. ([https://github.com/yu-heejin/sql-levelup/blob/main/5장_반복문_유희진.md](https://github.com/yu-heejin/sql-levelup/blob/main/5%EC%9E%A5_%EB%B0%98%EB%B3%B5%EB%AC%B8_%EC%9C%A0%ED%9D%AC%EC%A7%84.md))
- `findAll()`에서 select 쿼리 한번, delete 쿼리를 N번씩 날리고 있었던 것!
    
    ```java
    DELETE
    FROM table
    WHERE id = ?
    ```
    
- 1 + N의 성능 문제가 발생할 수 있다.

## deleteAllById()

```java
@Override
@Transactional
public void deleteAllById(Iterable<? extends ID> ids) {

    Assert.notNull(ids, "Ids must not be null!");

    for (ID id : ids) {
        deleteById(id);
    }
}
```

- 마찬가지로 반복을 통해 삭제하는 것을 확인할 수 있다.

## deleteAllInBatch()

```java
@Override
@Transactional
public void deleteAllInBatch(Iterable<T> entities) {   // id 대신 entity를 받는다.

	Assert.notNull(entities, "Entities must not be null!");

	if (!entities.iterator().hasNext()) {
		return;
	}

	applyAndBind(getQueryString(DELETE_ALL_QUERY_STRING, entityInformation.getEntityName()), entities, em)
			.executeUpdate();
}

/*
	QueryUtils class
*/
public static final String COUNT_QUERY_STRING = "select count(%s) from %s x";
public static final String DELETE_ALL_QUERY_STRING = "delete from %s x";
public static final String DELETE_ALL_QUERY_BY_ID_STRING = "delete from %s x where %s in :ids";
```

- `DELETE_ALL_QUERY_STRING`의 `delete from %s x`의 쿼리로 데이터를 삭제한다.
    - 즉, `delete from table_name`으로 delete를 수행한다.
- **엔티티 이름을 기반**으로 해당하는 테이블을 찾아 한방에 데이터를 삭제한다.
- 따라서 만약 테이블에 있는 모든 데이터를 삭제하고자 한다면 `deleteAllInBatch()`를 사용하는 것이 좋다.

## @Query

- `deleteAllInBatch`를 쉽게 구현할 수 있다.

## 참고 자료

- https://blog.yevgnenll.me/posts/jpa-use-not-deleteall-but-deleteallinbatch
- https://unluckyjung.github.io/jpa/2022/06/19/JPA-deleteAll-vs-deleteAllInBatch/
- https://m.blog.naver.com/developer501/222504733153
- https://aljjabaegi.tistory.com/681
- [https://velog.io/@pooh6195/deleteAllById와-deleteAllByIdInBatch는-뭐가-다른가](https://velog.io/@pooh6195/deleteAllById%EC%99%80-deleteAllByIdInBatch%EB%8A%94-%EB%AD%90%EA%B0%80-%EB%8B%A4%EB%A5%B8%EA%B0%80)