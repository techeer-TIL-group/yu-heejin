## Math.ceil()

- 숫자를 올림 처리할 때 사용한다.
- 입력받은 숫자보다 크거나 같은 정수 중 가장 작은 정수를 리턴한다.
- 즉, **입력받은 숫자를 올림한 정수를 리턴한다.**

### 정수 올림(음수 포함)

```jsx
// 1.올림
const ceil_1 = Math.ceil(1); // 1
const ceil_2 = Math.ceil(1.222); // 2
const ceil_3 = Math.ceil(1.5); // 2
const ceil_4 = Math.ceil(1.777); // 2

// 2. null 또는 0인 경우
const ceil_5 = Math.ceil(null); // 0
const ceil_6 = Math.ceil(0); // 0

// 3. 음수인 경우
const ceil_7 = Math.ceil(-1); // -1
const ceil_8 = Math.ceil(-1.111); // -1
const ceil_9 = Math.ceil(-1.5); // -1
const ceil_10 = Math.ceil(-1.777); // -1
```

### 자릿수 지정(소수점 이하, 10단위, 100단위)

```jsx
// 1.소수점이하
const ceil_1 = Math.ceil(1.222 * 10) / 10; // 1.3
const ceil_2 = Math.ceil(1.222 * 100) / 100; // 1.23

// 2. 10단위, 100단위
const ceil_3 = Math.ceil(1222 / 10) * 10; // 1230
const ceil_4 = Math.ceil(1222 / 100) * 100; // 1300
```

- `Math.ceil()` 함수를 이용해 **소수점 아래에서 값을 올림**하고 싶은 경우
    1. 처리하려는 숫자의 부동소수점을 올림하고 싶은 숫자 앞까지 옮겨준다. (10, 100, 1000… 등의 숫자를 곱한다.)
        - `1.222 * 10 = 12.22 → 13`
    2. `Math.ceil()` 함수를 적용하고 다시 소수점의 위치를 복구시킨다. (10, 100, 1000… 등의 숫자로 나눈다.)
        - `13 / 10 → 1.3`
- 반대로 **10단위, 100단위에서 올림을 처리**하고 싶은 경우
    1. 숫자의 부동 소수점을 올림하고 싶은 숫자 앞까지 옮겨준다. (10, 100, 1000… 등의 숫자로 나눈다.)
        - `1222 / 10 = 122.2 → 123`
    2. `Math.ceil()` 함수를 적용하고 다시 소수점의 위치를 복구시킨다.
        - `123 * 10 = 1230`

## Math.floor()

- 숫자를 내림 처리 할 때 사용한다.
- 입력받은 숫자보다 작거나 같은 정수 중 가장 큰 정수를 리턴한다.
- 즉, **입력받은 숫자를 내림한 정수를 리턴한다.**

### 정수 내림(음수 포함)

```jsx
// 1.내림
const floor_1 = Math.floor(1); // 1
const floor_2 = Math.floor(1.222); // 1
const floor_3 = Math.floor(1.5); // 1
const floor_4 = Math.floor(1.777); // 1

// 2. null 또는 0인 경우
const floor_5 = Math.floor(null); // 0
const floor_6 = Math.floor(0); // 0

// 3. 음수인 경우
const floor_7 = Math.floor(-1); // -1
const floor_8 = Math.floor(-1.111); // -2
const floor_9 = Math.floor(-1.5); // -2
const floor_10 = Math.floor(-1.777); // -2
```

### 자릿수 지정(소수점 이하, 10단위, 100단위)

```jsx
// 1.소수점이하
const floor_1 = Math.floor(1.777 * 10) / 10; // 1.7
const floor_2 = Math.floor(1.777 * 100) / 100; // 1.77

// 2. 10단위, 100단위
const floor_3 = Math.floor(1777 / 10) * 10; // 1770
const floor_4 = Math.floor(1777 / 100) * 100; // 1700
```

- 위와 같은 방법을 사용하면 된다.

## Math.round()

- 숫자를 반올림 처리할 때 사용한다.
- 파라미터로 입력받은 **숫자의 소수점 이하의 값이 0.5보다 크면 입력받은 수보다 다음으로 높은 절댓값을 가지는 정수를 리턴**한다.
- **소수점 이하의 값이 0.5보다 작으면 입력받은 수보다 절댓값이 더 낮은 정수를 리턴**한다.
- **소수점 이하의 값이 0.5와 같으면 입력받은 수보다 큰 다음 정수를 리턴**한다.

### 정수 반올림(음수 포함)

```jsx
// 1.반올림
const round_1 = Math.round(1); // 1
const round_2 = Math.round(1.222); // 1
const round_3 = Math.round(1.5); // 2
const round_4 = Math.round(1.777); // 2

// 2. null 또는 0인 경우
const round_5 = Math.round(null); // 0
const round_6 = Math.round(0); // 0

// 3. 음수인 경우
const round_7 = Math.round(-1); // -1
const round_8 = Math.round(-1.111); // -1
const round_9 = Math.round(-1.5); // -1
const round_10 = Math.round(-1.777); // -2
```

### 자릿수 지정(소수점 이하, 10단위, 100단위)

```jsx
// 1.소수점이하
const round_1 = Math.round(1.222 * 10) / 10; // 1.2
const round_2 = Math.round(1.5 * 10) / 10; // 1.5
const round_3 = Math.round(1.777 * 10) / 10; // 1.8

// 2. 10단위
const round_4 = Math.round(1001 / 10) * 10; // 1000
const round_5 = Math.round(1005 / 10) * 10; // 1010
const round_6 = Math.round(1007 / 10) * 10; // 1010
```

## 소수점 숫자 정밀도 문제

- 소수점 이하의 값을 올림/내림/반올림 처리할 때, **부동 소수점의 정밀도 문제 때문에 예상하지 못한 결과가 나오기도 한다.**
- 부동 소수점 오차를 해결하기 위해 `Number.EPSILON` 이라는 상수를 사용할 수도 있다!
    - 이 값은 자바스크립트에서 오차없이 나타낼 수 있는 가장 작은 양의 수이다.
    - 따라서 부동 소수점 오차를 보정하기 위해 `Number.EPSILON` 값을 더해준 뒤 그 값으로 반올림을 처리할 수 있다.

## 소수점 반올림

### toFixed()

```jsx
// 1.소수점이하
const fixed_1 = 1.222.toFixed(1); // 1.2
const fixed_2 = 1.5.toFixed(1); // 1.5
const fixed_3 = 1.777.toFixed(1); // 1.8

// 2.음수
const fixed_4 = (-1.222).toFixed(1); // -1.2
const fixed_5 = (-1.5).toFixed(1); // -1.5
const fixed_6 = (-1.777).toFixed(1); // -1.8
```

- 부동 소수점(Floating Point) 숫자를 **고정 소수점(Fixed Point) 숫자로 바꿔 리턴**한다.
- 이 때 파라미터로 숫자(digits)를 전달하면, **숫자만큼의 소수점 이하 자리수를 가지는 문자열로 리턴해준다.**
- **원본 숫자의 소수점 이하 길이가 digits보다 길면 숫자를 반올림하고, 짧으면 뒤를 0으로 채워서 리턴**한다.

### toPrecision()

```jsx
// 1.소수점이하
const precision_1 = 1.222.toPrecision(2); // 1.2
const precision_2 = 1.5.toPrecision(2); // 1.5
const precision_3 = 1.777.toPrecision(2); // 1.8

// 2.음수
const precision_4 = (-1.222).toPrecision(2); // -1.2
const precision_5 = (-1.5).toPrecision(2); // -1.5
const precision_6 = (-1.777).toPrecision(2); // -1.8
```

- 숫자를 파라미터로 전달된 **유효 자리수(precision) 길이의 문자열로 만들어 리턴**한다.
- 만약 **유효 자리수가 원본 숫자의 자리수보다 작으면 반올림한 값을 리턴**한다.
    - 올림이 아니라 반올림임을 기억할 것

## 참고 자료

- [https://hianna.tistory.com/446](https://hianna.tistory.com/446)