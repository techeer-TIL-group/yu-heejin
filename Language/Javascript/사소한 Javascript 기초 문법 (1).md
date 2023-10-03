## 문자열 대문자, 소문자로 변환하는 방법

### 문자열 대문자로 바꾸는 방법

```jsx
const str = 'hello black lobster!';
const result = str.toUpperCase();

document.write(result);
```

- `toUpperCase()` 메소드 사용
- [참고] 자바스크립트는 char 타입이 존재하지 않고, 한 문자만 있더라도 string으로 인식한다.

### 문자열 소문자로 바꾸는 방법

```jsx
const str = 'HELLO BLACK LOBSTER!';
const result = str.toLowerCase();

document.write(result);
```

- `toLowerCase()` 메서드 사용

### 대문자/소문자 체크 방법

```jsx
// 대문자인지 확인하는 함수 작성
function isUpperCase(str) {
  return str === str.toUpperCase();
}

// 소문자인지 확인하는 함수 작성
function isLowerCase(str) {
  return str === str.toLowerCase();
}
```

- 변환된 문자열과 원래 문자열을 비교한다.

## 배열을 문자열로 변환

### Array.join()으로 문자열 변환

```jsx
const arr = ["Hello", "World", "JavaScript", "!!!"];

let str = arr.join();
console.log(str)

str = arr.join("");
console.log(str)

str = arr.join(" ");
console.log(str)

// 실행 결과
Hello,World,JavaScript,!!!
HelloWorldJavaScript!!!
Hello World JavaScript !!!
```

- 배열에 여러 문자열이 있을 때 해당 문자열들을 연결하여 하나의 String 객체로 만들어준다.
- `Array.join(separator)`은 배열의 요소들을 하나의 문자열로 연결하며, 각 요소 사이에 인자로 전달된 구분자를 추가한다.
- 인자를 생략하면 `,`가 기본 구분자로 설정된다.

### for문으로 문자열 변환

```jsx
const arr = ["Hello", "World", "JavaScript", "!!!"];
let str = "";

arr.forEach((element) => {
  const separator = " ";
  if (str.length == 0) {
    str = str + element;
  } else {
    str = str + separator + element;
  }
})
console.log(str)

// 실행 결과
Hello World JavaScript !!!
```

### Array.toString()으로 문자열 변환

```jsx
const arr = ["Hello", "World", "JavaScript", "!!!"];

let str = arr.toString();
console.log(str)

// 실행 결과
Hello,World,JavaScript,!!!
```

- `,`를 구분자로 요소들을 하나의 문자열로 연결하여 리턴한다.
- replace()를 사용하여 `,`를 다른 구분자로 변경할 수 있다.
    
    ```jsx
    const arr = ["Hello", "World", "JavaScript", "!!!"];
    
    let str = arr.toString();
    str = str.replace(/,/g, " ");
    console.log(str)
    
    // 실행 결과
    Hello World JavaScript !!!
    ```
    
    - 하지만 배열 안의 문자열에 `,`가 있다면 해당 부분도 함께 교체되니 주의해야한다.

## 문자열 자르기

### substr

```jsx
var str = '자바스크립트';

var result1 = str.substr(0, 2);
// 결과 : "자바"

var result2 = str.substr(2, 4);
// 결과 : "스크립트"

var result3 = str.substr(2);
// 결과 : "스크립트"
```

- `substr(시작 위치, 길이)` or `substr(시작 위치)`
- 길이 부분을 생략하면 시작 위치부터 문자열 끝까지 자른다.

### substring

```jsx
var str = '자바스크립트';

var result1 = str.substring(0, 2);
// 결과 : "자바"

var result2 = str.substring(2, 5);
// 결과 : "스크립"

var result3 = str.substring(2, 6);
// 결과 : "스크립트"

var result4 = str.substring(2);
// 결과 : "스크립트"
```

- `substring(시작 위치, 종료 위치)` or `substring(시작 위치)`
- 시작 위치에서 종료 위치까지의 문자열을 자른다.
- 주의할 점은 **종료 위치 -1 까지의 문자열을 자른다.**
- 인자에 음수를 대입하면 해당 값은 0으로 치환되며, 종료 위치에 음수 또는 0인 경우 첫 번째 인수와 두 번째 인수가 뒤바뀐다는 것에 주의해야한다.
    
    ```jsx
    var str = '자바스크립트';
    
    var result1 = str.substring(-4, 5); // str.substring(0, 5)
    // 결과 : "자바스크립"
    
    var result2 = str.substring(2, -1); // str.substring(0, 2)
    // 결과 : "자바"
    ```
    

### slice 함수로 문자열 자르기

```jsx
var str = '자바스크립트';

var result1 = str.slice(0, 2);
// 결과 : "자바"

var result2 = str.slice(2, 6);
// 결과 : "스크립트"

var result3 = str.slice(2);
// 결과 : "스크립트"

/************************************/

var result4 = str.slice(-4);
// 결과 : "스크립트"

var result5 = str.slice(-4, 5);
// 결과 : "스크립"

var result6 = str.slice(2, -1);
// 결과 : "스크립"
```

- `slice(시작 위치, 종료 위치)` or `slice(시작 위치)`
- `slice` 함수의 기본 사용법은 `substring` 함수와 동일하며, 다른 점은 **음수를 자유롭게 사용할 수 있어 뒤에서부터 문자열을 자를 때 유용하게 사용할 수 있다.**
- `str.slice(-4)`는 문자열의 뒤에서부터 4번째 자리까지 문자열을 자르라는 의미이다.

## charAt()

- 문자열에서 지정된 위치에 존재하는 문자를 찾아서 반환하는 함수

## split()

```jsx
string.split(separator, limit)
```

- 문자열을 일정한 구분자(separator)로 잘라서 limit 크기 이하의 배열로 저장
- 인수는 필수는 아니며, **아무것도 전달하지 않으면 문자열 전체를 길이가 1인 배열에 담아서 반환한다.**
    
    ```jsx
    const str = "apple banana orange";
    
    const arr = str.split();
    
    document.writeln(arr); // apple banana orange
    document.writeln(arr.length); // 1
    ```
    

### 단어별로 자르기

```jsx
const str = "apple banana orange";

const arr = str.split(" ");

document.writeln(arr.length); // 3
document.writeln(arr[0]); // apple
document.writeln(arr[1]); // banana
document.writeln(arr[2]); // orange
```

### 글자별로 잘라서 배열에 담기

```jsx
const arr = str.split("");

document.writeln(arr.length); // 5
document.writeln(arr[0]); // a
document.writeln(arr[1]); // ' '(space)
document.writeln(arr[2]); // b
document.writeln(arr[3]); // ' '(space)
document.writeln(arr[4]); // c
```

- 공백도 한 글자로 취급한다.

## 참고 자료

- https://blacklobster.tistory.com/15
- https://codechacha.com/ko/javascript-array-to-string/
- https://gent.tistory.com/414
- https://redcow77.tistory.com/607
- https://leftday.tistory.com/98
- https://hianna.tistory.com/377