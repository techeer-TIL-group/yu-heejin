데브코스 코딩 테스트를 풀다가 못 푼 문제가 있어서 생각난김에 비슷한 문제를 찾아봤다.

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/131115)

## 서브쿼리를 사용하여 풀기

```sql
SELECT
	*
FROM
	food_product
WHERE price = (
	SELECT
		MAX(price)
	FROM
		food_product
)
;
```

## 오름차순으로 정렬한 후 Limit 걸기

```sql
SELECT
	*
FROM
	food_product
ORDER BY
	price DESC
LIMIT 
	1
;
```