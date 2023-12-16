아래와 같이 WITH절을 사용하면 오류가 발생한다.

```sql
WITH friends AS (
    SELECT requester_id AS id
    FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS id
    FROM RequestAccepted
)

WITH friends_count AS (
    SELECT id
        , COUNT(id) AS count
    FROM friends
    GROUP BY id
    ORDER BY count DESC
)
```

WITH절을 연속으로 사용하고자 한다면 다음과 같이 사용해야 한다.

```sql
WITH friends AS (
    SELECT requester_id AS id
    FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS id
    FROM RequestAccepted
),
friends_count AS (
    SELECT id
        , COUNT(id) AS count
    FROM friends
    GROUP BY id
    ORDER BY count DESC
)
```

## 참고 자료

- GPT 4
- https://k1asd1.tistory.com/23