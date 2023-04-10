## 자바스크립트는 동기식인가 비동기식인가?

- 동기 : **이전 작업의 실행이 끝나야** 다음 작업 실행을 시작한다.
- 비동기 : **이전 작업의 실행과 무관하게** 다음 작업을 실행한다.
    - 특정 코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 수행하는 자바스크립트의 특성
- 싱글 스레드인 자바스크립트는 기본적으로 동기식이지만, API 요청을 보낼 때 응답이올 때까지 마냥 기다리기만 할 수 없기 때문에 **비동기 처리가 필요**하다.
- 하지만 비동기 처리를 할 경우, 의도하지 않은 순서로 함수가 실행될 수 있기 때문에 원하는 부분에서 동기 방식으로 변환해줘야 한다.

### 콜백함수

- 비동기를 동기 방식으로 변환하는 방법 중 하나
- 콜백함수는 함수 안에서 또 다른 함수를 호출하는 것인데, 여러 함수를 순서대로 호출할 필요가 있을 경우 콜백 지옥을 경험하게 되는 문제점이 있다.
- 숫자 n을 파라미터로 받아와 다섯번에 걸쳐 1초마다 1씩 더해서 출력하는 작업을 setTimeout으로 구현하면 다음과 같다.
    
    ```jsx
    function increaseAndPrint(n, callback) {
      setTimeout(() => {
        const increased = n + 1;
        console.log(increased);
        if (callback) {
          callback(increased);
        }
      }, 1000);
    }
    
    increaseAndPrint(0, n => {
      increaseAndPrint(n, n => {
        increaseAndPrint(n, n => {
          increaseAndPrint(n, n => {
            increaseAndPrint(n, n => {
              console.log('끝!');
            });
          });
        });
      });
    });
    ```
    

## Promise

![Untitled](https://joshua1988.github.io/images/posts/web/javascript/promise.svg)

- 비동기를 동기 방식으로 변환하는 두 번째 방법
- 자바스크립트 비동기 처리에 사용되는 객체이다.
- Promise는 성공할 수도 있고, 실패할 수도 있다.
- **성공할 땐 resolve 함수를 호출**하고, **실패할 땐 reject 함수를 호출**한다.
    
    ```jsx
    const myPromise = new Promise((resolve, reject) => {
      // 구현..
    })
    ```
    
- Promise가 1초 뒤 성공하는 상황을 구현하면 다음과 같다.
    
    ```jsx
    const myPromise = new Promise((resolve, reject) => {
    	setTimeout(() => {
    		resolve(1);
    	}, 1000);
    });
    
    myPromise.then(n => {
    	console.log(n);
    });
    ```
    
    - `resolve`를 호출할 때 특정 값을 파라미터로 넣어주면, 이 값을 작업이 끝나고 나서 사용할 수 있다.
    - 작업이 끝나고 난 후 또 다른 작업을 해야 할 땐 Promise 뒤에 `.then(..)`을 붙여서 사용하면 된다.
- Promise가 1초 뒤 실패하는 상황을 구현하면 다음과 같다.
    
    ```jsx
    const myPromise = new Promise((resolve, reject) => {
      setTimeout(() => {
        reject(new Error());
      }, 1000);
    });
    
    myPromise.then(n => {
      console.log(n);
    })
    .catch(error => {
      console.log(error);
    });
    ```
    
    - 실패하는 상황에서는 `reject`를 사용하고, `.catch`를 통해 실패했을 시 수행할 작업을 설정할 수 있다.
- Promise를 만드는 함수는 다음과 같다.
    
    ```jsx
    function increaseAndPrint(n) {
    	return new Promise((resolve, reject) => {
    		setTimeout(() => {
    			const value = n + 1;
    			if (value === 5) {
    				const error = new Error();
    				error.name = 'ValueIsFiveError';
    				reject(error);
    				return;
    			console.log(value);
    			resolve(value);
    		}, 1000);
    	});
    }
    
    increaseAndPrint(0)
    	.then(increaseAndPrint)
    	.then(increaseAndPrint)
    	.then(increaseAndPrint)
    	.then(increaseAndPrint)
    	.then(increaseAndPrint)
    	.catch(e => {
    		console.error(e);
    	});
    ```
    
    - Promise를 사용하면 비동기 작업의 개수가 많아져도 코드의 깊이가 깊어지지 않게 된다.

## Promise의 세 가지 상태(states)

- 여기서 말하는 상태란 프로미스의 처리 과정을 의미한다.
- `new Promise()`로 프로미스를 생성하고 종료될 때까지 3가지 상태를 가진다.
    - Pending(대기): 비동기 처리 로직이 완료되지 않은 상태
    - Fulfilled(이행): 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
    - Rejected(실패): 비동기 처리가 실패하거나 오류가 발생한 상태

### Pending(대기)

- `new Promise()` 메서드를 호출하면 대기 상태가 된다.
- `new Promise()` 메서드를 호출할 때 콜백 함수를 선언할 수 있고, 콜백 함수의 인자는 `resolve`, `reject`이다.
    
    ```jsx
    new Promise(function(resolve, reject) {
      // ...
    });
    ```
    

### Fulfilled(이행)

- 콜백 함수의 인자 resolve를 다음과 같이 실행하면 이행(Fulfilled) 상태가 된다.
    
    ```jsx
    new Promise(function(resolve, reject) {
      resolve();
    });
    ```
    
- 이행 상태가 되면 `then()`을 이용하여 처리 결과 값을 받을 수 있다.
    
    ```jsx
    function getData() {
      return new Promise(function(resolve, reject) {
        var data = 100;
        resolve(data);
      });
    }
    
    // resolve()의 결과 값 data를 resolvedData로 받음
    getData().then(function(resolvedData) {
      console.log(resolvedData); // 100
    });
    ```
    
- 프로미스의 ‘이행’상태를 좀 다르게 표현하면 ‘완료’이다.

### Rejected(실패)

- reject를 아래와 같이 호출하면 실패 상태가 된다.
    
    ```jsx
    new Promise(function(resolve, reject) {
      reject();
    });
    ```
    
- 실패 상태가 되면 실패한 이유(실패 처리 결과 값)를 `catch()`로 받을 수 있다.
    
    ```jsx
    function getData() {
      return new Promise(function(resolve, reject) {
        reject(new Error("Request is failed"));
      });
    }
    
    // reject()의 결과 값 Error를 err에 받음
    getData().then().catch(function(err) {
      console.log(err); // Error: Request is failed
    });
    ```
    

## Promise 더 알아보기

- Promise 객체는 생성자를 사용해 만들 수 있다.
    
    ```jsx
    new Promise(/* executor */);
    ```
    
    - 생성자의 인자로 `executor`라는 함수를 이용한다.
    - executor 함수는 `resolve`와 `reject`를 인자로 가진다.
        
        ```jsx
        (resolve, reject) ⇒ {...}
        new Promise( /* executor */ (resolve, reject) => {...} );
        ```
        
        - resolve, reject는 각각 함수이다.
- 생성자를 통해 Promise 객체를 만드는 순간 pending(대기) 상태라고 한다.
    
    ```jsx
    new Promise((resolve, reject) => {}); // pending
    ```
    
- `executor` 함수 인자 중 하나인 `resolve` 함수를 실행하면, fulfilled(이행) 상태가 된다.
    
    ```jsx
    new Promise((resolve, reject) => {
    	//pending
    	//... 비동기 처리
    	resolve();  // fulfilled
    });
    ```
    
- `executor` 함수 인자 중 하나인 `reject` 함수를 실행하면 rejected(거부) 상태가 된다.
    
    ```jsx
    new Promise((resolve, reject) => {
    	reject();  // rejected
    });
    ```
    
- p라는 프로미스 객체는 1초 후에 fulfilled 된다.
    
    ```jsx
    const p = new Promise((resolve, reject) => {
    	/*pending*/
    	setTimeout(() => {
    		resolve(); /* fulfilled */
    	}, 1000);
    });
    
    p.then(() => {
    	/* resolve 된 이후에 실행됨*/ 
    	/* callback */
    	console.log('1000ms후에 fulfilled 됩니다.');
    });
    ```
    
    - **fulfilled 된 시점에 p.then 안에 설정한 callback 함수가 실행**된다.
- then을 설정하는 시점을 명확히하고 함수의 실행과 동시에 프로미스 객체를 만들면서 pending이 시작하도록 하기 위해 **프로미스 객체를 생성하면서 리턴하는 함수 p를 만들어 p 실행과 동시에 then을 설정한다.**
    
    ```jsx
    function p() {
    	return new Promise((resolve, reject) => {
    		/*pending*/
    		setTimeout(() => {
    			resolve(); /* fulfilled */
    		}, 1000);
    	});
    }
    
    //원하는 시점에 프로미스 객체를 생성하고 콜백함수도 호출할 수 있다.
    p().then(() => {
    	console.log('1000ms후에 fulfilled 됩니다.');
    });
    ```
    
- `reject`는 `then`이 아니라 `catch`를 통해 콜백 함수를 처리한다.
    
    ```jsx
    function p() {
    	return new Promise((resolve, reject) => {
    		/*pending*/
    		setTimeout(() => {
    			reject(); /* rejected */
    		}, 1000);
    	});
    }
    
    //원하는 시점에 프로미스 객체를 생성하고 콜백함수도 호출할 수 있다.
    p()
    	.then(() => {
    		console.log('1000ms후에 fulfilled 됩니다.');
    	})
    	.catch(() => {
    		console.log('1000ms후에 rejected 됩니다.');
    });
    ```
    
- `executor`의 `resolve` 함수에 인자를 넣어 실행하면, then의 callback 함수의 인자로도 받을 수 있다.
    
    ```jsx
    function p() {
    	return new Promise((resolve, reject) => {
    		setTimeout(() => {
    			resolve('hello'); 
    		}, 1000);
    	});
    }
    
    //원하는 시점에 프로미스 객체를 생성하고 콜백함수도 호출할 수 있다.
    p().then(message => {
    	console.log('1000ms후에 fulfilled 됩니다.', message);
    });
    ```
    
- 마찬가지로 `executor`의 `reject` 함수에 인자를 넣어 실행하면, catch의 callback 함수의 인자로도 받을 수 있다.
    
    ```jsx
    function p() {
    	return new Promise((resolve, reject) => {
    		/*pending*/
    		setTimeout(() => {
    			reject('error');
    		}, 1000);
    	});
    }
    
    //원하는 시점에 프로미스 객체를 생성하고 콜백함수도 호출할 수 있다.
    p()
    	.then(() => {
    		console.log('1000ms후에 fulfilled 됩니다.');
    	})
    	.catch(reason => {
    		console.log('1000ms후에 rejected 됩니다.', reason);
    });
    ```
    
    - 하지만 일반적으로는 error 문자열이 대신 객체를 만들어 던진다.
        
        ```jsx
        function p() {
        	return new Promise((resolve, reject) => {
        		/*pending*/
        		setTimeout(() => {
        			reject(new Error('error');
        		}, 1000);
        	});
        }
        
        //원하는 시점에 프로미스 객체를 생성하고 콜백함수도 호출할 수 있다.
        p()
        	.then(() => {
        		console.log('1000ms후에 fulfilled 됩니다.');
        	})
        	.catch(e => {
        		console.log('1000ms후에 rejected 됩니다.', e);
        	});
        ```
        
- 추가적으로 fulfilled되거나 rejected된 후에 최종적으로 실행할 것이 있다면, `.finally()`를 설정하고 함수를 인자로 넣는다.
    
    ```jsx
    function p() {
    	return new Promise((resolve, reject) => {
    		/*pending*/
    		setTimeout(() => {
    			reject(new Error('error');
    		}, 1000);
    	});
    }
    
    //원하는 시점에 프로미스 객체를 생성하고 콜백함수도 호출할 수 있다.
    p()
    	.then(() => {
    		console.log('1000ms후에 fulfilled 됩니다.');
    	})
    	.catch(e => {
    		console.log('1000ms후에 rejected 됩니다.', e);
    	})
    	.finally(() => {
    		console.log('end');
    	});
    ```
    

## Promise 예제

```jsx
function getData() {
  return new Promise(function(resolve, reject) {
    $.get('url 주소/products/1', function(response) {
      if (response) {
        resolve(response);
      }
      reject(new Error("Request is failed"));
    });
  });
}

// 위 $.get() 호출 결과에 따라 'response' 또는 'Error' 출력
getData().then(function(data) {
  console.log(data); // response 값 출력
}).catch(function(err) {
  console.error(err); // Error 출력
});
```

- 서버에서 제대로 응답을 받아오면 `resolve()` 메서드를 호출하고, 응답이 없으면 `reject()` 메서드를 호출한다.
- 호출된 메서드에 따라 `then()`이나 `catch()`로 분기하여 응답 결과 또는 오류를 출력한다.

## 여러 개의 프로미스 연결하기 (Promise Chaining)

- 프로미스의 또 다른 특징은 여러 개의 프로미스를 연결해 사용할 수 있다는 점이다.
- `then()` 메서드를 호출하고 나면 새로운 프로미스 객체가 반환된다.
    
    ```jsx
    function getData() {
      return new Promise({
        // ...
      });
    }
    
    // then() 으로 여러 개의 프로미스를 연결한 형식
    getData()
      .then(function(data) {
        // ...
      })
      .then(function() {
        // ...
      })
      .then(function() {
        // ...
      });
    ```
    

## 프로미스의 에러 처리 방법

```jsx
function getData() {
  return new Promise(function(resolve, reject) {
    reject('failed');
  });
}

// 1. then()의 두 번째 인자로 에러를 처리하는 코드
getData().then(function() {
  // ...
}, function(err) {
  console.log(err);
});

// 2. catch()로 에러를 처리하는 코드
getData().then().catch(function(err) {
  console.log(err);
});
```

### then()의 두번째 인자로 에러를 처리하는 방법

```jsx
getData().then(
  handleSuccess,
  handleError
);
```

### catch()를 이용하는 방법

```jsx
getData().then().catch();
```

## 에러 처리는 가급적 catch()를 사용하는 것이 좋다.

```jsx
// then()의 두 번째 인자로는 감지하지 못하는 오류
function getData() {
  return new Promise(function(resolve, reject) {
    resolve('hi');
  });
}

getData().then(function(result) {
  console.log(result);
  throw new Error("Error in then()"); // Uncaught (in promise) Error: Error in then()
}, function(err) {
  console.log('then error : ', err);
});
```

- getData() 함수의 프로미스에서 `resolve()` 메서드를 호출하여 정상적으로 로직을 처리하지만, `then()` 의 첫번째 콜백 함수 내부에서 오류가 나는 경우 오류를 제대로 잡아내지 못한다.
    
    ![Untitled](https://joshua1988.github.io/images/posts/web/javascript/then-not-handling-error.png)
    
- 같은 오류를 catch()로 처리하면 다른 결과가 나온다.
    
    ```jsx
    // catch()로 오류를 감지하는 코드
    function getData() {
      return new Promise(function(resolve, reject) {
        resolve('hi');
      });
    }
    
    getData().then(function(result) {
      console.log(result); // hi
      throw new Error("Error in then()");
    }).catch(function(err) {
      console.log('then error : ', err); // then error :  Error: Error in then()
    });
    ```
    
    ![Untitled](https://joshua1988.github.io/images/posts/web/javascript/catch-handling-error.png)
    

## Promise 단점

- 에러를 잡을 때 몇 번째에서 발생했는지 알아내가 어렵다.
- 특정 조건에 따라 분기를 나누는 작업도 어렵다.
- `async/await`를 사용하면 이러한 문제점을 깔끔하게 해결할 수 있다.

## 참고 자료

- [https://velog.io/@ljinsk3/JavaScript-비동기-처리-Promise-객체](https://velog.io/@ljinsk3/JavaScript-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%B2%98%EB%A6%AC-Promise-%EA%B0%9D%EC%B2%B4)
- [https://joshua1988.github.io/web-development/javascript/promise-for-beginners/](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)