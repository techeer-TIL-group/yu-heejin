## MySQL에서의 검색 기능

- 일반적으로 mysql에서 텍스트 문자열 검색 시 like같은 연산자를 사용하면 원하는 텍스트를 필터링하여 조회할 수 있다.
    
    ```sql
    SELECT * FROM FulltextTbl WHERE description LIKE '%남자%';
    ```
    
    - 하지만 LIKE를 사용할 경우 검색할 텍스트 내용이 많으면 성능이 떨어진다.
    - 이와 같은 문제를 ‘**전체 텍스트 검색**’ 기능이 해결해준다.
- 전체 텍스트 검색은 **첫 글자 뿐만 아니라 중간의 단어나 문장으로도 인덱스를 생성해주기 때문에 전체 텍스트 인덱스를 통해 검색 결과를 얻을 수 있다.**
- 전체 텍스트 인덱스는 신문 기사와 같이 **텍스트로 이루어진 문자열 데이터의 내용을 가지고 생성한 특수한 인덱스 종류**이다.
    - 따라서 내용이 긴 텍스트를 따로 인덱스하여 관리할 수 있다.

## 풀텍스트 인덱스(전체 텍스트 인덱스)

- 전체 텍스트 검색은 **긴 문자의 텍스트 데이터를 빠르게 검색**하기 위한 MySQL의 부가적인 기능이다.
    - 전체 텍스트도 결국 인덱스 종류의 한 가지
- **단어 또는 구문에 대한 검색**을 의미하며, 게시물 내용이나 제목 등과 같이 문서 내 키워드를 검색할 수 있다.
- MYSQL에서의 FULLTEXT 타입의 인덱스
- 일반 인덱스와 달리 풀텍스트 인덱스는 긴 문장 전체를 대상으로 인덱싱하며, **InnoDB와 MyISAM 테이블만 지원**한다.
    - 또한, **char, varchar, text 타입 문자만 인덱싱**이 가능하다.
    - 여러개의 열에 풀텍스트 인덱스를 지정할 수 있다.

### MATCH … AGAINST

- 풀텍스트 검색은 MATCH AGAINST 구문을 사용해 수행된다.
- MATCH는 쉼표로 구분되며, **검색할 열**을 지정한다.
- AGAINST는 **검색할 문자열과 수행할 검색 유형**을 나타내는 **search modifier**를 사용한다.
    
    ```sql
    search_modifier:
      {
           IN NATURAL LANGUAGE MODE
         | IN NATURAL LANGUAGE MODE WITH QUERY EXPANSION
         | IN BOOLEAN MODE
         | WITH QUERY EXPANSION
      }
    ```
    

## 풀텍스트 인덱스 사용법

### 생성 방식

```sql
CREATE TABLE table_name(
	...,
	column_name data_type
	...,
	FULLTEXT index_name (column_name)
);
```

```sql
CREATE FULLTEXT INDEX index_name ON table_name (column_name);
```

```sql
ALTER TABLE table_name ADD FULLTEXT (column_name);
```

### 삭제

```sql
ALTER TABLE table_name DROP INDEX FULLTEXT (column_name);
```

```sql
drop index idx_description on FulltextTbl;
```

### 조회

```sql
SHOW INDEX FROM table_name   // 전체 인덱스 확인 쿼리
```

## 전체 텍스트 검색

- 전체 텍스트 검색은 일반 인덱스와는 다른 점이 있다.
    - 쿼리의 일반 **WHERE 절에 MATCH(), AGAINST()와 같은 특수한 메서드**를 사용한다.

### 자연어 검색(default)

- 특별히 옵션을 지정하지 않거나 뒤에 in natural language mode를 붙이면 자연어 검색을 시작한다.
- 자연어 검색은 **정확한 단어**를 검색한다.
- **문자열을 단어 단위(토큰)으로 분리**한 후, **해당 단어 중 하나라도 포함되는 행**을 찾는다.
- 예를 들어, newspaper라는 테이블에서 article 열에 풀텍스트 인덱스가 생성되어 있고, 영화라는 단어가 들어간 기사를 찾으려면 다음과 같이 입력한다.
    
    ```sql
    SELECT * FROM newspaper WHERE MATCH(article) AGAINST('영화');
    SELECT * FROM newspaper WHERE MATCH(article) AGAINST('영화' in natural language mode);
    SELECT * FROM newspaper WHERE MATCH(article) AGAINST('영화 배우'); 
    -- ‘영화’ 또는 ‘배우’ 두 단어 중 하나가 포함된 기사 검색.
    ```
    
    - 하지만 위와 같이 검색할 경우 ‘영화’라는 정확한 단어만 검색되며, ‘영화는’, ‘영화가’ 등의 능동적인 검색은 불가능하다. → 불린 모드 검색
- 매치율
    - **입력된 검색어의 키워드가 얼마나 더 많이 포함되어 있는지에 따라 매치율이 결정**된다.
    - 전체 테이블의 50% 이상의 레코드가 검색된 키워드를 가지고 있으면, 해당 키워드는 검색어로서 의미가 없다고 판단해 검색 결과에서 배제한다.
    - 이때 **매치율이 계산될 땐 row 내의 고유 단어 수, 총 단어 수, 특정 단어를 포함하는 row 수 등을 기준으로 계산된다.**
    - 검색 결과는 가장 높은 관련성을 가진 결과부터 자동 정렬되는데, 아래와 같은 조건에 한해 자동 정렬한다.
        1. ORDER BY 절이 없어야 한다.
        2. 검색은 테이블 검색이 아닌 FULLTEXT Index를 사용해 수행해야 한다.
        3. 쿼리가 테이블을 조인하는 경우, FULLTEXT Index는 조인에서 가장 왼쪽에 있는 non-constanct 테이블이여야 한다.
- 대소문자
    - 자연어 검색은 기본적으로 대소문자를 구분하지 않고 검색한다.
    - 대소문자를 구분하는 전체 텍스트 검색을 수행하려면 binary collaction을 사용할 수 있다.
- 무시되는 검색어
    - 길이가 기준보다 짧거나 중지 단어의 경우 풀텍스트 검색에서 무시된다.

### 불린 모드 검색

- LIKE 연산자에서 %를 쓰듯 불린 모드 검색 기능을 사용한다.
- 불린 모드 검색이란 **단어나 문장이 정확히 일치하지 않아도 검색할 수 있는 것을 의미**한다.
- 문자열을 단어 단위로 분리한 후, 추가적인 검색 규칙을 적용해 단어가 포함되는 행을 찾는다.
- 뒤에 IN BOOLEAN MODE 옵션을 붙여주면 적용된다.
- 불린 모드 검색의 연산자
    1. +
        1. 검색 필수 연산자
        2. 아래의 쿼리문은 영화를 찾되, 반드시 액션이 들어가 있는 컬럼을 찾는다.
            
            ```sql
            SELECT * FROM newspaper
            WHERE MATCH(article) AGAINST('영화 +액션' IN BOOLEAN MODE);
            ```
            
    2. -
        1. 검색 제외 연산자
        2. 아래의 쿼리문은 영화를 찾되 액션은 들어가있지 않은 컬럼을 찾는다.
            
            ```sql
            SELECT * FROM newspaper
            WHERE MATCH(article) AGAINST('영화 -액션' IN BOOLEAN MODE);
            ```
            
    3. ~
        1. -보다 부드러운 검색 부정 연산자
        2. 아래의 쿼리문은 ‘영화’를 찾되, ‘액션’이 없는 열이 아닌 ‘액션’이 있는 열이 아래 순위로 가도록 찾는다.
            
            ```sql
            SELECT * FROM newspaper
            WHERE MATCH(article) AGAINST('영화 ~액션' IN BOOLEAN MODE);
            ```
            
    4. *
        1. 부분 검색 연산자
        2. ‘영화를’, ‘영화가’, 영화는’과 같이 능동적으로 찾아준다.
            
            ```sql
            SELECT * FROM newspaper
            WHERE MATCH(article) AGAINST('영화*' IN BOOLEAN MODE);
            ```
            
    5. “
        1. 부분 검색 “” 안에 있는 구문과 정확히 동일한 철자의 구문
        2. ‘재밌는 영화’, ‘재밌는 영화가’ 등
            
            ```sql
            SELECT * FROM newspaper
            WHERE MATCH(article) AGAINST("재밌는 영화" IN BOOLEAN MODE);
            ```
            
            - 단, ‘재밌는 한국 영화’, 재밌는 할리우드 영화’와 같은 단어는 불가능하다.
- 사용 예시
    
    ```sql
    
    -- ex) 영화가 영화는 영화를
    select * from newspaper
    where match(article) against ('영화*' in boolean mode);
    
    -- 정확히 '영화 배우' 단어가 들어있는 기사 내용 검색
    select * from newspaper
    where match(article) against('영화 배우' in boolean mode);
    
    -- '영화 배우' 단어가 들어 있는 기사 중에서 '공포' 내용이 들어간 결과
    select * from newspaper
    where match(article) against('영화 배우 +공포' in boolean mode);
    
    -- '영화 배우' 단어가 들어 있는 기사 중에서 '남자' 내용은 검색에서 제외
    select * from newspaper
    where match(article) against('영화 배우 -남자' in boolean mode);
    
    -- '여자' 또는 '남자' 가 있는 경우 검색
    SELECT *, MATCH(description) AGAINST('남자* 여자*' IN BOOLEAN MODE) AS 점수 
    FROM FulltextTbl WHERE MATCH(description) AGAINST('남자* 여자*' IN BOOLEAN MODE);
    
    -- 남자, 여자가 모두 들어있는 검색
    SELECT * FROM FulltextTbl 
    WHERE MATCH(description) AGAINST('+남자*+여자*' IN BOOLEAN MODE);
    
    -- 님자가 포함된 행을 찾고 여자가 포함되어 있다면 상위행으로 찾기
    SELECT * FROM FulltextTbl 
    WHERE MATCH(description) AGAINST('+남자* 여자*' IN BOOLEAN MODE);
    
    -- 여자가 포함된 행을 찾고 남자가 있으면 상위 행으로 찾기
    SELECT * FROM FulltextTbl 
    WHERE MATCH(description) AGAINST('남자* +여자*' IN BOOLEAN MODE);
    
    -- 딱 '남자' 가 포함된 행중에서 여자가 포함된 행만 찾기
    SELECT * FROM FulltextTbl 
    WHERE MATCH(description) AGAINST('남자 +여자' IN BOOLEAN MODE);    
    
    -- 딱 '남자' 가 포함된 행중에서 여자가 있으면 상위행으로, 남자만 있는것도 찾기
    SELECT * FROM FulltextTbl 
    WHERE MATCH(description) AGAINST('+남자 여자' IN BOOLEAN MODE);
    
    -- 남자로 검색된 영화 중에서 여자가 들어간 영화는 제거하는 검색
    SELECT * FROM FulltextTbl 
    WHERE MATCH(description) AGAINST('남자* -여자*' IN BOOLEAN MODE);
    ```
    

### 쿼리 확장 검색

```sql
select * from tbl_full where match(content) against('내용*' WITH QUERY EXPANSION);
```

- 자연어 검색을 확장한 내용으로, 2단계에 걸쳐서 검색을 수행한다.
    1. 첫 단계에서는 자연어 검색을 수행한다.
    2. 그 다음 첫 번째 검색의 결과에 매칭된 행을 기반으로 검색 문자열을 재구성하여 두 번째 검색을 수행한다.
        1. 이는 1단계 검색에서 사용한 단어와 연관성이 있는 단어가 1단계 검색에 매칭된 결과에 나타난다는 가정을 전제로 한다.
- 쿼리 확장 검색은 IN NATURAL LANGUAGE MODE WITH QUERY EXPANSION 혹은 WITH QUERY EXPANSION 한정자를 사용한다.
- 일반적으로 검색 구문이 아주 짧을 때 유용하다.

## 검색 단어 제한 수 풀기

- MySQL은 기본값으로 검색 가능 단어의 숫자가 3이다.
    - 즉, 3글자 이상만 전체 텍스트 검색이 가능하고 두 글자 단어는 안된다는 의미
    - 따라서 검색 가능 단어의 숫자를 확인하고 mysql 설정에서 수정해야한다.

```sql
-- 몇 글자 이상부터 검색 가능한지 확인
SHOW VARIABLES LIKE 'innodb_ft_min_token_size';
```

## 중지 단어(stopwords)

- 풀텍스트 인덱스는 긴 문장에 대해 인덱스를 생성하기 때문에 그 양이 커질 수밖에 없다.
    - 따라서, **실제로 검색에서 무시할만 한 단어들은 풀텍스트 인덱스로 생성하지 않는 편이 좋다.**

### 중지 단어 예시

- 다음과 같은 문장이 있다고 가정하자.
    
    ```sql
    이번 선거는 아주 중요한 행사이므로 모두 꼭 참여 바랍니다.
    ```
    
- 위 문장을 풀텍스트 인덱스로 단어를 끊어 만든다면 다음과 같다.
    
    ```sql
    이번 / 선거는 / 아주 / 중요한 / 행사이므로 / 모두 / 꼭 / 참여 / 바랍니다
    ```
    
    - 여기서 ‘이번’, ‘아주’, ‘모두’, ‘꼭’과 같은 부사는 사용자가 굳이 검색할 이유가 없으므로 인덱싱에서 제외하는게 효율적이다.
    - 이처럼 검색에 필요없는 단어를 중지 단어라고 한다.

### 중지 단어 만들기

- 전체 텍스트 인덱스 단어 확인
    
    ```sql
    --db와 테이블 이름은 소문자 써야된다.
    set global innodb_ft_aux_table = '디비명/테이블명';
    
    --테이블에서 만들어진 전체텍스트 인덱스 보기 (필요없는 검색 단어가 있음)
    select word, doc_count, doc_id, position from information_schema.innodb_ft_index_table ;
    ```
    
- 중지 단어 생성
    
    ```sql
    -- 중지단어 만들기전에 만들어놓은 풀텍스트 인덱스는 삭제
    drop index idx_all on FulltextTbl ;
    
    -- 중지단어를 위한 테이블 만들기
    -- 테이블의 데이터는 반드시 value , 타입은 varchar -> 약속
    CREATE TABLE user_stopword (value VARCHAR(30));
    
    --중지 단어 만들기
    insert into user_stopword values ('그는'), ('그리고'), ('극에') ; 
    
    -- 중지 단어 테이블에 지금 만들 테이블을 추가하는 작업
    -- 그러면 이제 user_stopword 테이블에 들어있는 단어로는 풀텍스트 인덱스를 만들지 않게 된다
    set global innodb_ft_server_stopword_table = '디비명/user_stopword'; -- 모두 소문자
    
    -- 만든 테이블이 들어갔는지 확인
    show global variables like 'innodb_ft_server_stopword_table' ;
    
    -- 중지단어 테이블 만들었으니, 이제 다시 풀텍스트 인덱스 생성.
    create Fulltext index idx_description on FulltextTbl ( description );
    
    -- 그러면 중지단어에 등록한 단어를 제외한 인덱스 단어들이 생성됨 (갯수가 줄어들음)
    select word, doc_count, doc_id, position 
    from information_schema.innodb_ft_index_table ;
    ```
    

## 참고 자료

- [https://inpa.tistory.com/entry/MYSQL-📚-풀텍스트-인덱스Full-Text-Index-사용법](https://inpa.tistory.com/entry/MYSQL-%F0%9F%93%9A-%ED%92%80%ED%85%8D%EC%8A%A4%ED%8A%B8-%EC%9D%B8%EB%8D%B1%EC%8A%A4Full-Text-Index-%EC%82%AC%EC%9A%A9%EB%B2%95)
- [https://gngsn.tistory.com/162](https://gngsn.tistory.com/162)