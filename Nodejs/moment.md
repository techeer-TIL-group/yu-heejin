- node.js는 javascript 기반으로 동작하기 때문에 기본적으로 시간 및 날짜 오브젝트인 Date를 가지고 있다.
- 그러나 Date 오브젝트를 사용하여 코드를 작성하면 가독성이 떨어지는 경우가 생기는데, 이 경우 moment.js를 사용한다.
    - **moment.js를 사용하면 다양한 형식으로 날짜나 시간을 파싱하거나 계산**할 수 있다.
- 하지만 **성능과 속도 측면에서 moment가 Date보다 뒤쳐진다는 단점**이 있다.

## moment.js 사용하기

### 현재 날짜

```jsx
// 현재 날짜
console.log(moment());
```

- 현재 날짜를 출력하고 싶다면 `moment()`를 사용한다.

### 특정 날짜 지정

```jsx
// 특정 날짜 지정
console.log(moment("2021-01-27", "YYYY-MM-DD")); 
// Moment<2021-01-27T00:00:00+09:00>
```

- `moment(’date’)`
- 다음과 같은 형식으로 인자를 줄 수도 있다.
    
    ```jsx
    let date1 = moment([2020, 11, 18]);
    ```
    

### 형식 지정

```jsx
// 현재 날짜 형식 지정
let date = moment("2021-01-27");

// 년-월-일
console.log(date.format("YYYY-MM-DD")); // 2021-01-27

// 시:분:초
console.log(date.format("HH:mm:ss")); // 00:00:00

// 요일
console.log(date.format("dddd")); // Wednesday

// 년-월-일 요일
console.log(date.format("YYYY-MM-DD dddd")); // 2021-01-27

// 년-월-일 시:분:초
console.log(date.format("YYYY-MM-DD HH:mm:ss")); 
// 2021-01-27 00:00:00

// 년-월-일 요일 시:분:초
console.log(date.format("YYYY-MM-DD dddd HH:mm:ss")); 
// 2021-01-27 Wednesday 00:00:00
```

### 날짜 더하거나 빼기

```jsx
// 날짜 더하거나 빼기
console.log(moment("2021-01-27").add(1, "days")); // 2021-01-28
console.log(moment("2021-01-27").add(2, "months")); // 2021-03-27
console.log(moment("2021-01-27").add(2, "years")); // 2023-01-27
console.log(moment("2021-01-27").subtract(1, "days")); // 2021-01-26
console.log(moment("2021-01-27").subtract(2, "months")); // 2020-11-27
console.log(moment("2021-01-27").subtract(2, "years")); // 2019-01-27
```

- 날짜 더하기 : `add()`
- 날짜 빼기 : `substract()`
- 인자로 더할 숫자, 단위를 준다.
- 만약 현 시점으로부터 날짜를 구하고자 한다면 `moment().add/subtract()` 사용
- 하나의 moment 변수를 기준으로 날짜를 연속적으로 더하거나 빼면 변한 값을 기준으로 연산하게 되는데, 이때 `clone()`을 사용하면 기존 값을 기준으로 계산이 가능하다.
    
    ```jsx
    // 해결 방법
    console.log("========== solutions to the above problems ==========");
    let fixedNow = moment("2021-01-27");
    console.log(fixedNow.clone().add(3, "days")); // 2021-01-30
    console.log(fixedNow.clone().subtract(5, "days")); // 2021-01-22
    console.log(fixedNow); // 2021-01-27
    ```
    
    ```jsx
    // 날짜 더하고 뺄 때 문제점
    console.log("========== problems when adding or subtracting dates ==========");
    let now = moment("2021-01-27");
    console.log(now.add(3, "days")); // 2021-01-30
    console.log(now.subtract(5, "days")); // 2021-01-25
    console.log(now); // 2021-01-25
    ```
    

### 시간 순서 비교

- `isBefore()` : `moment()`의 날짜가 `isBefore` 파라미터 날짜보다 이전인지 확인
    
    ```jsx
    // isBefore: moment()의 날짜가 isBefore 파라미터 날짜보다 이전인지 여부
    console.log("========== isBefore ==========");
    console.log(moment("2020-12-31").isBefore("2021-01-01")); // true
    console.log(moment("2020-12-31").isBefore("2020-12-30")); // false
    ```
    
- `isAfter()` : `moment()`의 날짜가 `isAfter`의 파라미터 날짜보다 이후인지 확인
    
    ```jsx
    // isAfter: moment()의 날짜가 isAfter의 파라미터 날짜보다 이후인지 여부
    console.log("========== isAfter ==========");
    console.log(moment("2020-12-31").isAfter("2021-01-01")); // false
    console.log(moment("2020-12-31").isAfter("2020-12-30")) // true
    ```
    
- `isSame()` : `moment()`의 날짜가 `isSame`의 파라미터 날짜와 같은지 확인
    
    ```jsx
    // isSame: moment()의 날짜가 isSame의 파라미터 날짜와 같은지 여부
    console.log("========== isSame ==========");
    console.log(moment("2020-12-31").isSame("2021-01-01")); // false
    console.log(moment("2020-12-31").isSame("2020-12-31")); // true
    ```
    
- `isSameOrBefore()` : `moment()`의 날짜가 `isSameOrBefore`의 파라미터 날짜와 같거나 이전인지 확인
    
    ```jsx
    // isSameOrBefore: moment()의 날짜가 isSameOrBefore의 파라미터 날짜와 같거나 이전인지 여부
    console.log("========== isSameOrBefore ==========");
    console.log(moment("2020-12-31").isSameOrBefore("2021-01-01")); // true
    console.log(moment("2020-12-31").isSameOrBefore("2020-12-31")); // true
    console.log(moment("2020-12-31").isSameOrBefore("2020-12-30")); // false
    ```
    
- `isSameOrAfter()` : `moment()`의 날짜가 `isSameOrAfter`의 파라미터 날짜와 같거나 이후인지 확인
    
    ```jsx
    // isSameOrAfter: moment()의 날짜가 isSameOrAfter의 파라미터 날짜와 같거나 이후인지 여부
    console.log("========== isSameOrAfter ==========");
    console.log(moment("2020-12-31").isSameOrAfter("2021-01-01")); // false
    console.log(moment("2020-12-31").isSameOrAfter("2020-12-31")); // true
    console.log(moment("2020-12-31").isSameOrAfter("2020-12-30")); // true
    ```
    
- `isBetween()` : `moment()`의 날짜가 `isBetween`의 파라미터들 사이의 날짜인지 확인
    
    ```jsx
    // isBetween: moment()의 날짜가 isBetween의 파라미터들 사이의 날짜인지 여부
    console.log("========== isBetween ==========");
    console.log(moment("2020-12-31").isBetween("2020-12-01", "2021-01-01")); // true
    console.log(moment("2020-12-31").isBetween("2021-01-01", "2021-01-02")); // false
    
    // isBetween의 첫 번째 파라미터는 항상 두 번쨰 파라미터보다 이전인 날짜여야 한다.
    // 그렇지 않으면 항상 false
    console.log(moment("2020-12-31").isBetween("2021-01-01", "2020-12-01")); // false
    ```
    
    - `isBetween`의 **첫번째 파라미터는 항상 두번째 파라미터보다 이전 날짜**여야 한다.

### 시간 차이

```jsx
// 시간의 차이
console.log(moment("2020-12-31").diff("2020-12-30", "days")); // 1
console.log(moment("2020-12-30").diff("2020-12-31", "days")); // -1
console.log(moment("2020-12-31").diff("2020-12-30", "minute")); // 1440
console.log(moment("2020-12-30").diff("2020-12-31", "minute")); // -1440
```

- `diff()` : `moment()`의 날짜를 기준으로 `diff`의 **첫번째 파라미터 날짜와의 차이를 두번째 파라미터를 기준으로 계산**하여 출력한다.

## 참고 자료

- [https://millo-l.github.io/Nodejs-moment-사용하기/](https://millo-l.github.io/Nodejs-moment-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0/)
- [https://momentjs.com/](https://momentjs.com/)
- [https://hanswsw.tistory.com/5](https://hanswsw.tistory.com/5)