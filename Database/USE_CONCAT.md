## USE_CONCAT

- `OR` 조건이나 `IN` 조건으로 검색하는 경우, 인덱스 Range Scan을 할 수 없다.
    - `OR` 조건은 두 개의 별도 범위를 지정하기 때문에 하나의 연속적인 범위로 인덱스를 스캔하는 것이 불가능하기 때문이다.
    - `IN` 조건은 `OR` 조건을 표현하는 다른 방식일 뿐이다.
- `IN` 조건이나 `OR` 조건이 있는 SQL에 `USE_CONCAT` 힌트를 사용하면 OR Expansion(Union All 쿼리 변환)이 발생한다.
    
    ```sql
    WHERE (전화번호 = :tel_no OR 고객명 = :cust_nm)
    
    // 변환 결과
    SELECT *
    FROM 고객
    WHERE 고객명 = :cust_nm     -- 고객명이 선두 컬럼인 인덱스 Range Scan
    UNION ALL
    SELECT *
    FROM 고객
    WHERE 전화번호 = :tel_no     -- 전화번호가 선두 컬럼인 인덱스 Range Scan
    	AND (고객명 <> :cust_nm OR 고객명 IS NULL)
    ```
    
- `IN`, `OR` 조건을 `UNION ALL`로 작성하면, **각 브랜치별로 인덱스 스캔 지점을 찾을 수 있기 때문에 인덱스 스캔이 가능하다.**
- 반대로 OR Expansion을 방지하려면 `NO_EXPAND`를 사용하면 된다.

## 사용 시 주의할 점

- 인자 없이 `USE_CONCAT`을 사용할 경우 모든 경우의 수를 고려하여 가장 비용이 저렴한 계획을 사용하기 때문에 `UNION ALL`로 분리가 되지 않을 수도 있다.
- 따라서 만약 힌트를 사용했으나 `UNION ALL`로 분리가 되지 않고 성능에 문제가 된다면 `USE_CONCAT` 힌트의 숫자 인자를 활용하여 튜닝해야 한다.
    
    ```sql
    SELECT /*+ GATHER_PLAN_STATISTICS QB_NAME(MAIN) USE_CONCAT(@MAIN 1) */
    e.employee_id, e.first_name, e.last_name,  d.department_name
    FROM employees e, departments d
    WHERE e.department_id = d.department_id   
    	AND (     
    		d.department_id IN (:v_dept1, :v_dept2)           
    	OR e.manager_id IN (:v_manager1, :v_manager2)
    ) ;
    
    ```
    
    - 힌트에 숫자 1을 사용하면 가능한 경우 모두 `UNION ALL` 로 분리하라는 의미이다.

## 참고 자료

- [https://scidb.tistory.com/entry/USECONCAT-힌트-제대로-알기](https://scidb.tistory.com/entry/USECONCAT-%ED%9E%8C%ED%8A%B8-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B8%B0)
- https://swpju.tistory.com/entry/OR-ExpansionUseconcat
- 친절한 SQL 튜닝 2장