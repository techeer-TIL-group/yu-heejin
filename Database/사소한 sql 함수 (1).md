## LIKE

- ‘감자’로 시작하는 데이터 검색
    
    ```sql
    SELECT * FROM table WHERE title LIKE '감자%';
    ```
    
- ‘감자’로 끝나는 데이터 검색
    
    ```sql
    SELECT * FROM table WHERE title LIKE '%감자';
    ```
    
- ‘감자’가 들어가는 데이터 검색
    
    ```sql
    SELECT * FROM table WHERE title LIKE '%감자%';
    ```
    
    - 이 경우 앞 뒤 공백까지 포함해서 검색한다.
    - 이 경우 `trim`을 이용해 공백을 제거해야한다.
- `_`를 사용할 경우 글자수를 정해주며, `%`를 사용하는 경우 글자수를 정해주지 않는다.
    
    ```sql
    --A로 시작하는 문자를 찾기--
    SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE 'A%'
    
    --A로 끝나는 문자 찾기--
    SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE '%A'
    
    --A를 포함하는 문자 찾기--
    SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE '%A%'
    
    --A로 시작하는 두글자 문자 찾기--
    SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE 'A_'
    
    --첫번째 문자가 'A''가 아닌 모든 문자열 찾기--
    SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE'[^A]'
    
    --첫번째 문자가 'A'또는'B'또는'C'인 문자열 찾기--
    SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE '[ABC]'
    SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE '[A-C]'
    ```
    
- 중복 조건을 검색하기 위해 `OR`을 사용하기도 한다.
    
    ```sql
    SELECT [필드명]
    FROM [테이블명]
    WHERE [필드명] LIKE '%특정 문자열%'
    	OR [필드명] LIKE '%특정 문자열2%';
    ```
    
    - 이 경우 복잡해질 수 있기 떄문에 정규식을 사용하기도 한다.
        
        ```sql
        -- 복수개의 특정 문자를 포함하는 데이터 검색 (특정 문자열을 '|' 를 기준으로 나눈다)
        
        SELECT [필드명]
        FROM [테이블명]
        WHERE [필드명] REGEXP '특정 문자열|특정 문자열2';
        ```
        

## WHERE EXISTS

- `IN`과 유사한 개념이지만 적용되는 범위가 다르다.
    - `IN()` 안에는 특정 값이나 서브쿼리가 올 수 있지만, `EXISTS()` 안에는 서브쿼리만 올 수 있다.
- 또한, `IN`은 `()` 안에 있는 **특정값이나 서브쿼리 결과값이 포함되는지만 체크**하나, `EXISTS`는 `()` 안에 **서브쿼리로부터 해당 컬럼의 값이 존재하는지 여부만 체크**한다.
- `IN`과 `EXISTS`는 동일한 결과를 보여주지만 `EXISTS`가 더 좋은 성능을 보여준다.
    - 따라서 단순히 특정 컬럼의 값을 이용할 땐 `IN`을, 서브쿼리를 사용할 땐 `EXISTS`를 사용한다.

## COUNT

### COUNT(1), COUNT(*), COUNT(col_name) 차이점

- 실제로 사용해보면 아무 차이도 없다.
- 다만 가독성을 위해서는 `COUNT(1)`보다는 `COUNT(*)`을 사용할 것을 권장한다.

### DISTICT

- 중복을 제외하고 카운트를 셀 수 있다.

### GROUP BY COUNT

- 그룹으로 묶은 후 집계

## NULL 값 치환하기

### IFNULL(?, ?)

- 컬럼이 `NULL`이면 0으로 치환하여 반환
    
    ```sql
    SELECT IFNULL(col_name, 0) FROM TEST;
    ```
    

### IF()

- 컬럼이 `NULL`일 경우 1을, `NULL`이 아닐 때는 2를 return
    
    ```sql
    IF(col_name IS NULL, '1', '2') FROM TEST;
    ```
    

### NULLIF(?, ?)

- 전자 == 후자 의 결과가 `false`면 전자 값 반환, `true`이면 `NULL`을 리턴한다.
    
    ```sql
    SELECT NULLIF(1, 1) ; 
    --> null 을 리턴한다.
     
    SELECT NULLIF(1, 2) ;
    --> 1을 리턴한다.
    ```
    

## MAX(), MIN()

### MAX()

- 선택된 칼럼 중 가장 큰 값 하나를 가져온다.

### MIN()

- 선택된 칼럼 중 가장 작은 값 하나를 가져온다.

## DATETIME 포멧 변경

- MySQL에서 DATETIME 타입은 `YYYY-MM-DD hh:mm:ss` 형식으로 반환한다.
- `DATE` 타입의 경우 `YYYY-MM-DD` 형식을 가지며, `DATE_FORMAT`으로 `%Y-%m-%d %h:%m:%s` 형식을 지정하면 시분초 값이 자동으로 0으로 채워진다.

## 참고 자료

- https://bactoria.tistory.com/22
- https://coding-factory.tistory.com/114
- https://lollolzkk.tistory.com/44
- https://runtoyourdream.tistory.com/112
- https://sjs0270.tistory.com/50
- https://wakestand.tistory.com/575
- https://sukkyu.tistory.com/60
- https://dkswnkk.tistory.com/534
- https://ggmouse.tistory.com/472
- [https://makand.tistory.com/entry/SQL-MAX-MIN-구문](https://makand.tistory.com/entry/SQL-MAX-MIN-%EA%B5%AC%EB%AC%B8)
- https://kig6022.tistory.com/7