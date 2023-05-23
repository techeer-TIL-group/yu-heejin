## sort

```jsx
array.sort([compareFunction])

function compare(a,b) {
	if(a < b) return -1; // a b 순 정렬 
  if(a > b) return 1; // b a 순 정렬
  if(a == b) return 0; // 그대로 ... 
}
```

- 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환한다.
- 기본 정렬 순서는 문자열의 유니코드 포인트를 따른다.
- `compareFunction`이 제공되지 않으면 요소를 문자열로 변환하고, 유니코드 포인트 순서로 문자열을 비교해 정렬한다.
- `compareFunction`이 제공되면 배열 요소는 `compare` 함수의 반환 값에 따라 결정된다.
    - (a, b) < 0 → a, b
    - (a, b) > 0 → b, a
    - (a, b) = 0 → 변경하지 않음

## localeCompare()

```jsx
referenceStr.localeCompare(compareString[, locales[, options]])

'a'.localeCompare('b') // -1 , 
'b'.localeCompare('a') // 1
'c'.localeCompare('c') // 0
```

- 참조 문자열이 정렬 순서에서 앞 또는 뒤에 오는지 또는 주어진 문자열과 같은지 숫자로 반환한다.
- referenceString이 compareString보다 앞에 있으면 -1, 뒤에 있으면 1, 같으면 0 반환

## 참고 자료

- [https://velog.io/@ksh4820/sort-localeCompare](https://velog.io/@ksh4820/sort-localeCompare)
- [https://codinghard.tistory.com/18](https://codinghard.tistory.com/18)