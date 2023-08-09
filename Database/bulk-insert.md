## Bulk Insert

- DB에 insert해야하는 데이터가 많은 경우 사용되는 기법
- 한번에 **다량의 데이터를 DB에 insert할 때 유용하다.**

### 사용 예시

- 기존 쿼리
    
    ```sql
    INSERT INTO table VALUES (1, "hello");
    INSERT INTO table VALUES (2, "world");
    INSERT INTO table VALUES (3, "!");
    ```
    
- Bulk Insert
    
    ```sql
    INSERT INTO table VALUES (1, "hello"), (2, "world"), (3, "!");
    ```
    

## 참고 자료

- https://dev.dwer.kr/2020/04/mysql-bulk-inserting.html