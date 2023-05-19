## forEach

```jsx
const a = [1, 2, 3];
const doubled = a.forEach((num, index) => {
	return num*=2;
})

console.log(doubled)// doubled = undefined
```

- 배열의 요소를 반복한다.
- **각 요소에 대해 콜백을 실행하며 값을 반환하지 않는다.**

## map

```jsx
const b = [1, 2, 3];
const doubledmap = b.map((num, index) => {
	return num*=2;
})

console.log(doubledmap)// doubledmap = [2,4,6];
```

- 배열의 요소를 반복한다.
- **각 요소에서 함수를 호출하여 결과로 새 배열을 작성하여 각 요소를 새 요소에 매핑**한다.

## 차이점

- .map()은 새로운 배열을 반환한다.
- 반복의 결과를 조작한 새로운 배열이 필요하다면 map(), 단순히 배열을 반복할 필요가 있다면 forEach를 사용한다.
- 성능적인 측면에서는 forEach가 좋으나, 실제로 결과 배열이 필요한 경우 map의 성능이 2배정도 좋다.

## 반복문 별 성능

### 값을 할당하지 않았을 때(결과 배열이 필요 없을 때)

- for에서 반복 길이를 먼저 설정하거나 내부에서 length를 계속해서 참조하는데 있어서 큰 차이가 없다.
- forEach나 map은 성능이 매우 느린편이다.

### 값을 할당할 때(결과 배열이 필요할 때)

- forEach가 가장 느린 성능을 보였고, 그 외 for, while, map은 모두 비슷한 성능을 보인다.
- 새로운 배열이 필요한 경우는 map, 그 외에는 순수 for문을 사용하는 것이 좋다.

## 참고 자료

- [https://fathory.tistory.com/31](https://fathory.tistory.com/31)