## 객체지향 프로그래밍

- **자바스크립트는 프로토타입 기반의 객체지향 프로그래밍 언어**이며, 자바스크립트를 이루고 있는 거의 모든 것은 객체이다.
- 이 때, **객체란 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조**를 말하며, 원시타입의 값을 제외한 **나머지(함수, 배열, 정규 표현식 등)는 모두 객체**이다.
- **객체지향 프로그래밍이란 이러한 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임**을 말한다.
- 객체지향 프로그래밍은 **객체의 상태를 나타내는 데이터와 상태 데이터를 조작할 수 있는 동작을 하나의 논리적인 단위로 묶어 생각**한다.
    - 객체란 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조라 할 수 있다.

## 자바스크립트의 프로토타입

![Untitled](https://velog.velcdn.com/images/ssulv3030/post/57633f85-3ef5-4300-9e98-cb007a83bfb1/image.png)

- 자바스크립트는 객체지향 프로그래밍 언어이다.
- C++, Java는 클래스 기반의 객체지향 프로그래밍 언어이며, **Javascript는 프로토타입 기반 객체지향 프로그래밍 언어이다.**
- 자바스크립트의 모든 타입(객체)은 `[[Prototype]]`을 통해 이를 담당하는 부모 객체와 연결 되도록 한다.
- 프로토타입이란 **어떤 객체의 상위 객체의 역할을 하는 객체로써, 다른 객체에 공유 프로퍼티들을 제공한다.**
    - 프로토타입을 상속받은 하위 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.
- 모든 객체는 `[[Prototype]]`이라는 내부 슬롯을 가지며, 이 내부 슬롯의 값은 프로토타입의 참조이다.
- 모든 프로토타입은 생성자 함수와 연결되어 있고, `[[Prototype]]` 내부 슬롯에는 직접 접근할 수 없지만, `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입에 간접적으로 접근할 수 있다.
- number, string, function, array 등의 타입을 사용할 때마다 우리가 따로 메서드 작업을 하지 않음에도, 우리는 자연스럽게 해당 타입의 내장 메서드를 사용할 수 있었다.
    - 이는 해당 타입 객체 안의 `[[Prototype]]`이 `Type.prototype`을 참조하여 메서드들을 가져오기 때문이다.
    - 따라서 자바스크립트에서 프로토타입이란, 이러한 내장 메서드를 사용할 수 있듯이 **해당 객체에 대한 내장 함수 혹은 직접 만든 함수를 공유할 수 있도록 제공하는 객체라 표현할 수 있다.**

### __proto__ 접근자 프로퍼티

- 모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 `[[Prototype]]` 내부 슬롯에 간접적으로 접근할 수 있다.
- `Object.prototype`의 접근자 프로퍼티인 `__proto__`는 `getter/setter` 함수라고 부르는 **접근자 함수를 통해 내부 슬롯의 값을 취득하거나 할당한다.**
    
    ```jsx
    const obj = {};
    const parent = { x: 1 };
    //getter
    obj.__proto__;
    //setter
    obj.__proto__ = parent;
    
    console.log(obj.x); //1
    ```
    
    - 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 `Object.prototype`의 프로퍼티이며, 모든 객체는 상속을 통해 `Object.prototype.__proto__`접근자 프로퍼티를 사용할 수 있다.
- `__proto__`를 통해 프로토타입에 접근하는 이유는 **상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서이다.**
    - **프로토타입 체인은 단방향 링크드리스트로 구현되어야 한다.**
- `__proto__`를 코드 내에서 직접 사용하는 것은 권장하지 않으며, 그 대신 `Object.getPrototypeOf`, `Object.setPrototypeOf`를 사용할 수 있다.

### 생성자 함수

```jsx
const arr = [1,2,3];
// 사실 요런식으로 생성되는 것이다. -> const arr = new Array(1,2,3);
```

- 클래스를 사용하지 않는 프로토타입 기반의 객체지향 언어인 **자바스크립트에서는 생성자 함수와 new를 사용하여 함수가 새로운 객체를 반환하는 것이 가능하다.**
- 자바스크립트에서 사용하는 number, string, function 등 다양한 타입 모두 해당 타입 객체 기반의 생성자 함수를 통해 만들어지게 되는 것이다.
    - 단, 함수의 경우 `[[Prototype]]`외에도, `prototype`이라는 속성 또한 지니게 된다.
    - 향후 생성자 함수의 인스턴스를 생성했을 때, 부모의 prototype 속성을 살펴보게 된다.
    - 간단하게 말해 `[[Prototype]]`은 모든 타입에서 나타나는 속성이며, `prototype`은 함수에서만 나타나는 속성이다.

### proto

![Untitled](https://velog.velcdn.com/images/beberiche/post/8badfd7f-a654-4a53-bde2-4e455fa05077/image.png)

- `new`를 통해 반환된 인스턴스가 가지게 되는 속성으로, `__proto__`를 통해 생성자 함수의 `prototype`을 참조할 수 있게 된다.
    - 즉, `__proto__`가 생성자 함수를 상속하는 것이다.
- `prototype` 객체 안에는 `constructor` 함수를 가지고 있고, 이 `constructor` 객체는 `prototype` 속성을 지녔던 생성자 함수를 가리키고 있다.

### 프로토타입 체이닝

![Untitled](https://velog.velcdn.com/images/beberiche/post/e8e5431e-50fa-4f7a-a714-9a5885ff5ffd/image.png)

- 해당 인스턴스 혹은 객체에 찾는 메서드가 존재하지 않다면, `__proto__` 혹은 `[[Prototype]]`을 통해 부모가 되는 객체의 `prototype`의 메서드를 탐색하게 된다.
- 만약 해당 탐색 객체에도 메서드가 존재하지 않으면 상위 부모 객체의 `prototype`까지 확인하며, 이는 최상위 부모 객체인 `Object.prototype`까지 확인한다.
- 탐색 과정에서 찾게 되면 상위 객체의 `prototype`에서 해당 메서드를 불러오게 되는데, 이 과정을 **프로토타입 체이닝**이라고 한다.

### 함수 객체의 prototype 프로퍼티

- 함수 객체만이 소유하는 `prototype` 프로퍼티는 **생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**
- `prototype` 프로퍼티는 **생성자 함수가 생성할 객체의 프로토타입을 가리킨다.**
    - 따라서 생성자 함수로서 호출할 수 없는 함수, **즉 `non-constructor`인 화살표 함수와 ES6 축약 표현으로 정의한 메서드는 `prototype` 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.**

## 자바스크립트의 상속

- 상속은 객체지향 프로그래밍의 핵심 개념으로, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.
- 자바스크립트는 다른 프로그래밍 언어의 클래스 기반 상속과는 다른 **생성자 함수를 활용한 프로토 타입 기반 상속을 사용한다.**
- 자바스크립트의 모든 객체는 **객체가 상속하는 속성과 메서드의 템플릿 역할을 하는 프로토타입 객체**를 가지고 있으며, **만약 객체 자체에 해당 속성과 메서드가 존재하지 않는 경우, 프로토타입으로부터 속성이나 메서드를 찾는 과정이 진행된다. (프로토타입 체이닝)**

### 생성자 함수 사용

```jsx
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    return Math.PI * this.radisus ** 2;
  };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);

// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); //false

function CircleByPrototype(radius) {
  this.radius = radius;
}
```

- 생성자 함수를 사용하여 인스턴스를 생성하면 `getArea`메서드를 모든 인스턴스가 중복 생성한다.

### 프로토타입 사용하기

```jsx
function CircleByPrototype(radius) {
  this.radius = radius;
}

CircleByPrototype.prototype.getArea = function () {
  return Math.PI * this.radisus ** 2;
};

// 반지름이 1인 인스턴스 생성
const CircleByPrototype1 = new CircleByPrototype(1);

// 반지름이 2인 인스턴스 생성
const CircleByPrototype2 = new CircleByPrototype(2);

console.log(CircleByPrototype1.getArea === CircleByPrototype2.getArea); //true
```

- 프로토타입을 사용하면 모든 인스턴스는 자신의 프로토타입, 즉 상위 객체의 프로토타입의 모든 프로퍼티와 메서드를 상속받는다.
- 동일한 내용의 메서드를 중복 생성하지 않고, 모든 인스턴스가 `getArea`메서드를 상속받아 사용할 수 있다.

## Object.create

```jsx
Object.create(proto[, propertiesObject])
```

- **기존 객체의 속성들을 상속받는 새로운 객체를 생성할 때 사용하는 메서드**이다.
- proto: 새로 만들 객체의 프로토타입이 될 객체
- propertiesObject: 새로 만든 객체에 추가될 속성

```jsx
function Character(name, forceUser) {
	this.name = name;
	this.forceUser = forceUser;
}

function Jedi(name, lightsaberColor) {
	Character.call(this, name, true);
	this.lightsaberColor = lightsaverColor;
}

Jedi.prototype = new Character();
// Jedi.prototype = Object.create(Character.prototype);
Jedi.prototype.constructor = Jedi;

console.log(Jedi.prototype);

// **new Character();**
// Jedi {
// 	name : undefined,
// 	forceUser: undefined,
// 	constructor : [Function: Jedi]
// }

// **Object.create(Character.prototype);**
// Jedi {
// 	constructor : [Function: Jedi]
// }
```

- `new` 인스턴스로 받는 것과 유사하다.
- 단, `Object.create`로 생성하는 경우, `constructor`가 설정되지 않고 객체만 생성한다.
- `new` 인스턴스의 경우, name, forceUser를 가지고 있지만, Object.create의 경우는 가지고 있지 않다.
    - **`new`를 통한 인스턴스의 경우 `prototype`에 대한 `Character`의 속성값을 그대로 가져온다.**
    - Jedi의 경우 이미 `Character.call`을 통해 본인의 name값을 가지고 있기 때문에, **프로토타입의 name은 불필요한 속성이 된다.**
    - 이 때, `Object.create`를 활용하면 Character.prototype만 받아오도록 할 수 있다.

![Untitled](https://velog.velcdn.com/images/beberiche/post/1d822c77-95f0-40cc-a00e-c5ac0e22984d/image.png)

![Untitled](https://velog.velcdn.com/images/beberiche/post/85b1567c-e2d7-4cd5-ba1f-203c680d044f/image.png)

## Mixin

```jsx
const walkMixin = {
  walk() {
    console.log(`${this.name} is walking`);
  }
};

const runMixin = {
  run() {
    console.log(`${this.name} is running`);
  }
};

const swimMixin = {
  swim() {
    console.log(`${this.name} is swimming`);
  }
};

function Athlete(name) {
  // Add any additional properties to the object
  this.name = name;

	// 이렇게 mixin 함수를 따로따로 불러오는 것도 가능
  // walkMixin.walk.call(this);
  // runMixin.run.call(this);
  // swimMixin.swim.call(this);
}

// Object.assign 통해 하나의 새로운 객체로 병합한 후 이를 상속 객체로 지정하는 것이다.
Object.assign(Athlete.prototype, walkMixin, runMixin, swimMixin);

const athlete = new Athlete("John");
athlete.walk(); // output: "John is walking"
athlete.run(); // output: "John is running"
athlete.swim(); // output: "John is swimming"
```

- 객체가 단일 클래스나 프로토타입이 아닌 **여러 소스로부터 속성과 메서드를 상속할 수 있도록 하는 설계 패턴**
- 프로토타입 기반 상속을 사용하는 자바스크립트의 경우, **모든 객체에 대한 프로토타입 속성은 하나로 분류되기 때문에 단일 상속만을 지원하고 있다.**
    - 이러한 부분을 자바스크립트에서 믹스인을 사용하여 **다중 상속과 유사하도록 만드는 것이 가능하다.**

## 참고 자료

- [https://velog.io/@beberiche/자바스크립트에서의-상속에-대해-설명해주세요](https://velog.io/@beberiche/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%97%90%EC%84%9C%EC%9D%98-%EC%83%81%EC%86%8D%EC%97%90-%EB%8C%80%ED%95%B4-%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94)
- [https://velog.io/@beberiche/프로토타입이-뭔가요](https://velog.io/@beberiche/%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EC%9D%B4-%EB%AD%94%EA%B0%80%EC%9A%94)
- [https://velog.io/@ssulv3030/자바스크립트는-상속을-어떻게-구현할까](https://velog.io/@ssulv3030/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%83%81%EC%86%8D%EC%9D%84-%EC%96%B4%EB%96%BB%EA%B2%8C-%EA%B5%AC%ED%98%84%ED%95%A0%EA%B9%8C)