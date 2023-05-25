```jsx
atob(encodedData)
```

- Base64로 인코딩된 문자열 데이터를 디코딩한다.
- 보내는 쪽은 `btoa()`로 인코딩해 전송하고, 받는 쪽에서는 `atob()`로 디코딩한다.
    - 문자열을 base64로 **인코딩**하려면 btoa()
    - base64로 인코딩된 문자열을 **디코딩**하려면 atob()
- 반환 값으로는 `encodedData`를 디코딩한 아스키 문자열이 전송된다.
- `encodedData`: base64 인코딩된 데이터를 담은 이진 문자열

## 참고 자료

- [https://developer.mozilla.org/ko/docs/Web/API/atob](https://developer.mozilla.org/ko/docs/Web/API/atob)
- [https://pro-self-studier.tistory.com/106](https://pro-self-studier.tistory.com/106)