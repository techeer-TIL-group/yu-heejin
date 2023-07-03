## Axios 모듈화

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fef7BLc%2FbtroRZLu0iQ%2FrcqRLurelR564UI38wFDYK%2Fimg.png)

- 필요시 Axios 라이브러리로부터 하나씩 설계하는 것보다 이미 설계된 모듈만 가져와 methos, path, param을 넣어주는 것이 효율적이다.
- apis라는 폴더를 만들고, `axios.create`를 사용하여 axios 인스턴스를 만들 수 있다.
- `axios.create`를 사용하면 API들의 공통 설정을 모두 넣을 수 있다.

## 참고 자료

- [https://0biglife.tistory.com/entry/Axios로-홈뷰-Settingget](https://0biglife.tistory.com/entry/Axios%EB%A1%9C-%ED%99%88%EB%B7%B0-Settingget)
- https://reverse-ant-success.tistory.com/207