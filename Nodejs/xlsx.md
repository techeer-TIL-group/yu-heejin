## xlsx 모듈

- 동기 방식 / 비동기 방식 설정 가능
- JSON 객체 반환

## 모듈 import

```jsx
// commonJS
const xlsx = require('xlsx');

// ECMA
import * as xlsx from 'xlsx';
```

## 파일 읽기

```jsx
const workBook = xlsx.read(data, read_options);

const workBook = xlsx.readFile(fileName, read_options);
```

## 시트 정보 추출

![Untitled](https://velog.velcdn.com/images%2Fhahaha%2Fpost%2F20568ada-1472-403d-ac84-51842ac31453%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-11-01%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.28.25.png)

```jsx
// 첫번째 시트 이름
const sheetName = workBook.sheetNames[0];

// 해당 시트의 데이터 정보
const sheet = workBook.Sheets[sheetName];

// 해당 시트의 데이터를 JSON 형식으로 표시
// [ { name: 'kim', age: 23 }, { name: 'park', age: 24 } ]
const data = xlsx.utils.sheet_to_json(sheet);
```

## 참고 자료

- [https://velog.io/@hahaha/Node.js-xlsx-모듈](https://velog.io/@hahaha/Node.js-xlsx-%EB%AA%A8%EB%93%88)