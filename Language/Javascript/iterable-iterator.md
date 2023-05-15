## Iteration Protocol

- ES6에서 새로 도입된 데이터 컬렉션 객체(대표적으로 Array)를 순회하기 위한 Protocol
- 해당 프로토콜을 준수한 객체만이 `for…of`문으로 순회할 수 있고, Spread 문법의 피연산자가 될 수 있다.
- **Iteration Protocol은 Iterable Protocol과 Iterator Protocol을 총칭한다.**

## 이터러블(Iterable)

- 자료를 반복할 수 있는 객체
    - 흔히 사용하는 배열이 대표적인 이터러블 객체이다.
- `Symbol.iterator` 메서드를 가진 객체
    - 이터레이터를 리턴하는 `[Symbole.iterator]()` 메서드를 가진다.
    - 배열의 경우 `Array.prototype`의 `Symbol.iterator`를 상속받기 때문에 이터러블이다.
- 별도로 `Symbol.iterator` 메서드를 정의해주거나 프로토타입 상속을 통해 해당 메서드를 가지고 있다면 Iterable Protocol의 조건을 충족하여 Iterable이라고 할 수 있다.
- Iterable 객체는 `for…of`문으로 순회할 수 있고, Spread 문법의 피연산자로 사용할 수도 있다.
- 대표적으로 Array 객체가 있다.

### 이터러블 지원 객체

> Array, String, Map, Set, TypedArray(Int8Array, Uint8Array, Uint8ClampedArray, Int16Array, Uint16Array, Int32Array, Uint32Array, Float32Array, Float64Array), DOM data structure(NodeList, HTMLCollection), Arguments
> 

## 이터레이터(Iterator)

```tsx
const array = [1, 2, 3, 4, 5];

const iterator = array[Symbol.itrator]()
// Symbol.itrator를 호출해서 반환된 iterator를 받는다.

console.log(iterator.next()); //반복
// { value: 1, done: false }
// { value: 2, done: false }
// { value: 3, done: false }
// { value: 4, done: false }
// { value: 5, done: false }
// { value: undefined, done: true }
```

- 이터러블 객체의 `Symbol.iterator` 메소드를 호출하면 `Iterator` 객체를 반환한다.
- `{value: 값, done: true/false}` 형태의 이터레이터 객체를 리턴하는 `next()` 메서드를 가진 객체
    - 반환된 Iterator 객체는 next 메서드를 소유하고 있다.
    - **next 메서드를 호출할 때 Iterator Result 객체를 반환한다면 Iterator Protocol을 충족하여 Iterator라고 할 수 있다.**
    - next 메서드로 순환할 수 있는 객체이며, `Symbol.iterator`안에 정의되어 있다.
- Iterator Result 객체는 value와 done 프로퍼티를 갖고 있으며, **value는 현재 순회하는 값을 갖고 있고, done은 순회가 언제 끝나는지 알려준다.**
    - next 메서드는 반복적으로 호출되다가 모든 요소를 순회하면 value 프로퍼티는 undefined, done 프로퍼티는 true가 되어 순회를 종료한다.

## Symbol.iterator

- 다음은 range 객체를 이터러블 객체로 만드는 과정이다.
    
    ```tsx
    let range = { // 1) 객체 생성
      from: 1,
      to: 5
    };
    
    range[Symbol.iterator] = function() { // 2) 새로운 키:밸류 를 추가한다. 키는 변수형태, 밸류는 함수이다.
    
        return { // 객체를 리턴한다. 그런데 좀 특벽할 형태의 객체
          current: this.from,
          last: this.to,
    
          next() { // 3) next() 정의
            if (this.current <= this.last) {
              return { done: false, value: this.current++ }; 
              // 4) {value : 값 , done : true/false} 형태의 이터레이터 객체를 리턴합니다.
            } else {
              return { done: true };
            }
          }
        };
    };
    ```
    
    1. 평범한 range 객체를 만든다.
    2. 우리가 흔히 쓰는 객체에 새로운 `key : value`를 추가하고 싶을 때, `range[key] = value`를 통해 `Symbol.iterator` 키값과 함수로 밸류를 지정해 넣었다.
    3. 추가한 함수는 어떠한 특별한 객체를 return 하게 되어 있고, 이 객체 안에 next()라는 메서드를 정의했다.
    4. 최종적으로 `{value: 값, done: true/false}` 형태의 이터레이터 객체를 return 한다.

### 이터러블, 이터레이터 구분

- **이터러블 객체는 range**이다.
    - `Symbol.iterator` 메서드를 가지고 있기 때문이다.
- **이터레이터 객체는 Symbol.iterator() 메서드가 리턴한 객체가 바로 이터레이터!**
    - 해당 객체 안에는 `{value: 값, done: true/false}`를 리턴하는 next() 메서드가 있기 때문이다.

## 이터러블 객체 순회하기

- 대표적으로 `for…of` 문이 있다.
- 이러한 반복문/연산자들은 Iterable 객체에서 Iterator를 조작하여 Iterator result를 참조해 명령을 실행한다.

### 순회 과정

```tsx
let range = { // 객체 생성
  from: 1,
  to: 5
};

// 1. for..of 최초 호출 시, Symbol.iterator가 호출됩니다.
range[Symbol.iterator] = function() {

  // Symbol.iterator는 이터레이터 객체를 반환합니다.
  // 2. 이후 for..of는 반환된 이터레이터 객체만을 대상으로 동작하는데, 이때 다음 값도 정해집니다.
  return {
    current: this.from,
    last: this.to,

    // 3. for..of 반복문에 의해 반복마다 next()가 호출됩니다.
    next() {
      // 4. next()는 값을 객체 {done:.., value :...}형태로 반환해야 합니다.
      if (this.current <= this.last) {
        return { done: false, value: this.current++ }; // 순회 진행
      } else {
        return { done: true }; // 순회 종료
      }
    }
  };
};

// 이제 의도한 대로 동작합니다!
for (let num of range) {
  alert(num); // 1, 2, 3, 4, 5
}
```

1. `for…of`가 시작되자마자 `for…of`는 `Symbol.iterator`를 호출한다.
2. 이후 `for…of`는 반환된 객체(이터레이터)만을 대상으로 동작한다.
3. `for…of`에 다음 값이 필요하면, `for…of`는 이터레이터의 `next()` 메서드를 호출한다.
4. `next()` 의 반환값은 `{value: any, done: boolean}`과 같은 형태이다.

## 유사배열 vs 이터러블

- 유사 배열: 인덱스와 length 프로퍼티가 있어 배열처럼 보이는 객체
- 다음 예시는 유사 배열 객체이다.
    
    ```tsx
    let arrayLike = { // 인덱스와 length프로퍼티가 있음 => 유사 배열
      0: "Hello",
      1: "World",
      length: 2
    };
    
    for (let item of arrayLike) {} // Symbol.iterator가 없으므로 에러 발생
    ```
    
- 이터러블과 유사배열은 배열이 아니기 때문에 push, pop 등의 메서드를 지원하지 않는다.

## 이터러블 객체 종류

### 문자열

```tsx
for (let char of "test") {
  // 글자 하나당 한 번 실행됩니다(4회 호출).
  alert( char ); // t, e, s, t가 차례대로 출력됨
}
```

- 배열과 문자열은 가장 광범위하게 쓰이는 내장 이터러블이다.

### Map, Set

- 인덱스로 접근하는 것이 아닌 이터러블 프로토콜을 따른다.
- 자체적으로 내장 `forEach()` 메서드를 지원한다.

## 참고 자료

- [https://velog.io/@kimjeongwonn/이터러블이터레이터제네레이터-복습](https://velog.io/@kimjeongwonn/%EC%9D%B4%ED%84%B0%EB%9F%AC%EB%B8%94%EC%9D%B4%ED%84%B0%EB%A0%88%EC%9D%B4%ED%84%B0%EC%A0%9C%EB%84%A4%EB%A0%88%EC%9D%B4%ED%84%B0-%EB%B3%B5%EC%8A%B5)
- [https://inpa.tistory.com/entry/JS-📚-이터러블-이터레이터-💯완벽-이해](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EC%9D%B4%ED%84%B0%EB%9F%AC%EB%B8%94-%EC%9D%B4%ED%84%B0%EB%A0%88%EC%9D%B4%ED%84%B0-%F0%9F%92%AF%EC%99%84%EB%B2%BD-%EC%9D%B4%ED%95%B4)