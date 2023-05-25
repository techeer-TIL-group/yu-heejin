- result.insertId로 확인할 수 있다.
    
    ```jsx
    const result = await Mysql.queryWithParam(
    	`INSERT INTO table
    	SET ?;`, instanceToPlain(param), conn
    );
    
    return result.insertId;
    ```
    

## 참고 자료

- [https://taptorestart.tistory.com/entry/Nodejs-mysql-모듈-데이터베이스에-insert-데이터-입력-뒤-id-값-확인하기](https://taptorestart.tistory.com/entry/Nodejs-mysql-%EB%AA%A8%EB%93%88-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4%EC%97%90-insert-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%9E%85%EB%A0%A5-%EB%92%A4-id-%EA%B0%92-%ED%99%95%EC%9D%B8%ED%95%98%EA%B8%B0)