## CHAR

- 크기가 고정
    - char(10)에 ‘abc’가 들어가도 10바이트의 크기를 가진다.
- 길이가 고정이기 때문에 정해진 값보다 큰 값이 들어오면 오류가 발생한다.
- 고정적인 데이터에 사용하는 것이 유리하다.
- varchar, TEXT는 크기를 계산하는 동작이 포함되기 때문에 느릴 수 있다.

## VARCHAR

- 크기가 가변적이다.
- 길이가 고정이기 때문에 정해진 값보다 큰 값이 들어오면 오류가 발생한다.

### VARCHAR 타입 궁금증

```sql
CREATE TABLE user (
  id       BIGINT NOT NULL,
  name     VARCHAR(1000),
  phone_no VARCHAR(1000),
  address  VARCHAR(1000),
  email    VARCHAR(1000),
  PRIMARY KEY(id)
);
```

- VARCHAR(10)이나 VARCHAR(1000)으로 해도 저장 공간에서는 아무런 차이가 없어보인다.
- 그러나 **VARCHAR 타입의 최대 저장 가능 길이에 따라 테이블을 생성하지 못하게 되는 것을 확인할 수 있다.**
    
    ```sql
    mysql> CREATE TABLE tb_long_varchar (id INT PRIMARY KEY, fd1 VARCHAR(1000000));
    ERROR 1074 (42000): Column length too big for column 'fd1' (max = 16383); use BLOB or TEXT instead
    
    mysql> CREATE TABLE tb_long_varchar (id INT PRIMARY KEY, fd1 VARCHAR(16383));
    ERROR 1118 (42000): Row size too large. The maximum row size for the used table type, not counting BLOBs, is 65535. This includes storage overhead, check the manual. You have to change some columns to TEXT or BLOBs
    
    mysql> CREATE TABLE tb_long_varchar (id INT PRIMARY KEY, fd VARCHAR(16382));
    Query OK, 0 rows affected (0.19 sec)
    
    mysql> ALTER TABLE tb_long_varchar ADD fd2 VARCHAR(10);
    ERROR 1118 (42000): Row size too large. The maximum row size for the used table type, not counting BLOBs, is 65535. This includes storage overhead, check the manual. You have to change some columns to TEXT or BLOBs
    ```
    
    - 하나의 레코드가 저장할 수 있는 최대 길이 65535를 초과하면 새로운 컬럼을 추가할 수 없다.
    - MySQL 서버는 **하나의 VARCHAR 컬럼이 너무 큰 길이를 사용하면 다른 컬럼들이 사용할 수 있는 최대 공간의 크기가 영향을 받게 된다는 것을 확인**할 수 있다.
    - 따라서 레코드 사이즈 한계로 인해 VARCHAR 타입의 최대 저장 길이 설정 시 공간을 아끼는 것이 중요하다.
    - TEXT, BLOB은 해당 제한 사항에 영향을 받지 않는다.

## TEXT

- 크기가 가변적이다.
- 길이 제한이 없어 자유롭게 값을 받을 수 있다.
- varchar와 속도 차이가 없다.

### TEXT 타입 궁금증

- VARCHAR 대신 TEXT를 사용하면 길이 제한 문제가 사라진다.

## 참고 자료

- https://superminy.tistory.com/35
- [https://velog.io/@bonjugi/varchar-vs-text-비교](https://velog.io/@bonjugi/varchar-vs-text-%EB%B9%84%EA%B5%90)
- https://medium.com/daangn/varchar-vs-text-230a718a22a1