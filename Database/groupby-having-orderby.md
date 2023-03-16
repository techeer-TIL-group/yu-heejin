## GROUP BY

- 데이터를 **특정 컬럼 기준으로 그룹화**시키는 명령어
    - ex) 연령별 평균 매출액 → 연령별로 그룹화
- **그룹을 나누고 다시 그 그룹안에서 세부 그룹**으로 나눌 수 있다.
    - GROUP BY COL1, COL2 → COL1 안에서 다시 COL2로 나누기 가능
    - ex) 연령별 성별 평균 매출액 → 연령별로 먼저 그룹, 그 다음 성별로 그룹지을 수 있음
- 각 그룹에 대한 **연산 결과(합, 평균, 개수 등)를 산출하기 위해 집계 함수**가 필요하다.
- **GROUP BY 구에 있는 컬럼은 반드시 SELECT 절에도 존재해야 한다.**
- GROUP BY 구에도 다양한 함수 사용이 가능하다.

### 그룹 함수

- 하나 이상의 행을 **그룹으로 묶어 연산하기 위한 함수**
- GROUP BY 기준으로 나눠진 각 그룹을 연산하는 역할
- COUNT 함수를 제외한 나머지 그룹함수는 NULL 값 제외하고 계산
    - COUNT 함수는 NULL을 포함한다.

| 함수종류 | 정의 | 참고 |
| --- | --- | --- |
| COUNT | '행'의 개수 | 데이터 NULL인 경우도 COUNT |
| SUM | 합계 | NULL 값 제외하고 연산 |
| AVG | 평균 | NULL 값 제외하고 연산. BUT, NULL값 포함해 평균 계산하고 싶다면 NULL을 0으로 채워줘야 함 |
| MAX | 최댓값 | NULL 값 제외하고 연산 |
| MIN | 최솟값 | NULL 값 제외하고 연산 |
| STDDEV | 표준편차 | NULL 값 제외하고 연산 |
| VARIANCE | 분산 | NULL 값 제외하고 연산 |

## HAVING

- **GROUP BY 절에 대한 조건**을 걸고 싶을 때 사용하는 명령어
- **그룹화된 결과에 조건**을 걸어주는 역할
- HAVING 뒤에는 **SELECT 구문에서 사용하는 AS 별칭 사용 불가능**

### WHERE 절과의 차이점

- 테이블 자체에 대한 조건 : WHERE
- 그룹별로 묶인 컬럼에 대한 조건 : HAVING

## ORDER BY

- 테이블을 **특정 컬럼값을 기준으로 정렬**하기 위한 명령어
    - 즉, ‘정렬’의 역할
- **여러개 컬럼을 기준**으로 정렬 가능하다.
    - ex) ORDER BY COL1, COL2 …
- 기본 정렬값은 오름차순이며, 내림차순으로 정렬하기 원할 경우 DESC
- SELECT 절의 **컬럼 순서 혹은 별칭**으로도 정렬 가능하다.
    - ex) ORDER BY 1, 2, …

## 사용 예시

```sql
SELECT MONTH(ORDER_DATE) AS 월, CATEGORY, SUM(UNIT_PRICE) AS 총주문금액  //월, 카테고리, 총 주문금액
FROM ORDERS   // 해당 테이블에서
WHERE ORDER_DATE >= '2021-01-01'  // 2021년의 데이터만 추린 후
GROUP BY MONTH(ORDER_DATE), CATEGORY  // 각 월의 카테고리 별 주문금
HAVING SUM(UNIT_PRICE) >= 10000
ORDER BY 1;   // 월별로 정렬(날짜별로 정렬)
```

## 참고 자료

- [https://velog.io/@genieee/GROUP-BY-HAVING-ORDER-BY-간단-정리](https://velog.io/@genieee/GROUP-BY-HAVING-ORDER-BY-%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC)