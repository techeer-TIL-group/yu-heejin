## Bulk Insert

```sql
INSERT INTO table1 (col1, col2) VALUES (val11, val12);
INSERT INTO table1 (col1, col2) VALUES (val21, val22);
INSERT INTO table1 (col1, col2) VALUES (val31, val32);

INSERT INTO table1 (col1, col2) VALUES
(val11, val12),
(val21, val22),
(val31, val32);
```

- 위 단일 쿼리 3개를 아래처럼 하나로 묶어서 처리하는 방식

## JPA에서 save, saveAll 메서드

- **JPA Entity ID 전략이 `IDENTITY`인 경우 `saveAll`과 `save`가 bulk insert로 처리되지 않는다.**
- `save`, `saveAll` 모두 내부적으로 동작하는 원리는 같다.
    
    ```java
    @Repository
    @Transactional(readOnly = true)
    public class SimpleJpaRepository<T, ID> implements JpaRepositoryImplementation<T, ID> {
      // 기타 메서드 생략
    
    	@Transactional
    	@Override
    	public <S extends T> List<S> saveAll(Iterable<S> entities) {
    
    		Assert.notNull(entities, "Entities must not be null");
    
    		List<S> result = new ArrayList<>();
    
    		for (S entity : entities) {
    			result.add(save(entity));  // N개의 Entity를 save
    		}
    
    		return result;
    	}
    
    	@Transactional
    	@Override
    	public <S extends T> S save(S entity) {
    
    		Assert.notNull(entity, "Entity must not be null");
    
    		if (entityInformation.isNew(entity)) {
    			em.persist(entity);
    			return entity;
    		} else {
    			return em.merge(entity);
    		}
    	}
    }
    ```
    
    - `saveAll`에 List 형태의 entity를 넣으면 반복문을 돌며 `save`를 한 번씩 호출한다.
    - `saveAll`을 호출해도 내부적으로는 `save` 메서드를 순차적으로 호출하도록 구현되어 있다.
    - 결국 **한 트랜잭션 안에서 반복문을 돌며 100개의 `save`를 호출하든, 100개의 entity list를 `saveAll`로 1회 호출하든 결과는 같다.**

## IDENTITY 전략으로 Batch INSERT가 불가능한 이유

- JPA + Batch Insert를 사용하려면 ID 전략을 Table 전략으로 수정해야한다.
- 그러나 **MySQL은 Sequence 전략이 없다.**
- 일반적으로 MySQL에서 사용하는 IDENTITY 전략은 auto-increment로 PK 값을 자동으로 증분해서 생성한다.
    - 따라서 Insert를 실행하기 전까지는 ID에 할당된 값을 알 수 없기 떄문에 Transaction Write Behind를 할 수 없고 결과적으로 Batch Insert를 진행할 수 없다.
- Entity를 persist하려면 `@Id`로 지정한 필드에 값이 필요한데, **IDENTITY 타입은 실제 DB에 insert를 해야만 값을 얻을 수 있기 때문에 batch처리가 불가능하다.**

## Bulk Insert 구현하기

### 0. application.yml 옵션 지정하기

```yaml
spring:
    datasource:
        url: jdbc:mysql://localhost:3306/db명?rewriteBatchedStatements=true
    jpa:
        properties:
            hibernate:
                ## bulk insert 옵션 ##
                # 정렬 옵션
                order_inserts: true
                order_updates: true
                # 한번에 나가는 배치 개수 -> 100개의 insert를 1개로 보낸다.
                jdbc:
                    batch_size: 100
```

- `rewriteBatchedStatements=true`로 설정하지 않으면 batch insert가 실행되지 않는다.
- `order_inserts`, `order_updates` 옵션은 insert하는 것과 update하는 것의 순서를 말한다.
- 예를 들어, 트랜잭션에 부모 엔티티를 save한 후 자식 엔티티를 save하는 순서가 있다고 가정하면 아래와 같은 쿼리 결과가 생성된다.
    
    ```sql
    insert into 부모 value a1
    insert into 자식 value b1
    insert into 부모 value a2
    insert into 자식 value b2
    ```
    
    - 그러나 이렇게 정렬되지 않고 쿼리가 생성되면 쿼리를 묶어서 한번에 사용할 수 없다.
    - 옵션을 true로 주면 아래와 같이 쿼리가 바뀐다.
        
        ```sql
        insert into 부모 value (a1, a2)
        insert into 자식 value (b1, b2)
        ```
        
- Batch Insert가 정확하게 실행되는지 출력하고자 한다면 다음과 같은 옵션을 추가하면 된다.
    
    ```yaml
    spring:
        datasource:
            url: jdbc:mysql://localhost:3306/db명?rewriteBatchedStatements=true&profileSQL=true&logger=Slf4JLogger&maxQuerySizeToLog=999999
    ```
    
    - profileSQL=true: Driver에 전송하는 쿼리를 출력한다.
    - logger=Slf4JLogger: 쿼리 출력 시 사용할 로거를 설정한다.
    - maxQuerySizeToLog=999999: 출력할 쿼리 길이

### 1. Sequence 전략 사용하기

```java
@Entity
@Builder
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@AllArgsConstructor(access = AccessLevel.PRIVATE)
public class Board {

    @Id
    @SequenceGenerator(
            name = "BOARD_SEQ_GENERATOR",   // 식별자 생성기 이름
            sequenceName = "BOARD_SEQ",  // 데이터베이스에 등록되어있는 시퀀스 이름: DB에는 해당 이름으로 매핑된다.
            initialValue = 1,  // DDL 생성시에만 사용되며 시퀀스 DDL을 생성할 때 처음 시작하는 수를 지정
						allocationSize = 50  // 시퀀스 한 번 호출에 증가하는 수
    )
    @GeneratedValue(strategy = GenerationType.SEQUENCE,
                    generator = "BOARD_SEQ_GENERATOR")
    private Long id;

    private String title;
}
```

- 시퀀스는 유일한 값을 순서대로 생성하는 특별한 DB 오브젝트이다.
- IDENTITY 전략과 같지만 내부 동작 방식이 다르다.
    - 시퀀스 전략은 em.persist()를 호출할 때 먼저 데이터베이스 시퀀스를 사용해 식별자를 조회해서 가져오고, 조회한 식별자를 엔티티에 할당한 후 엔티티를 영속성 컨텍스트에 저장한다.

### 2. Table 전략 사용하기

- 키 생성 전용 테이블을 하나 만들고 이름과 값으로 사용할 컬럼을 만들어 데이터베이스 시퀀스 전략을 흉내내는 전략
    
    ```sql
    create table 테이블명 (
        sequence_name varchar(255) not null,
        next_val bigint,
        primary key (sequence_name)
    )
    
    -- 예시
    create table table_sequence (
        TABLE_SEQ varchar(255) not null,
        next_val bigint,
        primary key (TABLE_SEQ)
    )
    ```
    
    - `squence_name` 컬럼은 시퀀스 이름으로 사용하고 `next_val` 컬럼을 시퀀스 값으로 사용한다.

```java
@Entity
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Tb {

    @Id
    @TableGenerator(
            name = "TABLE_SEQ_GENERATOR",
            table = "TABLE_SEQUENCE", // 위에서 생성한 테이블명
            pkColumnName = "TABLE_SEQ" // 위에서 생성한 테이블에 sequence_name
    )
    @GeneratedValue(strategy = GenerationType.TABLE,
                    generator = "TABLE_SEQ_GENERATOR")
    private long id;

    private String title;
}
```

### 3. JDBC를 사용하여 구현하기

```java
jdbcTemplate.batchUpdate("INSERT INTO chat_room_member (chat_room_id, member_id, is_exited) VALUES(" + chatRoomId + ", CAST(? AS bigint), true)",
  new BatchPreparedStatementSetter() {
      @Override
      public void setValues(PreparedStatement ps, int i) throws SQLException {
          ps.setString(1, String.valueOf(joinUsers.get(i)));
      }

      @Override
      public int getBatchSize() {
          return joinUsers.size();   // 저장하고자하는 개수만큼 batchsize 설정
      }
  }
);
```

- `JdbcTemplate.batchUpdate()` 메서드를 사용할 수 있다.

## 참고 자료

- https://imksh.com/113
- [https://velog.io/@blackbean99/SpringBoot-Bulk-Insert-알아보기-Insert-쿼리-최적화](https://velog.io/@blackbean99/SpringBoot-Bulk-Insert-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-Insert-%EC%BF%BC%EB%A6%AC-%EC%B5%9C%EC%A0%81%ED%99%94)
- [https://velog.io/@backtony/JPA-Batch-Insert와-JDBC-Batch-Insert](https://velog.io/@backtony/JPA-Batch-Insert%EC%99%80-JDBC-Batch-Insert)