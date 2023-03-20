> Javascript에서는 과거 callback 함수를 통해 비동기를 구현했지만, ***요즘에는 Promise 객체를 반환하게 하여 async/await로 작업이 완료되면 다음 로직이 진행되게끔 지연시키는 방식***을 통해 비동기를 구현한다.
> 
- Node는 7.6 버전부터 async/await를 별도의 도구 없이도 지원하기 시작했다.

## Async/await란?

- async/await는 **비동기 코드를 작성하는 새로운 방법**이다.
    - 이전에는 비동기 코드를 작성하기 위해 callback이나 promise를 사용해야 했다.
- async/await는 실제로는 **최상위에 위치한 promise에 대해서 사용**하게 된다.
    - async/await는 plain callback이나 node callback과 함께 사용할 수 없다.
- promise처럼 non-blocking이다.
- async/await는 **비동기 코드의 겉모습과 동작을 좀 더 동기 코드와 유사하게 만들어준다!**

## Promise 객체 반환

- Promise 객체를 반환하게끔 작성한 함수를 호출할 땐 함수명 앞에 await 키워드를 붙이고 호출하면 해당 작업이 완료된 후 코드가 진행된다.
- sudo code
    
    ```jsx
    // "샘플함수"는 비동기 작업 함수를 호출하여 콘솔에 기록하는 함수
    
    샘플함수 {
      const 작업결과 = 비동기 작업 함수 실행 // 시간이 걸림
      console.log(작업결과)
    }
    ```
    
    - 위 작업을 비동기처리 없이 구현하면 작업 결과는 undefined가 출력된다.
    - 즉, **비동기 작업을 수행하는 함수가 결과값을 반환해 작업 결과에 할당하기 전에 console.log 함수가 실행되었기 때문**이다.
    - 이렇게 **Promise를 반환하는 함수 반환값을 사용하기까지 코드를 지연시키기 위해 async, await를 사용한다.**

### async/await를 활용한 방식

```jsx
const sampleFunc = async () => {
	const result = await asyncFunc();   // asyncFunc 함수는 Promise 객체를 반환한다.
	console.log(result);
}
```

- **Promise 객체를 반환하는 함수 asyncFunc 앞에 await를 붙이고, 이를 포함하는 함수 앞에 async를 붙여준다.**
- await 키워드를 사용하려면 반드시 async 키워드로 선언된 함수 내부여야 한다.
- 위와 같이 코드를 작성하면 await 키워드가 console.log 함수의 실행을 지연시킴을 알 수 있다.

### async/await에서의 예외처리

```jsx
const sampleFunc = async () => {
	try {
		const result = await asyncFunc();
		console.log(result);
	} catch (error) {
		console.error(error);
	}
}
```

- 만약 asyncFunc에서 reject가 발생했을 경우 catch에서 해당 내용을 잡아 콘솔에 찍어낸다.

### then을 활용한 방식

```jsx
const sampleFunc = () => {
	asyncFunc().then(result => console.log(result))
}
```

- async/await를 이용한 방식 이전에는 then을 사용하는 방식이 존재했다.
- 얼핏 보면 코드가 깔끔해보이지만, 사실 depth가 한 단계 더 들어가면서 콜백 지옥처럼 보인다.
- 실제 코드를 다룰 때 then을 활용한 방식은 가독성이 떨어질 수 있다.
- 콜백 지옥에서 벗어나기 위해 Promise가 도입되었는데, 오히려 콜백 지옥같은 들여쓰기 지옥을 경험할 수도…

### then 예외처리

```jsx
// 1.
const sampleFunc = () => {
	asyncFunc()
				.then(result => console.log(result))
				.catch(err => console.log(err));
}

// 2.
const sampleFunc2 = () => {
	asyncFunc()
				.then(
						result => console.log(result),
						error => console.log(error)
	)
}
```

- 예외처리 과정에서도 async/await를 사용하는 것이 더 좋다.

## async/await 사용을 권장하는 이유

### 간결함과 깔끔함

- then을 사용할 때보다 코드 양이 줄어들고, response를 해결하기 위한 비동기 함수를 만들 필요도 없다.
- 또한, 코드의 nesting도 피할 수 있다.

### 에러 핸들링

- async/await는 동기와 비동기 에러 모두 try/catch를 통해 처리한다.
    - try/catch는 오래된 방법이지만 좋은 접근 방식이다.
- Promise를 사용한 다음 코드를 살펴보자.
    
    ```jsx
    const makeRequest = () => {
      try {
        getJSON()
          .then(result => {
            // this parse may fail
            const data = JSON.parse(result)
            console.log(data)
          })
          // uncomment this block to handle asynchronous errors
          // .catch((err) => {
          //   console.log(err)
          // })
      } catch (err) {
        console.log(err)
      }
    }
    ```
    
    - try/catch는 JSON.parse가 실패하더라도 동작하지 않을 것이다.
        - promise 안 쪽에서 발생한 에러이기 때문이다.
    - 우리는 promise 상에서 .catch를 호출해야하며, 에러를 처리하는 코드는 중복이 될 것이다.
- 위 코드를 async/await로 변경하면 다음과 같다.
    
    ```jsx
    const makeRequest = async () => {
      try {
        // this parse may fail
        const data = JSON.parse(await getJSON())
        console.log(data)
      } catch (err) {
        console.log(err)
      }
    }
    ```
    
    - 위처럼 사용하면 catch 블록이 에러를 잡을 수 있다.

### 분기

```jsx
const makeRequest = () => {
  return getJSON()
    .then(data => {
      if (data.needsAnotherRequest) {
        return makeAnotherRequest(data)
          .then(moreData => {
            console.log(moreData)
            return moreData
          })
      } else {
        console.log(data)
        return data
      }
    })
}
```

- 메인 promise에서 마지막 결과가 나오기까지 많은 nesting(6 단계)과 대괄호, return문들이 필요하다.
- 다음 코드를 async/await를 사용하면 다음과 같다.
    
    ```jsx
    const makeRequest = async () => {
      const data = await getJSON()
      if (data.needsAnotherRequest) {
        const moreData = await makeAnotherRequest(data)
        console.log(moreData)
        return moreData
      } else {
        console.log(data)
        return data    
      }
    }
    ```
    
    - 가독성이 증가한다.

### 중간 값(Intermediate value)

```jsx
const makeRequest = () => {
  return promise1()
    .then(value1 => {
      // do something
      return promise2(value1)
        .then(value2 => {
          // do something          
          return promise3(value1, value2)
        })
    })
}
```

- promise1을 호출하고 여기서 return된 값을 사용해 promise2를 호출하고, promise3을 호출하기 위해 두 개의 promise들의 결과를 사용한다.
- 만약 promise3이 value1을 요구하지 않았다면 promise들의 nesting을 조금 줄이기 쉬웠을 것이다.
- 위와 같은 코드를 value1, value2를 Promise.all로 묶어서 nesting을 피할 수 있다.
    
    ```jsx
    const makeRequest = () => {
      return promise1()
        .then(value1 => {
          // do something
          return Promise.all([value1, promise2(value1)])
        })
        .then(([value1, value2]) => {
          // do something          
          return promise3(value1, value2)
        })
    }
    ```
    
    - 코드의 의미를 희생시킨다. value1, value2를 배열로 묶는 이유는 오로지 nesting을 피하기 위함이기 때문이다.
- 위 로직을 async/await를 사용하면 더 간단하다.
    
    ```jsx
    const makeRequest = async () => {
      const value1 = await promise1()
      const value2 = await promise2(value1)
      return promise3(value1, value2)
    }
    ```
    

### error stack

```jsx
const makeRequest = () => {
  return callAPromise()
    .then(() => callAPromise())
    .then(() => callAPromise())
    .then(() => callAPromise())
    .then(() => callAPromise())
    .then(() => {
      throw new Error("oops");
    })
}

makeRequest()
  .catch(err => {
    console.log(err);
    // output
    // Error: oops at callAPromise.then.then.then.then.then (index.js:8:13)
  })
```

- promise 체인에서 반환되는 error stack은 어디서 에러가 발생했는지에 관해 어떤 힌트도 주지 않는다.
- 다음처럼 async/await 패턴을 사용하면 error를 포함한 함수를 가리킨다.
    
    ```jsx
    const makeRequest = async () => {
      await callAPromise()
      await callAPromise()
      await callAPromise()
      await callAPromise()
      await callAPromise()
      throw new Error("oops");
    }
    
    makeRequest()
      .catch(err => {
        console.log(err);
        // output
        // Error: oops at makeRequest (index.js:7:9)
      })
    ```
    
    - 상용 서버에서 error log를 파악할 땐 꽤나 유용하다.

### 디버깅

- promise를 디버깅할 땐 다음과 같은 고통(?)이 따른다.
    1. return되는 arrow function들에 breakpoint를 잡을 수 없다.
        
        ![Untitled](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*1rlNjMeVyQJhA0EdgJL1sw.png)
        
    2. .then 블록 안에 breakpoint를 잡고 step-over와 같은 debugshortcuts를 사용하게 되면 debugger는 .then을 따라서 움직이지 않는다.
        1. 디버그 도구가 동기화된 코드만 따라서 움직이기 때문이다.
- async/await를 사용하면 arrow function을 많이 사용할 필요가 없고, 디버그 도구는 동기화된 코드를 실행하는 것과 다름 없이 동작한다.
    
    ![Untitled](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*9LNQZrYsro5S862Bg1UWBw.png)
    

## then은 사용하지 말아야 하나요?

- then은 이러한 **비동기가 연속적으로 일어나는 상황에서 각 Promise 객체마다 다른 로직으로 체이닝을 구성하려고할 때** 유용하게 사용할 수 있다.

## 참고 자료

- [https://one-it.tistory.com/entry/Promise-정리-asyncawait-사용법-then과의-차이](https://one-it.tistory.com/entry/Promise-%EC%A0%95%EB%A6%AC-asyncawait-%EC%82%AC%EC%9A%A9%EB%B2%95-then%EA%B3%BC%EC%9D%98-%EC%B0%A8%EC%9D%B4)
- [https://medium.com/@constell99/자바스크립트의-async-await-가-promises를-사라지게-만들-수-있는-6가지-이유-c5fe0add656c](https://medium.com/@constell99/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-async-await-%EA%B0%80-promises%EB%A5%BC-%EC%82%AC%EB%9D%BC%EC%A7%80%EA%B2%8C-%EB%A7%8C%EB%93%A4-%EC%88%98-%EC%9E%88%EB%8A%94-6%EA%B0%80%EC%A7%80-%EC%9D%B4%EC%9C%A0-c5fe0add656c)