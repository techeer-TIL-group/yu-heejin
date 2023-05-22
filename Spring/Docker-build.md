## Gradle에서 Docker 이미지 생성

```docker
FROM openjdk:11-jre-slim
ARG JAR_FILE=build/libs/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

- Gradle의 경우, ./gradlew bootJar를 통해 jar 파일을 만들 수 있다.
- jar를 이용해 쉽게 Docker 이미지를 만들 수 있다.

### 위 코드의 문제점

- 프로젝트가 커지면서 의존성 및 코드의 양이 증가되고, jar 파일이 무거워질수록 컨테이너 이미지를 빌드하는 시간이 더 오래걸린다.
- Docker는 기존 layer가 변화되지 않으면 재사용(캐시 사용)하는 방식으로 진행되기 때문에 효율적인 아키텍처를 가지고 있다.
- 위 코드처럼 구성하면 자바의 모든 구조가 jar 파일이 되기 때문에 layer를 재사용할 수 없으며, 코드를 한 줄이라도 수정하게 되면 캐싱이 불가능하기 때문에 처음부터 이미지를 다시 빌드한다.

## Layering Feature

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb44hTl%2Fbtq76ztzhuP%2FaXBWaLMLKCa5K5cSBt1ap1%2Fimg.png)

- 위와 같은 문제를 해결하기 위해 layering feature를 사용한다.
- 쉽게 말해 하나로 이루어졌던 스프링 부트 구조를 4개의 레이어로 나누어 변경이 적은 부분은 캐싱된 것을 사용하게 만들고, 변경이 잦은 부분만 새로 빌드해 효율적으로 컨테이너 이미지를 만들 수 있다.
- 보통 dependencies의 변경이 가장 적고, application의 변경이 제일 잦다.

## 사용하기

- 다음과 같은 명령어를 통해 jar 파일을 4개의 레이어로 분리할 수 있다.
    
    ```bash
    java -Djarmode=layertools -jar build/libs/trip_in_world-0.0.1-SNAPSHOT.jar extract
    ```
    
    - jar 파일의 경로를 잘 찾아주는 것이 중요하다.
- Dockerfile을 다음과 같이 작성한다.
    
    ```docker
    FROM adoptopenjdk:11-jre-hotspot as builder
    WORKDIR application
    ARG JAR_FILE=build/libs/*.jar
    COPY ${JAR_FILE} application.jar
    RUN java -Djarmode=layertools -jar application.jar extract
    
    FROM adoptopenjdk:11-jre-hotspot
    WORKDIR application
    COPY --from=builder application/dependencies/ ./
    COPY --from=builder application/spring-boot-loader/ ./
    COPY --from=builder application/snapshot-dependencies/ ./
    COPY --from=builder application/application/ ./
    ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]
    ```
    

## 참고 자료

- [https://binux.tistory.com/62](https://binux.tistory.com/62)
- [https://medium.com/@gaemi/spring-boot-과-docker-with-jib-657d32a6b1f0](https://medium.com/@gaemi/spring-boot-%EA%B3%BC-docker-with-jib-657d32a6b1f0)