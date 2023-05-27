## async-await

- `async-await`는 비동기 동작(promise)의 상태가 완료될 때까지 기다린 후, 다음 코드를 순차적으로 읽어 나가며 실행한다.
- 테스크의 순서가 보장될 때 사용하면 좋지만, 각각의 테스크가 서로 연관성이 없는 작업일 경우에는 앞의 작업이 끝날 때까지 기다려야하는 불편함이 있다.

## Promise.all

- **여러 비동기 동작을 하나로 묶어 하나의 Promise처럼 관리**할 수 있게 해준다.
- 즉, 여러 비동기 테스크를 동시에(병렬적으로) 실행하고, 가장 마지막 테스크가 완료될 때 완료 상태의 Promise를 반환한다.
- 단, 테스크의 순서가 중요한 경우에는 절대 사용하지 않는다.
- **여러 Promise들 중 하나라도 reject를 반환하거나 오류가 발생할 경우 모든 Promise들을 reject 시킨다.**

### 구문

```jsx
Promise.all(iterable);
```

```jsx
const promise1 = Promise.resolve(3);
const promise2 = 42;
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'foo');
});

Promise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values);
});
// Expected output: Array [3, 42, "foo"]
```

```jsx
async function sampleFunc(): Promise<void> {
    // time start
    console.time('promise all example');

    // first promise
    const fetchNameList = async (): Promise<string[]> => {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
              const result: any = ['Jack', 'Joe', 'Beck'];
              resolve(result);
            }, 300);
        });
    };

    // second promise
    const fetchFruits = async (): Promise<string[]> => {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
              const result: any = ['Apple', 'Orange', 'Banana'];
              resolve(result);
            }, 200);
        });
    };

    // third promise
    const fetchTechCompanies = async (): Promise<string[]> => {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
              const result: any = ['Apple', 'Google', 'Amazon'];
              resolve(result);
            }, 400);
        });
    };

    // promise all
    const result: any[] = await Promise.all([
        fetchNameList(),
        fetchFruits(),
        fetchTechCompanies(),
    ]);

    // time end
    console.timeEnd('promise all example');

    console.log('%j', result);
}
```

- `iterable`: Array와 같이 순회 가능한 객체

## 참고 자료

- [https://merrily-code.tistory.com/214](https://merrily-code.tistory.com/214)
- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/all](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)
- [https://velog.io/@jay2u8809/Promise.all-로-비동기-처리를-구현해-보자](https://velog.io/@jay2u8809/Promise.all-%EB%A1%9C-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%B2%98%EB%A6%AC%EB%A5%BC-%EA%B5%AC%ED%98%84%ED%95%B4-%EB%B3%B4%EC%9E%90)