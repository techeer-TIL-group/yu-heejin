## PostgreSQL docker-compose 파일 설정

```yaml
version: '3'

services:
  database:
    image: postgres:latest
    container_name: database
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      POSTGRES_DB: ${DB_NAME}
```

- volume은 향후 설정 예정

## build.gradle 설정

```java
import org.jetbrains.kotlin.gradle.tasks.KotlinCompile

plugins {
    id("org.springframework.boot") version "2.6.1"
    id("io.spring.dependency-management") version "1.1.0"
    kotlin("jvm") version "1.7.22"
    kotlin("plugin.spring") version "1.7.22"

		// JPA 플러그인
    kotlin("plugin.jpa") version "1.7.22"
    kotlin("kapt") version "1.7.22"
    kotlin("plugin.allopen") version "1.7.22"
}

group = "com.kotlin.study"
version = "0.0.1-SNAPSHOT"
java.sourceCompatibility = JavaVersion.VERSION_11

// QueryDSL
val querydslVersion = "4.4.0"

configurations {
    compileOnly {
        extendsFrom(configurations.annotationProcessor.get())
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation("org.springframework.boot:spring-boot-starter-web")
    implementation("com.fasterxml.jackson.module:jackson-module-kotlin")
    implementation("org.jetbrains.kotlin:kotlin-reflect")
    compileOnly("org.projectlombok:lombok")
    annotationProcessor("org.projectlombok:lombok")
    testImplementation("org.springframework.boot:spring-boot-starter-test")

    // JPA
    implementation("org.springframework.boot:spring-boot-starter-data-jpa")
    runtimeOnly("org.postgresql:postgresql")

    // queryDSL
    implementation("com.querydsl:querydsl-jpa:$querydslVersion")
    kapt("com.querydsl:querydsl-apt:$querydslVersion:jpa")
    kapt("org.springframework.boot:spring-boot-configuration-processor")
}

// JPA
allOpen {
    annotation("javax.persistence.Entity")
    annotation("javax.persistence.MappedSuperclass")
    annotation("javax.persistence.Embeddable")
}

tasks.withType<KotlinCompile> {
    kotlinOptions {
        freeCompilerArgs = listOf("-Xjsr305=strict")
        jvmTarget = "11"
    }
}

tasks.withType<Test> {
    useJUnitPlatform()
}
```

## application.yml 파일 작성

```yaml
spring:
  profiles:
    include:
      - postgres

  jpa:
    properties:
      hibernate:
        ddl-auto: create   
        # 기존 테이블 삭제 후 새로 생성 (운영환경에서는 권장되지 않음)
        show_sql: true
        format_sql: true   
        # sql 출력문의 모양을 잡아준다.
        use_sql_comments: true   
        # 콘솔에 표시되는 쿼리문 위에 어떤 실행을 하려는지 힌트 표시, 주석 표시
    generate-ddl: true   
    # 앱 시작 시 @Entity로 정의한 테이브르이 create문 실행
```

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/   
    # docker에서 실행하지 않을 경우 ip주소는 localhost
    username: 
    password: 
    driver-class-name: org.postgresql.Driver
```

- 현재 코프링 프로젝트는 Docker 컨테이너에서 돌리지 않기 때문에 url ip 주소는 localhost로 설정해야 한다.

## QueryDslConfig 파일 작성하기

```kotlin
package com.kotlin.study.dongambackend.config

import com.querydsl.jpa.impl.JPAQueryFactory
import org.springframework.context.annotation.Bean
import org.springframework.context.annotation.Configuration
import org.springframework.data.jpa.repository.query.JpaQueryMethodFactory
import javax.persistence.EntityManager
import javax.persistence.PersistenceContext

@Configuration
class QueryDslConfig {
    @PersistenceContext
    lateinit var entityManager: EntityManager

    @Bean
    fun jpaQueryFactory(): JPAQueryFactory {
        return JPAQueryFactory(entityManager);
    }
}
```

## Test 코드 작성하기

- entity
    
    ```kotlin
    package com.kotlin.study.dongambackend
    
    import javax.persistence.Column
    import javax.persistence.Entity
    import javax.persistence.GeneratedValue
    import javax.persistence.Id
    
    @Entity
    data class TestEntity(var _name: String) {
        @Id
        @GeneratedValue
        var id: Int? = null
    
        @Column
        var name: String = _name
    }
    ```
    
- 정상 작동 확인용 controller
    
    ```kotlin
    package com.kotlin.study.dongambackend
    
    import org.springframework.web.bind.annotation.GetMapping
    import org.springframework.web.bind.annotation.RestController
    
    @RestController
    class TestController {
        @GetMapping("/")
        fun index(): String {
            return "Hello World"
        }
    }
    ```
    

## 실행 결과

<img width="419" alt="스크린샷 2023-03-18 오후 7 02 51" src="https://user-images.githubusercontent.com/96467030/226101520-66142bea-a83c-4d13-bd4a-ceea7a5f9569.png">

- 향후 서비스, 레포지토리 등의 코드 작성 예정

## 참고 자료

- [https://wave1994.tistory.com/130](https://wave1994.tistory.com/130)
- [https://sabarada.tistory.com/182](https://sabarada.tistory.com/182)
- [https://velog.io/@hyunsdb/KotlinSpring으로-간단한-CRUD-해보기](https://velog.io/@hyunsdb/KotlinSpring%EC%9C%BC%EB%A1%9C-%EA%B0%84%EB%8B%A8%ED%95%9C-CRUD-%ED%95%B4%EB%B3%B4%EA%B8%B0)
- [https://afsdzvcx123.tistory.com/entry/Docker-PostgreSQL-Docker-Compose-파일-작성](https://afsdzvcx123.tistory.com/entry/Docker-PostgreSQL-Docker-Compose-%ED%8C%8C%EC%9D%BC-%EC%9E%91%EC%84%B1)
- [https://aeliketodo.tistory.com/m/39](https://aeliketodo.tistory.com/m/39)
