- docker compose를 사용하고 있다면 이미 **여러 컨테이너를 사용하고 있다는 의미**이다.
    - 이러한 여러 컨테이너들은 DB일 수도 있고 웹 애플리케이션일 수도 있다.
- 이러한 서로 다른 컨테이너들이 하나의 volume을 공유해야 한다면 **docker-compose.yml 설정 파일에서 volumes설정을 사용하면 된다.**

## 예시 및 사용 방법

```yaml
services:
  nodejs:
    build:
      context: .
    volumes:
      - .:/usr/src/app
    ports:
      - “80:80”
  db:
    build:
      context: ./db
    volumes:
      - .:/var/lib/mysql
    ports:
      - “6630:3306”
```

- 위 예시는 nodejs와 db 모두 volumes 속성이 명시되어 있지만, 별도의 volumes를 사용하기 때문에 서로 다른 파일 시스템으로 이루어져 있다.
- 두 컨테이너가 같은 volume을 사용해야 한다면 설정 파일 맨 아래에 공유할 volume의 이름을 추가한다.
    
    ```yaml
    services:
      nodejs:
        build:
          context: .
        volumes:
          - shared-data:/usr/src/app
        ports:
          - “80:80”
      db:
        build:
          context: ./db
        volumes:
          - shared-data:/usr/src/mysql
        ports:
          - “6630:3306”
    volumes:
      shared-data:
    ```
    
    - db 컨테이너의 volumes 경로가 `/var/lib/mysql` → `/usr/src/mysql`로 변경되었다.
- volumes를 공유해서 사용 시 주의할 점은 **공유할 볼륨의 이름을 컨테이너에 명시하는 것뿐만 아니라 경로도 같아야 한다!**
    - 만약 nodejs 컨테이너의 볼륨 경로가 다음과 같이 변경되었다면
        
        ```yaml
        - shared-data:/test/nodejs
        ```
        
    - db 컨테이너의 볼륨 경로도 다음처럼 바꿔야 같은 볼륨을 두 컨테이너에서 동시에 사용할 수 있다.
        
        ```yaml
        - shared-data:/test/mysql
        ```
        
    - 수정한 docker compose를 실행하면 nodejs 디렉토리와 mysql 디렉토리가 /test 경로 밑에 생기는 것을 확인할 수 있다.

## 참고 자료

- [https://medium.com/@su_bak/docker-compose에서-서로-다른-container가-같은-volume을-공유하는-방법-5e49430c5282](https://medium.com/@su_bak/docker-compose%EC%97%90%EC%84%9C-%EC%84%9C%EB%A1%9C-%EB%8B%A4%EB%A5%B8-container%EA%B0%80-%EA%B0%99%EC%9D%80-volume%EC%9D%84-%EA%B3%B5%EC%9C%A0%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-5e49430c5282)