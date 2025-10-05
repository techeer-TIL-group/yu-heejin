## COUNT(*)

- null 값인 컬럼도 포함한다.
- row의 개수를 카운트한다.
- 데이터를 확인하지 않고 검색된 행의 개수만 반환하기 때문에 속도가 빠르다.

## COUNT(1)

- null 값인 컬럼도 포함하여 계산한다.
- `COUNT(*)` 와 결과가 같고, 서버 내부 동작이 좀 더 빠르게 동작한다.

## COUNT(column)

- null인 경우를 제외하고 카운트한다.
- 실제 데이터를 확인하며 NULL check 하기 때문에 속도가 느리다.

# 참고 자료

- https://ryean.tistory.com/10
- https://becomeanexpert.tistory.com/47
- [https://velog.io/@qwerty1434/COUNT와-COUNTcolumn의-성능비교](https://velog.io/@qwerty1434/COUNT%EC%99%80-COUNTcolumn%EC%9D%98-%EC%84%B1%EB%8A%A5%EB%B9%84%EA%B5%90)