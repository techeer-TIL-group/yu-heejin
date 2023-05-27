## Promise.all의 문제점

- `Promise.all([ promise1, promise2, … ])`의 형태로 사용된다.
- 배열로 받은 모든 Promise가 fulfill된 후, 모든 프로미스의 반환 값을 배열에 넣어 반환한다.
- 만약 배열에 있는 Promise들 중 하나라도 reject 될 경우, 성공한 Promise 응답은 모두 무시된 채 바로 오류를 발생시킨다.

### 예시

- 다음과 같이 비동기 api로 서버에 요청을 보낸다고 가정하자.
    
    ```jsx
    const req1 = axios.post('서버주소', { obj1 });
    const req2 = axios.post('서버주소', { obj2 });
    const req3 = axios.post('서버주소', { obj3 });
    const req4 = axios.post('서버주소', { obj4 });
    const req5 = axios.post('서버주소', { obj5 });
    
    Promise.all([req1, req2, req3, req4, req5])
       .then((result) => console.log(result))
       .catch((err) => console.log(err));
    ```
    
    - 위처럼 한번에 매우 많은 Request를 서버에 날리면 서버에 과부하를 줄 수 있다.
    - 만약 나머지 요청은 문제없이 성공했으나, req3 요청만 오류가 생겨 reject 될 경우, 재요청을 보낼 때 req1부터 모든 요청을 다시 한꺼번에 보낸다.
    - 즉, **실패한 요청만 보내는 것이 아닌 모든 요청을 보내 비효율적으로 작업을 처리하고 있는 것이다.**

## Promise.allSettled

<img width="647" alt="스크린샷 2023-05-27 오후 5 33 41" src="https://github.com/yu-heejin/yu-heejin/assets/96467030/d2a53672-845c-428f-a6fa-480a08a65e19">

- `Promise.all`과 달리 `Promise.allSettled`는 여러 프로미스를 병렬적으로 처리하되, 하나의 프로미스가 실패해도 무조건 이행한다.
- `Promise.allSettled([ promise1, promise2, … ])`의 형태로, `Promise.all`과 동일한 형태로 실행되지만 반환값은 매우 다르다.
- 배열로 받은 모든 프로미스의 fulfilled, reject 여부와 상관없이, **전부 완료만 되었다면 해당 프로미스들의 결과를 배열로 반환한다.**
    - 성공 시 status 필드에 fulfilled된 값을 넣고, 실패하면 reject값을 넣는다.

## 참고 자료

- [https://inpa.tistory.com/entry/JS-📚-더이상-Promiseall-쓰지말고-PromiseallSettled-사용하자](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%8D%94%EC%9D%B4%EC%83%81-Promiseall-%EC%93%B0%EC%A7%80%EB%A7%90%EA%B3%A0-PromiseallSettled-%EC%82%AC%EC%9A%A9%ED%95%98%EC%9E%90)