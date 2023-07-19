## JOIN

- 두 개 이상의 테이블에 대해서 결합하여 나타낼 때 사용한다.

## Inner Join

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F990E01375CEA5F1434)

```sql
SELECT table1.col1, table1.col2, ..., table2.col1, table2.col2, ...
FROM table1 [table1의 별칭]
JOIN table2 [table2의 별칭] ON table1.col1 = table2.col2
```

- 조인하고자 하는 두 개의 테이블에서 **공통된 요소들을 통해 결합**한다.
- sql에서 단순히 조인을 사용할 땐 암묵적으로 이너 조인을 사용한다.
- 조인 시 table1과 table2의 어떤 컬럼을 기준으로 할 지는 ON 뒤에 작성한다.
    - 위 쿼리는 table1의 col1 컬럼과 table2의 col2 컬럼이 같은 행들에 대해서 조인을 실시한다.

## Outer Join

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99887C415CEA662D2A)

- 아우터 조인은 left outer join, right outer join, full outer join 세 종류가 있다.
- 아우터 조인은 **두 테이블의 공통 영역을 포함해 한쪽 테이블의 다른 데이터를 포함하는 조인 방식이다.**

### Left Outer Join

```sql
select employee.empName, department.deptName
from employee
left outer join department on employee.deptNo = department.deptNo
```

- 왼쪽 테이블에 누락된 값이 출력된다.

### Right Outer Join

```sql
select employee.empName, department.deptName
from employee
right outer join department on employee.deptNo = department.deptNo
```

- 오른쪽 테이블에 누락된 값이 출력된다.

### Full Outer Join

```sql
select employee.empName, department.deptName
from employee
full outer join department on employee.deptNo = department.deptNo
```

- 양쪽 테이블에 공통된 값이 없어도 누락된 값이 출력된다.

## Cross Join

```sql
select employee.empName, department.deptName
from employee
cross join department
```

- 이너 조인과 아우터 조인은 두 테이블간의 특정 기준에 의해 데이터 결합의 결과를 보여주는 방식이었다면, 크로스 조인은 **특정 기준 없이 두 테이블간 가능한 모든 경우의 수에 대한 결합을 보여준다.**

## Self Join

- 말 그대로 자기 스스로를 결합시키는 조인이다.

## 참고 자료

- https://doorbw.tistory.com/223