## ORDER BY

```sql
SELECT *
FROM members
ORDER BY id, name;
```

- sql 결과를 정렬하고 싶을 때 사용한다.
- `order by` 기준을 여러개로 설정하고 싶은 경우 컴마로 원하는만큼 이어붙여주면 된다.
- 맨 앞의 값이 같을 경우 뒤의 조건으로 정렬 기준을 판별하게 된다.
- 종종 `ORDER BY 1, 2` 형식으로 쓰기도 하는데, **이 숫자는 컬럼의 순서를 의미**한다.
    
    ```sql
    SELECT id, name
    FROM members
    ORDER BY 1, 2;
    ```
    

## DATEDIFF

```sql
SELECT DATEDIFF(날짜형식, 시작날짜, 종료날짜)

-- 예시
SELECT DATEDIFF(DAY, '2017-02-13', '2017-03-15') AS RESULT # 30
```

- 두 날짜의 차이를 계산하는 함수
- **시작 날짜에서 종료 날짜까지의 일 수 차이를 반환**한다.
- 날짜 포맷에 시간이 포함되어 있는 경우 시간은 계산에 포함하지 않는다.
- 날짜 범위에서 벗어나는 값을 입력하는 경우 NULL을 반환한다.

## DATE_ADD / DATE_SUB

```sql
DATE_ADD(기준 날짜, INTERVAL)
DATE_SUB(기준 날짜, INTERVAL)

# 현재 시간에 1초 더하기
SELECT DATE_ADD(NOW(), INTERVAL 1 SECOND);
# 현재 시간에 1분 더하기
SELECT DATE_ADD(NOW(), INTERVAL 1 MINUTE);
# 현재 시간에 1시간 더하기
SELECT DATE_ADD(NOW(), INTERVAL 1 HOUR);
# 현재 시간에 1일 더하기
SELECT DATE_ADD(NOW(), INTERVAL 1 DAY);
# 현재 시간에 1달 더하기
SELECT DATE_ADD(NOW(), INTERVAL 1 MONTH);
# 현재 시간에 1년 더하기
SELECT DATE_ADD(NOW(), INTERVAL 1 YEAR);
```

- 특정 날짜와 숫자값을 가지고 계산하는 함수
- `DATE_ADD`는 입력된 기간만큼 더하는 함수이며, `DATE_SUB`는 입력한 기간만큼 빼는 함수이다.
- `DATE_SUB`도 같은 형식으로 작성하면 된다.

## TIMEDIFF

```sql
SELECT TIMEDIFF('2022-02-01 23:00:00','2022-01-30 00:00:00');
```

- 두 기간 사이의 시간 계산
- 시간 또는 날짜 범위에서 벗어난 값을 입력하는 경우 `NULL`을 반환한다

## PEROID_DIFF

```sql
SELECT PERIOD_DIFF('202202','202112');
>> 2

SELECT PERIOD_DIFF('202202','201212');
>> -10

SELECT PERIOD_DIFF('2202','1912');
>> 26
```

- 두 기간 사이의 개월 수 차이 계산
- `YYYYMM` 혹은 `YYMM` 형식으로 값을 지정한다.

## 중복 데이터 제거하기

- SELECT 시 distinct를 붙이면 중복 데이터를 제거한 결과를 얻을 수 있다.

## 날짜 데이터에서 일부만 추출하기

- YEAR : 연도 추출
- MONTH : 월 추출
- DAY : 일 추출 (DAYOFMONTH와 같은 함수)
- HOUR : 시 추출
- MINUTE : 분 추출
- SECOND : 초 추출

## SQL 구문 순서

```sql
SELECT 컬럼명 --------------------- (5) 
FROM 테이블명 ------------------- (1)
WHERE 테이블 조건 --------------- (2)
GROUP BY 컬럼명 -------------------- (3)
HAVING 그룹 조건 ----------------- (4)
ORDER BY 컬럼명 -------------------- (6)
```

## ‘같지 않다’ 연산자

- <>
- ≠

## IF-ELSE / CASE-WHEN-THEN / IFNULL

### IF-ELSE

```sql
IF(조건문, 참일 때의 값, 거짓일 때의 값)

-- 예시
SELECT IF(department = '감자튀김', '감자', '고구마') AS '구황작물'
```

### CASE-WHEN-THEN

```sql
CASE
	WHEN 조건 THEN '조건 반환값'
	WHEN 조건 THEN '조건 반환값'
	ELSE default_value
END

-- 예시
CASE
    WHEN status = 'SALE'
    THEN '판매중'
    WHEN status = 'RESERVED'
    THEN '예약중'
    WHEN status = 'DONE'
    THEN '거래완료'
END AS status
```

### IFNULL

```sql
IFNULL(값1, 값2)

-- 예시
SELECT IFNULL(name, '이름 없음')
```

## CONCAT / SUBSTRING / GROUP_CONCAT

### CONCAT

```sql
CONCAT('내 이름은 ', name, '입니다.'). # 내 이름은 ${name} 입니다.
```

- 문자열 합치기
- 단, 문자열 요소 중 하나라도 `NULL`이면 `NULL`을 반환한다.
    
    ```sql
    CONCAT('내 이름은 ', NULL, '입니다.').  # NULL
    ```
    

### SUBSTRING

```sql
SUBSTRING(str, pos, len)

-- 예시
# 시작 위치부터 자른다.
SUBSTRING('안녕하세요', 2)   # 녕하세요
# 문자열 len을 주면 해당 길이까지만 자른다.
SUBSTRING('안녕하세요저는감자튀김입니다룰루랄라', 3, 6)  # 하세요저는감
# 음수면 끝에서부터 자른다.
SUBSTRING('안녕하세요', -2)   # 세요
```

### GROUP_CONCAT(col_name SEPARATOR ‘,’)

```sql
SELECT type, group_concat(name) FROM test GROUP BY type ;
```

- 세로로 뽑힌 데이터(행 데이터)를 합쳐서 가로로(문자열 하나로) 뽑을 수 있는 함수

## REPLACE

```sql
REPLACE(str,from_str,to_str)

-- 예제
mysql> SELECT REPLACE('www.mysql.com', 'w', 'Ww'); # 'WwWwWw.mysql.com'
```

- 문자열을 바꾼다.

## 참고 자료

- https://jhnyang.tistory.com/470
- https://ggmouse.tistory.com/134
- https://extbrain.tistory.com/109
- [https://velog.io/@donghoim/MySQL-시간-더하기-빼기-DATEADD-DATESUB-함수](https://velog.io/@donghoim/MySQL-%EC%8B%9C%EA%B0%84-%EB%8D%94%ED%95%98%EA%B8%B0-%EB%B9%BC%EA%B8%B0-DATEADD-DATESUB-%ED%95%A8%EC%88%98)
- [https://velog.io/@12aeun/SQL-mysql에서-날짜-시간-계산하기](https://velog.io/@12aeun/SQL-mysql%EC%97%90%EC%84%9C-%EB%82%A0%EC%A7%9C-%EC%8B%9C%EA%B0%84-%EA%B3%84%EC%82%B0%ED%95%98%EA%B8%B0)
- https://extbrain.tistory.com/60
- https://data-make.tistory.com/23
- https://zorba91.tistory.com/32
- https://redcow77.tistory.com/260
- [https://blog.edit.kr/entry/mysql-문자열-합치기](https://blog.edit.kr/entry/mysql-%EB%AC%B8%EC%9E%90%EC%97%B4-%ED%95%A9%EC%B9%98%EA%B8%B0)
- https://roqkffhwk.tistory.com/90
- https://fruitdev.tistory.com/16
- https://extbrain.tistory.com/52