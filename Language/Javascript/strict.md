## Strict Mode

- ECMA5에서 추기된 모드로, ECMA3과 호환성 때문에 존재하지만 ECMA5에서 폐기된 기능들은 사용하지 못하도록 하는 모드
- Strict Mode를 사용하면 안전하지 않은 동작을 수행했을 때 차단되거나 예외가 발생해 더 안전한 스크립트를 작성할 수 있도록 도와준다.

## use strict

- strict mode를 사용하는 명령어
- 프로그램 전체에 사용하고자한다면 스크립트 최상단에 다음과 같이 추가한다.
    
    ```jsx
    "use strict";
    ```
    
- 스크립트 전체가 아닌 함수나 특정 컨텍스트 내에서만 사용할 수 있다.
    
    ```jsx
    function imStrict(){
      "use strict";
      // ... your code ...
    }
    ```
    

## Strict Mode에서 달라지는 점

### 변수와 프로퍼티

- 정의되지 않은 변수에 할당할 수 없다.
    - 즉, foo = “bar”;와 같이 사용하는 경우 오류가 발생한다.
- 프로퍼티의 경우 다음과 같은 상황 발생 시 오류가 발생한다.
    
    ```jsx
    function nonextensible() {
      'use strict';
      var obj = {};
      Object.preventExtensions(obj);
      obj.a = 123; // TypeError: can't define property "a": Object is not extensible
    }
    nonextensible();
    
    function nonwritable() {
      'use strict';
      var obj = {};
      Object.defineProperty(obj, 'a', {
        value: 123,
        writable: false
      });
      obj.a = 456; // TypeError: "a" is read-only
    }
    nonwritable();
    
    function nonconfigurable() {
      'use strict';
      var obj = {};
      Object.defineProperty(obj, 'a', {
        value: 1,
        configurable: false
      });
      delete obj.a; // TypeError: property "a" is non-configurable and can't be deleted
    }
    nonconfigurable();
    ```
    
    - writable 속성이 false인 프로퍼티에 값을 작성하는 경우
    - configurable 속성이 false인 프로퍼티를 지우는 경우
    - extensible 속성이 false인 객체에 프로퍼티를 추가하는 경우
- var로 선언된 변수와 함수 선언문으로 생성된 함수와 함수의 파라미터 삭제가 불가능하다.
    
    ```jsx
    var foo = "test";
    function test(){}
    delete foo; // Error
    delete test; // Error
    
    function test2(arg) {
      delete arg; // Error
    }
    ```
    
- 객체 리터럴에서 같은 프로퍼티를 두 번이상 정의하면 예외가 발생한다.
    - 단, **ES2015에서 허용된다.**

### eval

- eval이라는 이름의 사용 자체를 막고 오류를 발생시킨다.
    
    ```jsx
    // All generate errors...
    obj.eval = ...
    obj.foo = eval;
    var eval = ...;
    for ( var eval in ... ) {}
    function eval(){}
    function test(eval){}
    function(eval){}
    new Function("eval")
    ```
    
- eval 함수를 사용해서 새로운 변수를 정의하는 것도 차단한다.
    
    ```jsx
    eval("var a = false;");
    print( typeof a ); // undefined
    ```
    

### 함수

- 함수의 인자가 담긴 변수인 arguments를 덮어쓰면 오류가 발생한다.
    
    ```jsx
    arguments = [ ... ];  // not allowed
    ```
    
- 파라미터에서 같은 이름의 파라미터를 정의하면 오류가 발생한다.
    
    ```jsx
    function( foo, foo ) {}
    ```
    
- `arguments.caller`와 `arguments.callee`에 접근하면 예외를 던지기 때문에 익명함수를 참조해야 한다면 이름을 지정해야 한다.
    
    ```jsx
    setTimeout(function later(){
      // do stuff...
      setTimeout( later, 1000 );
    }, 1000 );
    ```
    
- 다른 함수에 대한 `arguments`와 `caller` 프로퍼티는 존재하지 않기 때문에 이 프로퍼티들을 정의하는 것이 금지되고 타입 에러를 던진다.
    
    ```jsx
    function test(){
      function inner(){
        // Don't exist, either
        test.arguments = ...; // Error
        inner.caller = ...; // Error
      }
    }
    ```
    
- `null`이나 `undefined`를 전역 객체로 강제하는 경우 오류가 발생한다.
    
    ```jsx
    (function(){ ... }).call( null ); // Exception
    ```
    
- `getter`만 있고 `setter`가 없는 객체의 프로퍼티에 값 할당이 불가능하다.
    
    ```jsx
    // 'use strict';
    var obj = {
      get x() {
        return 0
      }
    };
    obj.x = 123; // TypeError: setting a property that has only a getter
    ```
    

이 외의 다양한 조건들은 아래 블로그에서 확인할 수 있다.

## 참고 자료

- https://blog.outsider.ne.kr/823
- [https://noritersand.github.io/javascript/javascript-엄격-모드-strict-mode/](https://noritersand.github.io/javascript/javascript-%EC%97%84%EA%B2%A9-%EB%AA%A8%EB%93%9C-strict-mode/)