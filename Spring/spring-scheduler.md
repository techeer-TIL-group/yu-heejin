## @Scheduled

- Spring Scheduler는 `@Scheduled` 어노테이션을 명시해 사용할 수 있다.
- 보통 실행하고자 하는 메소드명 위에 명시한다.
- 스케줄러가 정상 작동하기 위해서는 스프링 애플리케이션에서 Scheduling을 활성화 시켜야한다.

### Scheduling 활성화하기

- Scheduling은 `@EnableScheduling` 어노테이션을 클래스 위에 명시해 활성화 시킨다.
- `@Scheduled`를 사용하고자 하는 메소드가 위치한 클래스 위에 명시할 수도 있고, `@SpringBootApplication` 이 위치한 클래스 위에 명시해도 된다.
    
    ```java
    @EnableScheduling
    public class MySchedulerClass {
      @Scheduled(cron="0/60 * * * * ?")
    }
    
    // or
    
    @SpringBootApplication
    @EnableScheduling
    public class MySchedulerApplication { 
      public static void main(String[] args) {
        SpringApplication.run(MySchedulerApplication.class, args);
      }
    }
    ```
    

## Spring Scheduler 동작

- 스프링 스케줄러는 동기적으로 스케줄러를 실행한다.
- `@Scheduled`의 옵션으로 `fixedRate`, `fixedDelay`, `cron`을 사용해서 각각 다른 방식으로 동작시킬 수 있다.

### fixedRate

- 작업의 시작부터 시간을 카운트한다.
    
    ```java
    @Scheduled(fixedRate=1000) // 단위: ms
    public void fixedRateScheduler() {
      System.out.println("나는 이 작업이 끝날때 까지 기다리지 않고 1000ms 마다 실핼될거야");
    }
    ```
    
    - 즉, **이 작업이 시작한 직후**부터 시간을 계산한다.

### fixedDelay

- 작업이 끝난 시점부터 시간을 카운트한다.
    
    ```java
    @Schedueld(fixedDelay=1000) // 단위: ms 
    public void fixedDelayScheduler() {
      System.out.println("나는 이 작업이 끝나고 나서 다시 1000ms 후에 실행될거야");
    }
    ```
    
    - 즉, **이 작업이 끝난 직후**부터 시간을 계산한다.

### cron

- 개발자가 초, 분, 시, 일, 월, 주, (년) 을 지정해 스케줄러를 동작시킨다.
    - 이때 (년)은 생략 가능하다.
    
    ```java
    @Scheduled(cron="0/60 * * * * ?") 
    public void cronScheduler() {
      System.out.println("나는 시스템 시간을 기준으로 1분 마다 주기적으로 실행될거야");
    }
    ```
    

## @Async가 필요한 이유

- Spring Scheduler는 **동기적으로 실행**된다.
- 따라서 스케줄러를 비동기적으로 실행할 필요가 있다.
- `@Async`를 활용하면 **다음 스케줄러가 이전 스케줄러의 작업이 끝날 때까지 기다리지 않고 자신의 작업을 처리할 수 있다.**
- `@Async`를 사용하기 전에 `@EnableAsync`로 활성화 시켜야 한다.
    
    ```java
    @SpringBootApplication
    @EnableScheduling
    @EnableAsync            // Async 활성화 
    public class MySchedulerApplication {
      public void main(String[] args) {
        SpringApplication.run(MySchedulerApplication.class, args); 
      }
    }
    
    @Async
    @Scheduled(cron="0/60 * * * * ?")
    public void cronScheduler() {
      System.out.println("나는 시스템 시간을 기준으로 1분 마다 주기적으로 실행될거야");
    }
    ```
    

## Cron 표현식

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcuXvMc%2FbtrbIHaRsDC%2Fi6BoUqC6jR1DakrbCI3Y70%2Fimg.jpg)

| 초 | 분 | 시 | 일 | 월 | 요일 | 년도 |
| --- | --- | --- | --- | --- | --- | --- |
| 0 ~ 59 | 0 ~ 59 | 0 ~ 23 | 1 ~ 31 | 1 ~ 12 | 0 ~ 6 | *생략가능* |

### Cron 표현식 특수문자

- : 모든 값(매시, 매일, 매주처럼 사용한다.)
- ? : 특정 값이 아닌 어떤 값이든 상관 없음
- : 범위를 지정할 때
- , : 여러 값을 지정할 때
- / : 증분값, 즉 초기값과 증가치를 설정할 때
- L : 지정할 수 있는 범위의 마지막 값 표시
- W : 가장 가까운 평일(weekday)을 설정할 때
- # : N번 째 특정 요일을 설정할 때

### 사용 예시

- 매 10분마다 : `0 0/10 * * * *`
- 매 3시간마다 : `0 0 0/3 * * *`
- 2018년도 매일 14시 30분마다 : `0 30 14 * * * 2018`
- 매일 10시 ~ 19시 사이에 10분 간격으로 : `0 0/10 10-19 * * *`
- 매일 10시와 19시에만 10분 간격으로 : `0 0/10 10,19 * * *`
- 매달 25일 01시 30분에 : `0 30 1 25 * *`
- 매주 월, 금요일 10시와 19시 사이 10분마다 : `0 10 10-19 ? * MON,FRI`
- 매달 마지막날 15시 30분 : `0 30 15 L * *`
- 2017~2018년 매월의 마지막 토요일 오후 1시 20분 : `0 20 13 ? * 6L 2017-2018`

## 참고 자료

- [https://velog.io/@kimh4nkyul/Scheduled를-활용한-Spring-Scheduler](https://velog.io/@kimh4nkyul/Scheduled%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-Spring-Scheduler)
- [https://wooncloud.tistory.com/75](https://wooncloud.tistory.com/75)