- **여러 값을 `OR` 관계로 묶어 나열하는 조건**을 WHERE 절에 사용할 때 쓸 수 있는 키워드이다.
- **조건의 범위를 지정하는데 사용**되며, 값은 콤마(,)로 구분하여 괄호 내애 묶는다.
- 해당 값 중에서 **하나 이상 일치하면 조건에 부합하는 것으로 간주**한다.

## WHERE {column_name} IN (조건1, 조건2, 조건3 …)

- `WHERE` 뒤에 쓴 컬럼과 `IN` 뒤로 나열한 조건들 중 일치하는 `ROW`를 가져온다.
- `IN` 뒤에 나열한 조건들은 `OR` 조건으로 검색하게 된다.

## WHERE {column_name} NOT IN (조건1, 조건2, 조건3 …)

- `IN` 뒤로 나열한 조건과 일치하지 않는 `ROW`들을 가져온다.
- 컬럼의 내용이 조건1이거나 조건2이거나 조건3인 내용을 **제외하고** 가져온다.
- `OR` 조건으로 검색한다.

## WHERE {column_name} IN (조건)

- 다중 조건뿐만 아니라 하나의 조건도 줄 수 있다. (`NOT IN`도 마찬가지이다.)

## IN 조건의 장점

- 목록에 넣을 값이 여러개일 경우 `OR` 보다 쓰거나 보기에도 이해하기 쉽다.
- `IN` 연산자가 `OR` 연산자보다 실행 속도가 빠르다.
- `IN` 연산자 안에 다른 `SELECT`문(서브쿼리)를 넣을 수 있다.

## 참고 자료

- [https://ojava.tistory.com/12](https://ojava.tistory.com/12)
- [https://velog.io/@inyong_pang/MySQL-IN-조건](https://velog.io/@inyong_pang/MySQL-IN-%EC%A1%B0%EA%B1%B4)