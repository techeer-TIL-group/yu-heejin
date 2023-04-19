## req.param

- 주소에 포함된 변수를 담는다.
- `https://okky.com/post/12345` 라는 주소가 있다면 `12345`를 담는다.
- `Path Variable`이라고 부르기도 한다.
- 원하는 조건의 데이터들 혹은 하나의 데이터에 대한 정보를 받아올 때 적절하다.

## req.query

- 주소 바깥에 있는 ? 이후의 변수를 담는다.
- `https://okky.com/post?q=Node.js` 라는 주소가 있다면 `Node.js`를 담는다.
- `key=value`로 이루어져 있으며 &로 연결할 수 있다.
- filtering, sorting, searching에 적절하다.
- `Query Parameter`라고 부르기도 한다.

## req.body

- 주소에서는 확인할 수 없는 요청 데이터

## 참고 자료

- [https://medium.com/@bouncewind0105/request-param-query-body-의-차이점-2e7e4fddd8b9](https://medium.com/@bouncewind0105/request-param-query-body-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90-2e7e4fddd8b9)
- [https://velog.io/@montoseon/param-vs-query-vs-body](https://velog.io/@montoseon/param-vs-query-vs-body)
- [https://i-ten.tistory.com/m/243](https://i-ten.tistory.com/m/243)