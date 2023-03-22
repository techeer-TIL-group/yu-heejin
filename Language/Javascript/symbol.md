## Symbol이란?

- ES6부터 새롭게 추가된 원시 타입 중 하나
- 고유하고 변경할 수 없는 식별자를 생성하며, 한 번 생성하면 복사할 수 없다.
- 객체의 고유한 프로퍼티 키를 만들기 위해 사용된다.
- 모든 심볼 값은 고유하고 중복되지 않으며, 한 번 생성되면 그 값은 변경되지 않기 때문에 충돌 위험이 없는 타입이다.
- new 연산자를 사용한 문법을 지원하지 않기 때문에 생성자 측면에선 불완전한 내장 객체 클래스라고 볼 수 있다.

## 생성

```jsx
const sym = Symbol();
const symbol = Symbol();

console.log(sym); // Symbol()
console.log(sym === symbol); // false
console.log(typeof sym); // symbol
```

- 심볼은 Symbol() 함수로 생성할 수 있으며, 이 함수로 생성된 심볼의 타입은 객체가 아닌 symbol이다.
- 변경 불가능하고 고유한 원시 타입답게 이 함수는 호출될 때마다 새로운 값을 생성한다.

```jsx
const sym = Symbol("moon");
const symbol = Symbol("moon");

console.log(sym.description); // "moon"
console.log(symbol.description); // "moon"

console.log(sym === symbol); // false
```

- Symbol() 함수는 인자에 문자열을 전달할 수 있으나, **이 문자열은 해당 심볼에 대한 설명을 담기 위한 주석인 description 속성**만을 의미할 뿐 심볼 자체에는 아무 영향을 주지 않는다.
- 따라서, **같은 문자열을 전달하여 심볼 두 개를 생성하더라도 해당 심볼들은 서로 다른 값을 담고 있다.**

## 활용

### 고유한 프로퍼티 키 생성

```jsx
const obj = {};
const sym = Symbol("moon");

obj[sym] = "moon";

console.log(obj); // { [Symbol(moon)]: "moon"}
console.log(obj[sym]); // "moon"
```

- 객체의 프로퍼티 키를 생성할 때 심볼을 사용하면 **다른 어떠한 프로퍼티와도 충돌하지 않는 키를 생성할 수 있다.**

### 프로퍼티 키 감추기

```jsx
const obj = {};
const sym = Symbol("moon");

obj[sym] = "moon";
obj["str"] = "string";
obj.hello = "hello";

for (let i in obj) {
  console.log(i); // "str" "hello"
}

console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(moon)]
```

- 심볼로 만들어진 키는 for…in 루프, Object.keys(), Object.getOwnPropertyNames()와 같은 객체 조사 메서드에서 무시되며, Object.getOwnPropertySymbols()를 통해서만 가져올 수 있다.
- 이러한 특성을 활용하면 심볼로 선언된 키 값들을 감추는 것도 가능하다.

### Symbol 객체 활용

- Symbol() 함수로 심볼 값을 생성할 수 있다는 것은 이것이 함수 객체라는 의미이다.
- 이 객체 내부에는 다양한 프로퍼티와 메서드가 존재하는데, 이들 중 **length와 prototype을 제외한 나머지를 Well-Known Symbol이라고 부른다.**
- 자바스크립트 엔진은 동작하는 과정에서 객체들에 대해 이 심볼들의 참조를 시도하는데, 만약 참조가 가능하다면 그 객체는 해당 심볼들이 가진 코드 동작이 가능한 것으로 간주한다.
- 예를 들어, Symbol.iterator를 가진 객체의 경우 자바스크립트 엔진은 이 객체가 이터레이션 프로토콜을 따르는 것으로 간주하고 이터레이터로 동작하도록 한다.
    
    ```jsx
    const array = ["a", "b", "c"];
    
    const iterator = array[Symbol.iterator]();
    
    console.log(iterator.next()); // { value: 'a', done: false }
    console.log(iterator.next()); // { value: 'b', done: false }
    console.log(iterator.next()); // { value: 'c', done: false }
    console.log(iterator.next()); // { value: undefined, done: true }
    ```
    
    - **배열에는 Array.prototype[Symbol.iterator]가 구현되어 있기 때문에 해당 심볼 프로퍼티 키에 접근이 가능**하고, 이에 따라 자바스크립트 엔진은 **Symbol.iterator를 프로퍼티 키로 사용한 메서드를 이터레이터로 동작하도록 반환했다.**
- 또 다른 예시로 Symbol.for 메서드가 있는데, 이 메서드는 **문자열을 인자로 전달 받아 해당 문자열을 키로 사용하여 전역 심볼 레지스트리에서 키와 일치하는 심볼 값을 탐색**한다.
    
    ```jsx
    const sym1 = Symbol.for("moon"); // 탐색 실패, 전역 레지스트리에 moon이라는 키로 새로운 심볼 생성
    const sym2 = Symbol.for("moon"); // 탐색 성공, 전역 레지스트리의 moon이라는 키에 해당하는 심볼 반환
    
    console.log(sym1 === sym2); // true
    ```
    
    - 이 때 탐색에 성공하면 해당 심볼 값을 반환하고 실패하면 해당 키로 새로운 심볼 값을 생성하여 저장한다.
    - 즉, sym1, sym2는 같은 심볼을 참조하며 이를 서로 공유하고 있다.

## 참고 자료

- [https://moon-ga.github.io/javascript/symbol/](https://moon-ga.github.io/javascript/symbol/)