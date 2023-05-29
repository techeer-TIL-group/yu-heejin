```tsx
// 아래 객체는 jinju라는 참조를 통해 접근할 수 있다
let jinju = { name: 'Jinju' };

// 그런데 참조를 null로 덮어쓰면 위 객체에 더 이상 도달 불가능하므로 객체가 메모리에서 삭제된다
jinju = null;

//-------------------------------------------

let jinju = { name: 'Jinju' };

let arr = [ jinju ];

jinju = null;	// 참조를 null로 덮어 씀

// jinju를 나타내는 객체는 배열의 요소이기 때문에 가비지 컬렉터의 대상이 되지 않는다
// arr[0]을 이용해서 해당 객체를 얻을 수도 있다
alert(JSON.stringify(arr[0]);
```

- 자바스크립트는 도달 가능한 값을 메모리에 유지한다.
- 자료구조를 구성하는 요소도 **자신이 속한 자료구조가 메모리에 남아있는 동안 대개 도달 가능한 값으로 취급되어 메모리에서 삭제되지 않는다.**
    - **객체의 프로퍼티나 배열 요소, 맵이나 셋을 구성하는 요소들이 이에 해당한다.**

## WeakMap

```tsx
let weakMap = new WeakMap(); //weakMap 선언

let object = {name:'Jason'};

weakMap.set(object, 'hi!'); 
weakMap.set('string', 'hi!'); // error!
```

- `WeakMap`과 `Map`은 비슷한 자료구조
- `WeakMap`은 `Key`로 반드시 객체를 받아와야 한다.
    - **객체만을 key로 가질 수 있다.**
- WeakMap은 가지고 있는 요소 전체를 반복 구문으로 탐색할 방법이 없다.
    - `Iterator`, `keys()`, `values()`, `entries()` 메서드를 지원하지 않기 때문에 key, value값 전체를 얻는 것은 불가능하다.
- 키로 사용된 객체에 대한 유일한 참조가 WeakMap내에만 남아 있을 경우 해당 객체를 가비지 컬렉트 할 수 있다.
    
    ```tsx
    let jinju = { name: 'Jinju' };
    
    let weakMap = new WeakMap();
    weakMap.set(jinju, '...');
    
    jinju = null; // 참조를 덮어씀
    
    // jinju를 나타내는 객체는 이제 메모리에서 지워진다
    ```
    
    - 이것은 애플리케이션 생명 주기 내에서 삭제되어야 할 객체와 관련된 몇몇 메타 데이터를 저장하는 경우 유용하다.

## WeakSet

```tsx
let weakSet = new WeakSet(); // weakSet 선언

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

weakSet.add(john); // weakSet에 데이터를 추가
weakSet.add(pete); 
weakSet.add(john); 

console.log(weakSet.has(john)); // true
weakSet.delete(john); //weakSet 데이터 삭제
console.log(weakSet.has(john)); // false 

console.log(weakSet.has(pete)); // true
pete=null;
console.log(weakSet.has(pete)); // false
```

- **객체만 저장할 수 있다는 점**만 제외한다면 `Set` 객체와 비슷하다.
- 저장된 데이터 값이 null과 같이 가비지 컬렉터의 대상이 된다면, 해당 데이터가 자동으로 삭제되는 특성을 가지고 있다.
    - 도달 가능할 때만 메모리에서 유지된다.
    - WeakSet 내 유일 참조가 남을 경우 해당 객체를 가비지 컬렉트 할 수 있다.
- weakMap처럼 부차적인 데이터를 저장할 때 사용한다.

## 참고 자료

- [https://eloquence-developers.tistory.com/170](https://eloquence-developers.tistory.com/170)
- [https://ko.javascript.info/weakmap-weakset](https://ko.javascript.info/weakmap-weakset)
- [https://overcome-the-limits.tistory.com/564](https://overcome-the-limits.tistory.com/564)
- [https://velog.io/@jeanbaek/JavaScript-Map-WeakMap-Set-WeakSet](https://velog.io/@jeanbaek/JavaScript-Map-WeakMap-Set-WeakSet)
- [https://velog.io/@100pearlcent/Map-Set-WeakMap-WeakSet](https://velog.io/@100pearlcent/Map-Set-WeakMap-WeakSet)