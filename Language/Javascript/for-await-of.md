## for await of

- 반복문 내에서 일어나는 모든 비동기문을 기다려주는 구문

### 사용 예시

```jsx
const names = ['a', 'b', 'c', 'd'];

names.forEach(async (name) => {
  const result = await fetch(`https://someurl.com/names/${name}`);
  console.log(result.json());
});

console.log('모든 api 통신 완료'); // 이 부분이 forEach의 반복문 작업이 모두 끝나기 전에 실행된다.
```

- 위와 같은 api 통신을 하는 코드를 실행하면 다음과 같은 결과가 나온다.
    
    ```jsx
    $ 모든 api 통신 완료
    $ <json result 'a'...>
    $ <json result 'b'...>
    $ <json result 'c'...>
    $ <json result 'd'...>
    ```
    
- **console은 `forEach`에서 진행되는 모든 비동기작업이 끝나는 것을 기다리지 않는다!**
    - 이 때 `for await`를 사용하면 쉽게 해결할 수 있다.
        
        ```jsx
        const names = ['a', 'b', 'c', 'd'];
        
        for await (let name of names) {
          const result = await fetch(`https://someurl.com/names/${name}`);
          console.log(result.json());
        }
        
        console.log('모든 api 통신 완료'); // forEach의 반복문이 끝나기를 기다린 후 로깅을 한다.
        
        실행 결과
        $ <json result 'a'...>
        $ <json result 'b'...>
        $ <json result 'c'...>
        $ <json result 'd'...>
        $ 모든 api 통신 완료
        ```
        

## Promise.all()과의 차이점

- `Promise.all()`은 인자의 **프로미스 배열을 동시에 실행**한다. (순서 x)
- `for await of` 내의 비동기 작업은 **루프를 돌며 순차적으로 실행**된다. (순서 o)

## 참고 자료

- https://velog.io/@hiro2474/understandfor-await-of
- https://kyounghwan01.github.io/blog/JS/JSbasic/for-await-of/