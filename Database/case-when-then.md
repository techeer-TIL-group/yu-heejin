컬럼의 조건에 따라 다른 컬럼의 값을 업데이트 해야할 경우 사용한다.

## 구문 형식

```sql
CASE
	WHEN 조건
	THEN ‘반환 값’
	WHEN 조건
	THEN ‘반환 값’
	ELSE ‘WHEN 조건에 해당 안되는 경우 반환 값’
END AS ~
```

- WHEN과 THEN은 한 쌍이어야 하며, 다수 존재할 수 있다.
- ELSE는 모든 조건에 해당하지 않는 경우 반환 값을 설정할 수 있다.
- ELSE가 존재하지 않고 조건에 맞지 않아 반환 값이 없으면 NULL을 반환한다.

## 사용 예시

```sql
SELECT
    history_id,
    car_id,
    DATE_FORMAT(start_date, '%Y-%m-%d') AS start_date, 
    DATE_FORMAT(end_date, '%Y-%m-%d') AS end_date,
    CASE
    WHEN DATEDIFF(end_date, start_date) + 1 >= 30
    THEN '장기 대여'
    ELSE '단기 대여'
    END
    AS rent_type
FROM car_rental_company_rental_history
WHERE DATE_FORMAT(start_date, '%Y-%m') = '2022-09'
ORDER BY history_id DESC;
```

## 참고 자료

- [https://www.next-t.co.kr/seo/sql/mysql-case-when-then-구문/](https://www.next-t.co.kr/seo/sql/mysql-case-when-then-%EA%B5%AC%EB%AC%B8/)
- https://extbrain.tistory.com/46