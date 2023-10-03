## Object.values()

- 특정 객체를 대상으로 value 값들만 뽑아서 배열로 변환하는 메서드
- 배열 값 순서는 오브젝트의 속성을 `for in` 구문 등으로 반복한 결과와 동일하다
    - `for in` 구문은 순서를 보장하지 않는다.

### 숫자들로 이루어진 객체

```jsx
var obj = {
	a: 2,
	b: 42,
	c: 4
};

Object.values(obj); // [2, 42, 4]
```

### 여러 형식의 값들로 이루어진 객체

```jsx
var obj = {
	a: {a : 'somestring'},
	b: 42,
	c: false,
	d: 'str'
};

Object.values(obj); // [{a : 'somestring'}, 42, false, 'str']
```

## Array.prototype.reverse()

```jsx
const array1 = ['one', 'two', 'three'];
console.log('array1:', array1);
// Expected output: "array1:" Array ["one", "two", "three"]

const reversed = array1.reverse();
console.log('reversed:', reversed);
// Expected output: "reversed:" Array ["three", "two", "one"]

// Careful: reverse is destructive -- it changes the original array.
console.log('array1:', array1);
// Expected output: "array1:" Array ["three", "two", "one"]
```

- 배열의 순서를 반전한다.

## Array.prototype.find()

```jsx
const array1 = [5, 12, 8, 130, 44];

const found = array1.find(element => element > 10);

console.log(found);
// Expected output: 12
```

- 주어진 판별 함수를 만족하는 **첫 번째 요소**의 값을 반환한다.
- 해당 요소가 없다면 undefined를 반환한다.
- `findIndex()`는 값 대신 인덱스를 반환한다.

## Array.prototype.unshift

```jsx
const array1 = [1, 2, 3];

console.log(array1.unshift(4, 5));
// Expected output: 5

console.log(array1);
// Expected output: Array [4, 5, 1, 2, 3]
```

- **새로운 요소를 배열의 맨 앞쪽에 추가**하고 새로운 길이를 반환한다.

## reduce()

```jsx
arr.reduce(callback(acc, cur, curIndex, arr), initVal]);
```

- acc: 누적 값
- cur: 현재 값
- curIndex: 현재 인덱스
- arr: 원본 배열

## JSON.parse(), JSON.stringify()

### JSON 내장 객체

- 자바스크립트에서는 JSON 포멧의 데이터를 간편하게 다룰 수 있도록 `JSON`이라는 객체를 내장하고 있다.
- 해당 객체는 자바스크립트 코드를 브라우저에서 실행하든 Node.js 런타임에서 실행하든 상관없이 **전역(global)에서 접근이 가능하다.**

### JSON.parse()

```jsx
const str = `{
  "name": "홍길동",
  "age": 25,
  "married": false,
  "family": { "father": "홍판서", "mother": "춘섬" },
  "hobbies": ["독서", "도술"],
  "jobs": null
}`;

const obj = JSON.parse(str);

// 실행 결과
{
    name: "홍길동",
    age: 25,
    married: false,
    family: {
        father: "홍판서",
        mother: "춘섬"
    },
    hobbies: [
        "독서",
        "도술"
    ],
    jobs: null
}
```

- JSON 문자열을 Javascript 객체로 변환한다.
- 출력 결과를 보면 다음과 같은 차이가 있다.
    - JSON 문자열에서는 key를 나타낼 때 반드시 `“`로 감싸줘야한다.
    - 반면 Javascript 객체에서는 `“`를 반드시 사용할 필요가 없다.
        - 단, 특수 문자나 영어 이외의 단어가 키로 지정되면 사용해야한다.
- 외부에서 문자열 형태로 주어진 데이터를 해당 언어에서 다루기 용이하도록 내장 데이터 타입으로 변환하는 과정을 **역직렬화(deserialization)**이라고 부른다.

### JSON.stringify()

- Javascript 객체를 JSON 문자열로 변환
- Javascript 객체를 인자로 받고 JSON 문자열을 반환한다.

## 참고 자료

- https://mine-it-record.tistory.com/376
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift
- [https://okayoon.tistory.com/entry/Javascript의-reduce-메서드를-알아보자](https://okayoon.tistory.com/entry/Javascript%EC%9D%98-reduce-%EB%A9%94%EC%84%9C%EB%93%9C%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
- https://www.daleseo.com/js-json/