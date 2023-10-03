## ON DUPLICATE KEY UPDATE vs REPLACE INTO

- 두 쿼리 모두 PK와 중복되는 경우 update, 아니라면 insert
- 단, **ON DUPLICATE KEY UPDATE는 update를 수행**하고, **REPLACE INTO의 경우 중복될 경우 해당 데이터를 삭제하고 다시 추가**한다.
- 따라서 **REPLACE INTO를 사용하면 데이터 자체는 update된 것처럼 보여도 실제 데이터에는 완전히 다른 데이터가 들어가게 된다.**
- **성능면에서도 ON DUPLICATE KEY UPDATE가 훨씬 빠르다!**

## 참고 자료

- https://multifrontgarden.tistory.com/144
- https://stackoverflow.com/questions/9168928/what-are-practical-differences-between-replace-and-insert-on-duplicate-ke
- http://jason-heo.github.io/mysql/2014/03/05/manage-dup-key2.html
- https://medium.com/@dotbox/why-using-replace-into-might-be-a-bad-idea-eafd250e787b