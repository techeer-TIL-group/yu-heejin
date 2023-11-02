## RegExp.test()

```jsx
const str = 'table football';

const regex = new RegExp('foo*');
const globalRegex = new RegExp('foo*', 'g');

console.log(regex.test(str));
// Expected output: true

console.log(globalRegex.lastIndex);
// Expected output: 0

console.log(globalRegex.test(str));
// Expected output: true

console.log(globalRegex.lastIndex);
// Expected output: 9

console.log(globalRegex.test(str));
// Expected output: false
```

- **주어진 문자열이 정규 표현식을 만족하는지 판별**하고, 그 여부를 `true`/`false`로 반환한다.

## Object.freeze()

```jsx
const obj = {
  prop: 42,
};

Object.freeze(obj);

obj.prop = 33;
// Throws an error in strict mode

console.log(obj.prop);
// Expected output: 42
```

- 객체를 동결하기 때문에 변경할 수 없다.
- 동결된 객체는 새로운 속성을 추가하거나 제거하는 것을 방지하여 속성의 불변성을 보장한다.
- javascript는 typescript와 달리 `as const` 같은 기능이 없기 때문에 해당 기능을 사용하여 상수 선언을 많이 하는 것 같다.

## Number.isSafeInteger()

```jsx
Number.isSafeInteger(3); // true
Number.isSafeInteger(Math.pow(2, 53)); // false
Number.isSafeInteger(Math.pow(2, 53) - 1); // true
Number.isSafeInteger(NaN); // false
Number.isSafeInteger(Infinity); // false
Number.isSafeInteger("3"); // false
Number.isSafeInteger(3.1); // false
Number.isSafeInteger(3.0); // true
```

- **인수로 전달된 숫자 값이 안전한 정수인지 여부**를 확인

### 안전한 정수

- 안전한 정숫값은 `-(2^53 - 1)` 부터 `2^53 - 1` 사이의 **모든 정수값으로 구성**된다.

## join()

```jsx
const arr = ['바람', '비', '물'];

console.log(arr.join());
// 바람,비,물

console.log(arr.join(''));
// 바람비물

console.log(arr.join('-'));
// 바람-비-물
```

- 배열의 원소를 문자열로 합친다.

## trim()

```jsx
const greeting = '   Hello world!   ';

console.log(greeting);
// Expected output: "   Hello world!   ";

console.log(greeting.trim());
// Expected output: "Hello world!";
```

- 문자열 양 끝 공백을 제거한 새로운 문자열을 반환한다.
- 한쪽 끝 공백만 제거하고 싶다면 `trimStart()`, `trimEnd()`를 사용한다.

## repeat()

```jsx
string.repeat(${count});

"abc".repeat(-1); // RangeError
"abc".repeat(0); // ''
"abc".repeat(1); // 'abc'
"abc".repeat(2); // 'abcabc'
"abc".repeat(3.5); // 'abcabcabc' (count will be converted to integer)
"abc".repeat(1 / 0); // RangeError

({ toString: () => "abc", repeat: String.prototype.repeat }).repeat(2);
// 'abcabc' (repeat() is a generic method)
```

- 주어진 문자열을 옵션의 `count`만큼 반복하여 붙인 다음 새로운 문자열로 반환하는 함수
- `count`는 양의 정수여야하며 무한대보다 작고, 최대 문자열 크기를 넘어서는 안된다.

## 참고 자료

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze
- https://yorr.tistory.com/21
- [https://velog.io/@arthur/모던-자바스크립트-Deep-Dive-28장.-Number](https://velog.io/@arthur/%EB%AA%A8%EB%8D%98-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Deep-Dive-28%EC%9E%A5.-Number)
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/isSafeInteger
- https://yeoncoding.tistory.com/46
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/trim
- https://redcow77.tistory.com/629
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/repeat