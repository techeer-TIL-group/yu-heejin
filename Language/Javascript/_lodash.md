## lodash

- 자바스크립트 라이브러리로, 객체, 배열 등의 데이터 구조를 쉽게 변환하여 사용하게끔 도와준다.
- _ 기호를 사용하기 때문에 lodash라고 한다.
- 배열에서 중복값을 제거할 때 uniqBy, unionBy 메소드를 사용한다.
- 배열에서 원하는 객체 데이터를 추출할 때, find 메소드를 사용하고, 제거할 땐 remove 메소드를 사용한다.
- 깊은 복사를 사용할 경우 cloneDeep 메소드를 이용한다.

## 사용 방법

- 설치하기
    
    ```tsx
    npm install --save lodash
    ```
    
- import
    
    ```tsx
    import _ from 'lodash';
    ```
    

## lodash 메소드

### uniqBy

```tsx
_.uniqBy(배열 변수, '고유한 속성의 이름');
```

- 배열 데이터 안의 값에서 **중복되는 값들을 제거한 후 반환**한다.
- 배열 데이터가 하나일 때 사용한다.

### unionBy

```tsx
_.unionBy(배열 변수1, 배열 변수2, '고유한 속성의 이름');
```

- uniqBy와 비슷한데, **uniqBy는 합친 후의 배열 데이터**이고, **unionBy는 합치기 전 배열의 데이터를 합쳐서 중복되는 값을 제거한 후 반환**한다.

### find

```tsx
_.find(배열, { 찾을 객체 데이터 });
```

- 배열 안에서 특정한 객체 데이터만 찾아서 반환한다.

### findIndex

```tsx
_.findIndex(배열, { 찾을 객체 데이터 });
```

- 배열 안에서 특정한 객체 데이터의 index를 반환한다.

### remove

```tsx
_.remove(배열, { 제거할 객체 데이터 });
```

- 배열에서 원하는 객체 데이터를 제거한 후 반환한다.

### cloneDeep

```tsx
_.cloneDeep(value)
```

- 배열 데이터를 깊은 복사 시 사용한다.

## 참고 자료

- [https://goddino.tistory.com/203](https://goddino.tistory.com/203)