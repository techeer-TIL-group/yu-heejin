## 삼항 연산자 (Ternary Conditional)

```jsx
var isArthur = true;
var weapon;

if(isArthur) {
  weapon = "Excalibur";
} else {
  weapon = "Longsword";
}
```

- 삼항 연산자를 사용하면 위 if-else 문을 다음과 같이 바꿀 수 있다.
    
    ```jsx
    var weapon = isArthur ? "Excalibur" : "Longsword";
    ```
    
    ```jsx
    // 두개 이상의 변수를 이용하여 값을 받는 경우
    isArthur && isKing ? (weapon = "Ex", helmet = "Goose")
                         :
                         (weapon = "ln", helmet = "Iron")
    
    // 즉시 실행함수로 값을 받는 경우
    isArthur && isKing ? function () {
                            // ...
                         }();
                         :
                         function () {
                            // ...
                         }();
    ```
    

## Logical Assignment - OR

```jsx
// 삼항연산자 사용
function addSword(sword) {
  this.swords = swords ? swords : [ ];
  this.swords = push.(sword);
}

// OR 연산자 사용
function addSword(sword) {
  this.swords = swords || [ ];
}

// OR 연산자 잘못 사용한 예
// 아래의 경우 계속 new array 를 할당함.
function addSword(sword) {
  this.swords = [ ] || swords;
}
```

- OR 연산자: falsy하지 않은 가장 첫번째 값을 갖는다.

## Logical Assignment - AND

```jsx
var result1 = "King" && "Arthur";
console.log(result1); //Arthur
var result2 = "Arthur" && "King";
console.log(result2); // King
```

- OR 연산자와 다르게 두 개의 truthy한 값이 있으면 마지막으로 확인한 truthy 값이 리턴된다.
- falsy 값이 있는 경우 OR 연산자와 동일하게 동작한다.

## Switch Blocks

```jsx
var regimnet = 3;

if (regiment == 1) {
  ...
} else if (regiment == 2) {
  ...
} else if (regiment == 3) { // 앞 1,2 를 거쳐 3으로 온다.
  ...
}

switch (regiment) {
  case 1:
    ...
  case 2:
    ...
  case 3: // 3으로 바로 온다.
    ...
}
```

- `if-else`문과 `switch`문의 차이점은, 순차적으로 모든 `if`문을 도느냐 아니면 해당하는 case로 바로 가서 불필요한 연산을 줄이느냐의 차이이다.

## Loop Optimization

```jsx
treasureChest = {
  necklaces: ["A", "B", "C", "D"];
};

for (var i = 0; i < treasureChest.necklaces.length; i++) {
  console.log(treasureChest.necklaces[i]);
}
```

- 컴퓨터 메모리 관점에서 일반적인 `for` 문의 연산 순서는 다음과 같다.
    1. i값 탐색
    2. treasureChest 객체 탐색
    3. necklaces 속성 탐색
    4. necklaces 속성의 배열 인덱스 탐색
    5. length 프로퍼티 검색

### 최적화한 연산

```jsx
// 1. length property 를 한번만 접근 (기존 for 문은 반복시 마다 접근)
  var x = treasureChest.necklaces.length;
  for (var i = 0; i < x; i++) {
    console.log(treasureChest.necklaces[i]);
  }
```

```jsx
// 2. for 문의 초기 선언문 쪽에서 x 값을 선언하면, 전역 변수로 var x 를 선언하지 않아도 되어 메모리가 더 효율적이게 된다.
for (var i = 0, x = treasureChest.necklaces.length; i < x; i++) {
  console.log(treasureChest.necklaces[i]);
}
```

- 자바스크립트 `var`는 `{}`로 스코핑 되어있지 않기 때문에 위의 `for` 반복문이 끝나면 `x` 값은 최종 값으로 할당되어 있다.

```jsx
// 3. 각 반복 싸이클마다 treasureChest 객체의 속성에 접근을 할 필요가 없다.
var list = treasureChest.necklaces;
for (var i = 0, x = treasureChest.necklaces.length; i < x; i++) {
  console.log(list[i]);
}
```

- 모든 인덱스를 접근할 땐 `for loop`가 좋고, 때로는 `for in`보다 성능이 나은 경우가 있다.
    - `for in`은 prototype에 접근하여 기존의 기 정의된 메서드까지 포함하여 출력하므로 비효율적이다.

## Performance - Inheritance

```jsx
function SignalFire(id, logs) {
  this.id = id;
  this.logs = logs;

  this.functionality1: function() {

  },
  this.functionality2: function() {

  },
  this.functionality3: function() {

  },
}
```

- 위 생성자 함수의 경우 매번 객체를 생성할 때마다 사용하지 않는 메서드들을 메모리에서 사용하는 낭비가 발생한다.
- 따라서 매번 객체 생성시 필요한 속성이나 메서드만 가져가도록 하고, 공통 메서드는 다음과 같이 프로토타입으로 뺀다.
    
    ```jsx
    SignalFire.prototype = {
      functionality1: function() {
    
      },
      functionality2: function() {
    
      },
      functionality3: function() {
    
      },
    }
    ```
    

## Performance - Get rid of var redundancy

```jsx
var a = 1;
var b = "hello";
var c = ["a","b","c"];

var a = 1,
    b = "hello",
    c = ["a","b","c"];

// 코드의 가독성이 높아지고, 간결하다.
```

- var 지정어를 사용할 때 다음과 같이 코드량을 줄일 수 있다.

## String Concatenation

```jsx
var page = "";
for (var i = 0, x =  newPageBuild.length ; i < x ; i++) {
  page += newPageBuild[i];
}

// join() 메서드 활용
page = newPageBuild.join("\n");
```

- 문자열 길이에 따라 `+=` 연산자와 join() 메서드의 성능 차이가 발생한다.
    - 문자열이 짧으면 `+=` 연산자가 성능이 더 빠르다.
    - 문자열이 길고, 문자열이 배열안에 리스트 형태로 저장되어 있을 때 `join()` 메서드가 성능이 더 우월하다.
    

## 참고 자료

- https://joshua1988.github.io/web-development/javascript/javascript-best-practices/