## CTE(Common Table Expression)

```sql
WITH [RECURSIVE] TABLE명 AS (
    SELECT - # 비반복문. 무조건 필수
    [UNION ALL] # RECURSIVE 사용 시 필수. 다음에 이어붙어야 할 때 사용
    SELECT - 
    [WHERE -] # RECURSIVE 사용 시 필수. 정지 조건 필요할 때 사용
)
```

- 기존의 뷰나 파생 테이블, 임시 테이블 등으로 사용되는 것들을 대신할 수 있으며, 더 간결하게 표현할 수 있다.
- 데이터의 **임시 결과 집합**
- CTE를 표현하는 구문으로 `WITH`절이 있다.
- 메모리 상에 가상 테이블을 저장할 때 사용된다.

## WITH(비재귀)

```sql
WITH 가상테이블명 AS
(
    SELECT 쿼리
    UNION ALL -- 뭐 붙이거나 할 경우 추가
    SELECT 쿼리
)
```

- 오라클처럼 MySQL에서도 `WITH`문으로 가상 테이블을 만들 수 있다.

## WITH RECURSIVE(재귀)

```sql
WITH RECURSIVE 가상테이블명 [(column1, ...)] AS (
   -- 초기값 (처음 호출하는 쿼리)
   -- non-recursive query
   SELECT [(column1, ...)]
   UNION [ALL]
   -- recursive query (반복 쿼리)
   SELECT [(column1, ...)] FROM 가상테이블명 [WHERE]
)

WITH RECURSIVE mine_record AS (
   SELECT 1 AS num
   UNION ALL
   SELECT num + 1 FROM mine_record WHERE num < 10
)
```

- `WITH RECURSIVE` 구문을 사용하면 반복문처럼 사용할 수 있다.
- 재귀적 쿼리라고도 하며, 보통 테이블 데이터가 계층형일 때 많이 사용한다.
- 해당 구문을 통해 쿼리가 반복되며, 반복된 결과를 parent query 영역에서 `FROM` 절로 가져와 사용하는 구조이다.
- `UNION` 다음에 사용되는 재귀 쿼리문에서는 보통 `WHERE` 조건을 통해 반복을 멈추도록 제한한다.

## VIEW vs CTE

- WITH는 쿼리문이 실행될 때 임시로 사용하는 테이블이고, View는 데이터셋에 저장해 여러 사람이 활용할 수 있다.

### VIEW

- VIEW는 **질의의 결과를 저장하는 것이 아니라 질의 그 자체를 저장**한다.
    - 물리적 객체로 디스크에 저장되지만 쿼리만 저장하고 쿼리에서 반환된 데이터는 저장하지 않는다.
- 다른 **쿼리문에서 호출 시마다 실행**된다.
    - 동일한 뷰를 다시 정의하지 않고도 다른 쿼리에서 사용할 수 있다.
- 쿼리문의 alias

### Common Table Expression

- 해당 쿼리문 내에서만 존재하는 일시적인 테이블(결과의 집합)
- 메모리에서만 존재하며 `WITH`문 사용시에만 연산된다.
    - 쿼리가 실행되는 동안에만 메모리에 존재하며, 해당 쿼리문이 끝나면 버려진다.
- `inline view`라고 부르기도 한다.
- 재귀가 가능해 트리 구조 관련 사용시 유용하다.

### VIEW, CTE 사용 예시

1. 가끔 참조되는 쿼리는 일반적으로 CTE를 사용한다.
    1. 쿼리가 다시 필요한 경우 CTE를 복사하고 필요한 경우 수정하면 된다.
2. 동일한 쿼리를 자주 참조하는 경우 VIEW를 만드는 것이 좋다.
3. VIEW는 특정 사용자의 데이터베이스 엑세스를 제한함과 동시에 필요한 정보를 얻을 때 사용한다.
    1. 만약 볼 수 있는 데이터와 볼 수 없는 데이터를 나누고자할 경우 뷰를 사용하는 것이 좋다.

## 참고 자료

- https://wakestand.tistory.com/455
- https://mine-it-record.tistory.com/447
- https://horang98.tistory.com/10
- https://velog.io/@sangmin7648/SQL-View-vs-CTE
- https://learnsql.com/blog/difference-between-sql-cte-and-view/