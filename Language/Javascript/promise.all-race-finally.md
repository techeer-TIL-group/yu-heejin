## Promise.all()

```jsx
const promise1 = Promise.resolve(3);
const promise2 = 42;
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'foo1');
});
const promise4 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'foo2');
});

Promise.all(
  [promise1, promise2, promise3, promise4]).then((values) => {
  console.log(values);
});
//output: Array [3, 42, "foo1","foo2"]
```

- 처리하고자 하는 Promise를 배열로 담아 `Promise.all`에 인자로 전달하면 **배열에 있는 모든 Promise들이 동시에 실행**된다.
- 단, **배열 요소 중 어느 하나라도 Reject가 발생하면 모든 작업이 Reject된다.**

## Promise.race()

```jsx
var p1 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('하나'), 1000);
});
var p2 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('둘'), 2000);
});
var p3 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('셋'), 3000);
});
var p4 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('넷'), 4000);
});

Promise.race([p1, p2, p3, p4, p5])
.then(value => {
  console.log(value);
});

// 콘솔 출력값:
// "하나"
```

- 가장 빨리 응답 받은 결과만 Resolve한다.
- Promise들끼리 경주해서 **가장 빨리 도착한 Promise의 result만 `then()` 구문으로 넘어갈 수 있다.**
- 오류도 마찬가지로 가장 빨리 발생한 오류만 `catch()`구문으로 넘어간다.
    
    ```jsx
    var p1 = new Promise((resolve, reject) => {
      setTimeout(() => reject('하나'), 1000);
    });
    var p2 = new Promise((resolve, reject) => {
      setTimeout(() => reject('둘'), 2000);
    });
    var p3 = new Promise((resolve, reject) => {
      setTimeout(() => reject('셋'), 3000);
    });
    var p4 = new Promise((resolve, reject) => {
      setTimeout(() => reject('넷'), 4000);
    });
    
    Promise.race([p1, p2, p3, p4, p5])
    .then(value => {
      console.log(value);
    })
    .catch(error=>{
      console.log("error",error);
    });
    
    // 콘솔 출력값:
    // error 하나
    ```
    

## Promise.finally()

```jsx
let isLoading = true;

//fetch는 promise 객체를 반환하는 함수
fetch(myRequest).then(function(response) {
    var contentType = response.headers.get("content-type");
    if(contentType && contentType.includes("application/json")) {
      return response.json();
    }
    throw new TypeError("Oops, we haven't got JSON!");
  })
  .then(function(json) { /* process your JSON further */ })
  .catch(function(error) { console.log(error); })
  .finally(function() { isLoading = false; });
```

- Promise 객체를 반환한다.
- Promise가 처리되면 **충족되거나 거부되는지 여부에 관계없이 지정된 콜백 함수가 실행된다.**
- Promise가 성공적으로 수행되었는지 거절되었는지에 관계없이 Promise가 처리된 후 **코드가 무조건 한 번은 실행되는 것을 보장한다.**

## 참고 자료

- https://velog.io/@chaerin00/Promise.all-Promise.race-Promise.finally