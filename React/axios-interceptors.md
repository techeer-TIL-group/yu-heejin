## Interceptors

- then, catch로 처리되기 전에 요청과 응답을 가로챌 수 있다.
- 가로챈 후 어떠한 작업을 수행한다.

## 적용 과정

1. axios 인스턴스 생성
    
    ```tsx
    const instance = axios.create({
        baseURL: this.requestUrl,
        timeout: 30000,
        withCredentials: true,
    });
    ```
    
2. 요청/응답 인터셉터 추가
    
    ```tsx
    instance.interceptors.request.use(
      async (config) => {
          // TODO: redux 사용 변경
          const accessToken = await EncryptedStorage.getItem('accessToken');
    
          if (accessToken) {
              config.headers['Authorization'] = `Bearer ${accessToken}`;
          }
    
          return config;
      },
      (error) => {
          return Promise.reject(error);
      }
    )
    
    instance.interceptors.response.use(
      (response) => {
          return response;
      },
      async (error) => {
          const { response } = error;
    
          // accessToken, refreshToken이 만료된 경우 or 로그인을 하지 않은 경우
          if (response.status === 401) {
              await validateToken.requestTokenApi(response.data.message);
              return response;
          }
    
          return Promise.reject(error);
      }
    )
    ```
    

## 참고 자료

- https://leeseong010.tistory.com/133
- https://axios-http.com/kr/docs/interceptors
- [https://maruzzing.github.io/study/rnative/axios-interceptors로-토큰-리프레시-하기/](https://maruzzing.github.io/study/rnative/axios-interceptors%EB%A1%9C-%ED%86%A0%ED%81%B0-%EB%A6%AC%ED%94%84%EB%A0%88%EC%8B%9C-%ED%95%98%EA%B8%B0/)