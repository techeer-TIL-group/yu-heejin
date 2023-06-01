## REPLACE INTO

- Oracle에서는 MERGE INTO로 쓰인다.
- **PK를 기준**으로 데이터가 존재하면 **기존 데이터를 지우고 기준 키로 데이터를 다시 쓰는 것**

## 사용 예시

```sql
REPLACE INTO table VALUES();
REPLACE INTO table SET column = value;
```

- 기존 key값과 겹치는 부분이 없으면 INSERT, 동일한 key가 있다면 DELETE후 INSERT
- 작성하지 않은 컬럼 값은 DEFAULT값 or NULL
- 기존 key와 겹치는 경우, 이전 key가 들어있던 row가 삭제된 후, 내가 REPLACE 부분에 작성한 row가 신규로 들어간다.
- SET으로 사용할 경우 **WHERE를 사용할 수 없다.**

## 참고 자료

- https://wakestand.tistory.com/527
- https://xggames.tistory.com/103
- https://www.javatpoint.com/mysql-replace