## ON DUPLICATE KEY UPDATE

- 데이터 삽입 시 **PK, UK가 중복된 경우 지정한 데이터만 업데이트**
- 기존 데이터는 UPDATE, 신규 데이터는 INSERT
- 중복된 key가 없는 경우 INSERT

## 사용 예시

```sql
CREATE TABLE BOOK (
	bId INT AUTO_INCREMENT primary KEY,
	bName VARCHAR(50) UNIQUE KEY,
	bPrice INT NOT NULL DEFAULT 0,
);

// 위 테이블에서 bName이 UK
INSERT INTO BOOK (bName, bPrice) VALUES ('chaos_theory', 23000) 
ON DUPLICATE KEY UPDATE price = price + 5000;
```

- 쿼리를 처음 실행하면 INSERT문이 작동하고, 두 번 이상 실행 시 ON DUPLICATE KEY ~ 구문만 실행되어 price에 5000이 더해진다.
- PK가 복합키여도 작동한다.

## 작동 방식

1. INSERT문을 실행하여 신규 데이터를 입력한다.
2. 중복되는 UNIQUE KEY 값을 확인한다.
3. 중복된 UNIQUE KEY값이 존재하면 ON DUPLICATE KEY UPDATE ~ 구문을 실행한다.
4. 새로 INSERT 된 중복되는 UNIQUE KEY값을 가진 항목을 DELETE한다.

## 참고 자료

- https://greensky0026.tistory.com/255
- https://wickedmagica.tistory.com/253
- https://sdevstudy.tistory.com/12