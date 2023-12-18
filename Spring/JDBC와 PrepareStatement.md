## JDBC (Java DataBase Connectivity)

![https://shs2810.tistory.com/18](https://prod-files-secure.s3.us-west-2.amazonaws.com/f30f0daf-8f58-4626-9f2f-1aec07bc75ed/98d23b75-f7ac-4de4-825c-8a180073ea72/Untitled.png)

https://shs2810.tistory.com/18

- Java 기반 애플리케이션의 데이터를 데이터베이스에 저장 및 업데이트하거나, 데이터베이스에 저장된 데이터를 Java에서 사용할 수 있도록 하는 Java API
    - Java 애플리케이션에서 데이터베이스에 접근하기 위해 **JDBC API를 사용하여 데이터베이스에 연동**할 수 있으며, **데이터베이스에서 자료를 Query하거나 업데이트 하는 방법을 제공**한다.
- 응용프로그램과 DBMS 간의 통신을 중간에서 번역해주는 역할을 한다.
- 아래와 같은 3가지 기능을 표준 인터페이스로 정의하여 제공한다.
    
    ![https://ittrue.tistory.com/250](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIS7Q7%2FbtrR4G2GJPN%2FU3k0zntzKSMYLO3HJC8431%2Fimg.png)
    
    https://ittrue.tistory.com/250
    
    - java.sql.Connection: 연결
    - java.sql.Statement: SQL을 담은 내용
    - java.sql.ResultSet: SQL 요청 응답
- Spring Data JDBC, Spring Data JPA와 같은 기술이 등장하면서 JDBC API를 직접적으로 사용할 일은 줄어들었지만, 내부적으로 JDBC를 이용하기 때문에 내부 동작에 대해 알 필요가 있다.

## JDBC API Package

### java.sql.*

- Data source(RDB)에 저장된 데이터를 접근하고 처리하기 위한 API 제공
- Connections, Statement, PreparedStatement, ResultSet 등

### javax.sql.*

- 서버에 존재하는 data source를 접근하고 이용하기 위한 API 제공
- **데이터베이스 자체에 접근**하기 위해 사용한다.
- DataSource, XADataSource 등

## JDBC 동작 흐름

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbHk4IH%2FbtrR2EZokg5%2FVlsEZPLukP9YKb7tK0paT1%2Fimg.png)

- JDBC API를 사용하기 위해 JDBC 드라이버를 먼저 로딩한 후 데이터베이스와 연결한다.

### JDBC 드라이버

- 데이터베이스와의 통신을 담당하는 인터페이스
- Oracle, MySQL 등과 같이 데이터베이스에 알맞은 JDBC 드라이버를 구현하여 제공한다.
    - 따라서 DBMS 종류와 상관없이 동일한 방법으로 데이터베이스 접속 및 질의 실행이 가능하다.
- JDBC 드라이버의 구현체를 이용해 데이터베이스에 접근할 수 있다.

## JDBC API 사용 흐름

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwTyMC%2FbtrR5yww0DA%2Fjwst72s4xtTKhLTtzfJ0mK%2Fimg.png)

1. JDBC 드라이버 로딩: 사용하고자 하는 JDBC 드라이버를 로딩한다.
2. Connection 객체 생성: JDBC 드라이버가 정상적으로 로딩되면, DriverManager를 통해 데이터베이스와 연결되는 세션인 Connection 객체를 생성한다.
3. Statement 객체 생성: Statement 객체는 작성된 SQL 쿼리문을 실행하기 위한 객체로 정적 SQL 쿼리 문자열을 입력으로 가진다.
4. Query 실행: 생성된 Statement 객체를 이용해 입력한 SQL 쿼리를 실행한다.
5. ResultSet 객체로부터 데이터 조회: 실행된 SQL 쿼리문에 대한 결과 데이터셋
6. ResultSet, Statement, Connection 객체들의 Close: JDBC API를 통해 사용된 객체들은 생성된 객체들을 사용한 순서의 역순으로 Close한다.

## Statement, PreparedStatement

- 두 인터페이스 모두 쿼리문을 실행하는 인터페이스이지만 차이가 존재한다.

### SQL 실행 단계

1. 쿼리 문장 분석
2. 컴파일
3. 실행

### Statement

```java
Connection conn = DriverManager.getConnection(url, id, pwd);
Statement stmt = conn.createStatement();
stmt.executeUpdate("Update Member Set Name = " + param + "Where id = " + 10);
stmt.executeUpdate("Update Member Set Name = " + param + "Where id = " + 11);
```

- 쿼리문을 수행할 때마다 SQL 실행 단계 1~3단계를 거친다.
    - 사용자가 입력한 값에 따라 매번 달라지는 SQL문 자체를 DB로 보내기 때문이다.
- SQL문을 수행하는 과정에서 매번 컴파일하기 때문에 성능상 이슈가 발생한다.

### PreparedStatement

```java
public void saveAllChatMembers(List<Long> joinUsers, Long chatRoomId) {
  jdbcTemplate.batchUpdate(
      "INSERT INTO chat_room_member (chat_room_id, member_id, is_exited) VALUES (?, ?, ?)",
      new BatchPreparedStatementSetter() {
          @Override
          public void setValues(PreparedStatement ps, int i) throws SQLException {
              ps.setLong(1, chatRoomId);
              ps.setLong(2, joinUsers.get(i));
              ps.setBoolean(3, false);
          }

          @Override
          public int getBatchSize() {
              return joinUsers.size();
          }
      }
  );
}
```

- **컴파일이 미리 되어있기 때문에 Statement에 비해 좋은 성능을 낸다.**
    - 처음 한 번만 1~3단계를 거친 후 캐시에 담아 재사용한다.
- 특수문자를 자동으로 파싱하기 때문에 SQL Injection 같은 공격을 막을 수 있다.
    - 내부 코드
        
        ```java
        /**
             * Convert a string to a Java literal using the correct escape sequences.
             * The literal is not enclosed in double quotes. The result can be used in
             * properties files or in Java source code.
             *
             * @param s the text to convert
             * @param buff the Java representation to return
             * @param forSQL true if we embedding this inside a STRINGDECODE SQL command
             */
            public static void javaEncode(String s, StringBuilder buff, boolean forSQL) {
                int length = s.length();
                for (int i = 0; i < length; i++) {
                    char c = s.charAt(i);
                    switch (c) {
        //            case '\b':
        //                // BS backspace
        //                // not supported in properties files
        //                buff.append("\\b");
        //                break;
                    case '\t':
                        // HT horizontal tab
                        buff.append("\\t");
                        break;
                    case '\n':
                        // LF linefeed
                        buff.append("\\n");
                        break;
                    case '\f':
                        // FF form feed
                        buff.append("\\f");
                        break;
                    case '\r':
                        // CR carriage return
                        buff.append("\\r");
                        break;
                    case '"':
                        // double quote
                        buff.append("\\\"");
                        break;
                    case '\'':
                        // quote:
                        if (forSQL) {
                            buff.append('\'');
                        }
                        buff.append('\'');
                        break;
                    case '\\':
                        // backslash
                        buff.append("\\\\");
                        break;
                    default:
                        if (c >= ' ' && (c < 0x80)) {
                            buff.append(c);
                        // not supported in properties files
                        // } else if (c < 0xff) {
                        // buff.append("\\");
                        // // make sure it's three characters (0x200 is octal 1000)
                        // buff.append(Integer.toOctalString(0x200 | c).substring(1));
                        } else {
                            buff.append("\\u")
                                    .append(HEX[c >>> 12])
                                    .append(HEX[c >>> 8 & 0xf])
                                    .append(HEX[c >>> 4 & 0xf])
                                    .append(HEX[c & 0xf]);
                        }
                    }
                }
            }
        ```
        

## PreparedStatement를 사용해야하는 이유

### SQL Injection

- Statement 방식을 사용하면 다음과 같은 문제가 발생한다.
    
    ```java
    Connection conn = DriverManager.getConnection(url, id, pwd);
    
    Statement stmt = conn.createStatement();
    
    stmt.executeQuery("Select * from Member Where id = " + id + " and pwd = " + pwd);
    ```
    
    ```sql
    Select * 
    from Member 
    Where id = ‘test123’ and pwd = ‘1234’ OR ‘1’ = ‘1’; 
    ```
    
    - 따라서 사용자가 악의적으로 파라미터 값을 조작하여도 막는 것이 어렵다.
    - `‘1’ = ‘1’`은 항상 true이기 때문에 모든 행이 반환된다. (WHERE true)
- PreparedStatement 방식을 사용하면 파라미터 값이 악의적으로 조작되어도 파라미터 바인딩 덕분에 이러한 문제를 방지할 수 있다.
    
    ```java
    Connection conn = DriverManager.getConnection(url, id, pwd);
    
    String sql = "Select * from Member Where id = ? and pwd = ?";
    
    PreparedStatement pstmt = conn.prepareStatement(sql);
    pstmt.setString(1, id);
    pstmt.setString(2, pwd);
    pstmt.executeQuery(); 
    ```
    

### DB 캐시 문제

<aside>
💡 라이브러리 캐시란 SQL 파싱, 최적화, 로우 소스 생성 과정을 거쳐 생성한 내부 프로시저를 재사용할 수 있도록 캐싱해두는 메모리 공간이다.

</aside>

- SQL의 경우 라이브러리 캐시에 쿼리문이 저장될 때 전체 SQL이 이름 역할을 대신한다.
- 따라서 아래는 모두 다른 SQL문이 된다.
    
    ```sql
    SELECT * FROM emp WHERE empno = 7900;
    select * from EMP where EMPNO = 7900;
    select * from emp where empno = 7900;
    select /* comment */ * from emp where empno = 7900;
    select /*+ fist_rows */ * from emp where empno = 7900;
    ...
    ```
    
    - 모두 같은 의미임에도 불구하고 서로 다른 SQL문으로 인식하게 되어 캐시의 공간을 차지한다.
- 만약 아래와 같이 Statement 객체를 사용할 때, 라이브러리 캐시는 다음과 같다.
    
    ```java
    public void login(String login_id) throws Exception {
    	String SQLStmt = "SELECT * FROM CUSTOMER WHERE LOGIN_ID = '" + login_id + "'";
    	Statement st = con.createStatement();
    	ResultSet rs = st.executeQuery(SQLStmt);
    	
    	if (rs.next()) {
    		// do anything
    	}
    
    	rs.close();
    	st.close();
    }
    ```
    
    ```sql
    SELECT * FROM CUSTOMER WHERE LOGIN_ID = 'oraking'
    SELECT * FROM CUSTOMER WHERE LOGIN_ID = 'javaking'
    SELECT * FROM CUSTOMER WHERE LOGIN_ID = 'tommy'
    SELECT * FROM CUSTOMER WHERE LOGIN_ID = 'karajan'
    SELECT * FROM CUSTOMER WHERE LOGIN_ID = 'oraking2222'
    ...
    ```
    
    - Statement는 **첫 수행한 Query와 완전히 일치하는 Query를 요청하는 경우에만 캐싱 데이터를 재활용할 수 있기 때문이다.**
        - 쿼리 문자열 자체를 DB로 보내기 때문이다.
- 아래와 같이 PrepareStatement로 수정하면 캐시는 다음과 같다.
    
    ```java
    public void login(String login_id) throws Exception {
    	String SQLStmt = "SELECT * FROM CUSTOMER WHERE LOGIN_ID = ?";
    	PreparedStatement st = con.prepareStatement(SQLStmt);
    	st.setString(1, login_id);
    	ResultSet rs = st.executeQuery();
    	
    	if (rs.next()) {
    		// do anything
    	}
    
    	rs.close();
    	st.close();
    }
    ```
    
    ```sql
    SELECT * FROM CUSTOMER WHERE LOGIN_ID = :1
    ```
    
    - 바인딩 변수를 사용하여 지정하기 때문에 캐시에는 해당 쿼리문 하나만 지정된다.
    - PrepareStatement 방식은 Statement 방식과 달리 파라미터를 바인딩하기 때문에 기존 컴파일된 Query를 재활용할 수 있다.

## JPA, MyBatis PreparedStatement 사용 여부

[[보안] Spring 에서 SQL Injection 방어 그리고 원리 (feat. Prepared Statement, JPA, MyBatis)](https://developer-ping9.tistory.com/545)

- JPA Hibernate는 기본값으로 PreparedStatement를 사용한다.
- MyBatis는 구현에 따라 다른데, 쿼리문에 `${}`를 사용하면 Statement, `#{}`를 사용하면 PreparedStatement를 사용한다.

## 참고 자료

- https://ittrue.tistory.com/250
- https://shs2810.tistory.com/18
- [https://velog.io/@ragnarok_code/DataBase-Statement와-Prepared-Statement-차이점](https://velog.io/@ragnarok_code/DataBase-Statement%EC%99%80-Prepared-Statement-%EC%B0%A8%EC%9D%B4%EC%A0%90)
- https://shuu.tistory.com/129
- https://developer-ping9.tistory.com/545
- 데이터베이스프로그래밍 강의자료
- 친절한 SQL 튜닝 1장
    - [https://github.com/yu-heejin/kind-SQL-tuning/blob/main/1장/1.2_SQL_공유_및_재사용.md](https://github.com/yu-heejin/kind-SQL-tuning/blob/main/1%EC%9E%A5/1.2_SQL_%EA%B3%B5%EC%9C%A0_%EB%B0%8F_%EC%9E%AC%EC%82%AC%EC%9A%A9.md)