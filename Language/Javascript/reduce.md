## reduce 함수

- 함수를 실행한 후 하나의 결과값을 반환

## 사용 예시 1 - 기본

```tsx
const numbers = [4, 3, 2, 1];
let sum = numbers.reduce((acc, cur) => acc + cur)
```

- `acc (accumulator)` : 누산기, 누적되는 값, 최종적으로 출력되는 값
- `cur (current)` : 현재 돌고있는 요소
- 위 코드를 풀어서 보면 다음과 같다.
    
    ```tsx
    const numbers = [4, 3, 2, 1];
    
    let sum = numbers.reduce((acc, cur) => {
    	return acc + cur;
    });
    ```
    
- 더 풀어서 살펴보면 다음과 같다.
    
    ```tsx
    acc += cur; // 4 += 3;
    acc += cur; // 7 += 2;
    acc += cur; // 9 += 1;
    
    console.log(acc); // 10
    ```
    
    - `acc`에는 가장 첫번째 원소인 4를 할당한다. `(index 0)`
    - `cur`에는 나머지 원소인 3, 2, 1이 순차적으로 들어간다. `(index 1, 2, 3…)`

## 사용 예시 2 - 인자 추가 1

```tsx
const numbers = [4, 3, 2, 1];
let sum = numbers.reduce((accumulator, current) => accumulator + current, 0);
console.log(sum);
```

- `initialValue` : acc의 초기값 (optional)
- **acc의 초기값으로 initialValue를 할당하는 순간 cur은 index 1이 아니라 0부터 돌아간다!**

## 사용 예시 3 - 인자 추가 2

```tsx
const avg = numbers.reduce((acc, cur, index, arr) => {
	if (index === arr.length - 1) { // index가 마지막일 때
		return (acc + cur) / arr.length; // cur - 4
	}
	return acc + cur; // cur - 1, 2, 3
	}, 0);
ㅤ
console.log("avg", avg);
```

- `idx (index)` : 배열 요소의 순서 (optional)
- `arr (array) or src (source)` : 현재 배열, 원본 배열 (optional)

## 참고 자료

- [https://velog.io/@teo_ryu/javascript-reduce-함수와-싸우기](https://velog.io/@teo_ryu/javascript-reduce-%ED%95%A8%EC%88%98%EC%99%80-%EC%8B%B8%EC%9A%B0%EA%B8%B0)