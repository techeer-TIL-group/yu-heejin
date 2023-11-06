LeetCode에서 아래 문제를 풀고 있었다.

[Group Sold Products By The Date - LeetCode](https://leetcode.com/problems/group-sold-products-by-the-date/)

```sql
WITH sort_activities AS (
    SELECT DISTINCT sell_date
        , product
    FROM activities
    ORDER BY product
)

SELECT sell_date
    , COUNT(*) AS num_sold
    , GROUP_CONCAT(product) AS products
FROM sort_activities
GROUP BY sell_date
ORDER BY sell_date
;
```

하지만 위 코드는 정답이 아니라 오답이었다.

```sql
# 기대값
["2020-07-23", 3, "**Handbag,Hat**,Mortarboard"]
# 내 결과
["2020-07-23", 3, "**Hat,Handbag**,Mortarboard"]
```

- `WITH` 절 내에서 `ORDER BY`를 하고 있기 때문에 사전순 정렬이 가능할 것이라 생각했으나 **순서가 보장되지 않는 문제가 발생했다.**
- 즉, `WITH` 절에서 `ORDER BY`를 했다 하더라도 `**GROUP_CONCAT` 내부에서는 정렬 유지가 안되는 것이다.**

## MySQL 공식 문서

```sql
GROUP_CONCAT([DISTINCT] expr [,expr ...]
             [ORDER BY {unsigned_integer | col_name | expr}
                 [ASC | DESC] [,col_name ...]]
             [SEPARATOR str_val])
```

- MySQL 공식 문서에서는 `GROUP_CONCAT`의 순서를 설정하려면 함수 내에서 `ORDER BY`를 사용할 것을 권장하고 있다.
    
    ```
    In MySQL, you can get the concatenated values of expression combinations.
    To eliminate duplicate values, use the DISTINCT clause.
    To sort values in the result, use the ORDER BY clause.
    To sort in reverse order,
    add the DESC (descending) keyword to the name of the column you are sorting by in the ORDER BY clause.
    ```
    

## 결론

- `GROUP_CONCAT` 내에서 문자열을 그룹화하는 과정에서 순서를 보장해주지 않는다.
- `GROUP_CONCAT`에서 순서를 보장하고 싶다면, 함수 내에서 `ORDER BY`를 사용한다.
- 최종 정답은 아래와 같다.
    
    ```sql
    SELECT sell_date
        , COUNT(DISTINCT product) AS num_sold
        , GROUP_CONCAT(DISTINCT product ORDER BY product) AS products
    FROM activities
    GROUP BY sell_date
    ORDER BY sell_date
    ;
    ```
    

## 참고 자료

- https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_group-concat