필요에 의해 **서로 다른 결과를 한 줄로 합쳐서 보여줘야 하는 경우**가 있다.

이 때, 전체 결과값을 가져와서 java와 같은 프로그램 언어 안에서 for문을 돌며 문자열을 붙여도 되지만, **`SELECT` 쿼리를 던질 때 결과값으로 합쳐져 있는 문자열**을 받을 수도 있다.

이처럼 **여러개의 `ROW`로 되어 있는 데이터를 한 개의 값으로 묶어서 가지고 올 경우** `GROUP_CONCAT`을 사용하여 처리할 수 있다.

## 사용 예시

### SELECT * FROM test;

| type | name |
| --- | --- |
| fruit | 수박 |
| fruit | 사과 |
| fruit | 바나나 |
| fruit | 사과 |

### SELECT type, `GROUP_CONCAT(name)` FROM test GROUP BY type;

| type | name |
| --- | --- |
| fruit | 수박,사과,바나나,사과 |
- `GROUP_CONCAT`을 기본적인 형태로 사용했을 경우 **문자열 사이에 쉼표(,)**가 붙게 된다.
- **구분자를 변경**하고 싶을 땐 다음과 같이 `SEPARATOR ‘구분자’`를 붙여준다.
    - SELECT type, `GROUP_CONCAT(name SEPARATOR ‘|’)` FROM test GROUP BY type;
        
        
        | type | name |
        | --- | --- |
        | fruit | 수박|사과|바나나|사과 |
- 합쳐지는 문자열에서 **중복되는 문자열을 제거**할 땐 `DISTINCT`를 사용한다.
    - SELECT type, `GROUP_CONCAT(DISTINCT name)` FROM test GROUP BY type;
        
        
        | type | name |
        | --- | --- |
        | fruit | 수박,사과,바나나 |
- **문자열을 정렬**하여 나타내고 싶으면 `ORDER BY`를 사용한다.
    - SELECT type, `GROUP_CONCAT(DISTINCT name ORDER BY name)` FROM test GROUP BY type;
        
        
        | type | name |
        | --- | --- |
        | fruit | 바나나,사과,수박 |

## 참고 자료

- [https://fruitdev.tistory.com/16](https://fruitdev.tistory.com/16)