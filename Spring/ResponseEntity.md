## @ResponseBody

- HTTP 규격에 맞는 응답을 만들어주기 위한 어노테이션
- HTTP 요청을 객체로 변환하거나, 객체를 응답으로 변환하는 `HttpMessageConverter`를 사용한다.
    - `HttpMessageConverter`는 해당 어노테이션이 붙은 대상을 response body에 직렬화를 하는 방식으로 작동된다.
    - 따라서 컨트롤러에서 반환할 객체나 메소드에 `@ResponseBody`를 붙이는 것 만으로 HTTP 규격에 맞는 값을 반환할 수 있다.

```java
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

@ResponseBody
@ResponseStatus(HttpStatus.OK)
public MoveResponseDto move(@PathVariable String name, @RequestBody MoveDto moveDto) {
  String command = makeMoveCmd(moveDto.getSource(), moveDto.getTarget());
    springChessService.move(name, command, new Commands(command));
    
  MoveResponseDto moveResponseDto = new MoveResponseDto(springChessService
      .continuedGameInfo(name), name);
      
  return moveResponseDto;
}
```

- 어노테이션을 추가하는 것만으로도 간단하게 처리할 수 있다.
- 또한, 해당 메서드를 가진 컨트롤러에 `@RestController` 어노테이션이 붙으면, `@ResponseBody`를 생략하여 더 간결하게 코드를 작성할 수 있다.
- 그러나 HTTP 규격 구성 요소 중 하나인 Header에 대해서 유연하게 설정할 수 없고, status 역시 메서드 밖에서 어노테이션을 별도로 지정해주어야한다.
    - 이는 `@ResponseBody`만 사용할 시 별도의 뷰를 제공하지 않고 데이터만 전송하기 때문이다.

## ResponseEntity

- HTTP 응답을 빠르게 만들어주기 위한 객체
- `@ResponseBody`와 달리 어노테이션이 아닌 객체로 사용된다.
    - 즉, **응답으로 변환될 정보를 모두 담은 요소들을 객체로 만들어 반환**한다.
- 객체의 구성요소에서 `HttpMessageConverter`는 응답이 되는 본문을 처리해준 후, RESTTemplate에 나머지 구성 요소인 status를 넘겨준다.
- 적절한 상태와 데이터를 클라이언트에 전달할 수 있더,

### Response Entity 선언 구조

```java
//ResponseEntity 선언 구조
public class ResponseEntity extends HttpEntity {

  private final Object status;
}
```

- `ResponseEntity`의 구조를 보면 status만 필드 값으로 가지고 있다.
    - 이는 `ResponseEntity`에서 직접적으로 status code를 지정할 수 있음을 의미한다.
    - 나머지 부분은 `HttpEntity`에 구현이 되어있는데, 이는 `**RequestEntity`와 여러 설정들을 공유하기 때문이다.**

### HttpEntity 선언 구조

```java
//HttpEntity 선언 구조
public class HttpEntity<T> {
    public static final HttpEntity<?> EMPTY = new HttpEntity<>();
  
    private final HttpHeaders headers;
  
    @Nullable
    private final T body;
}
```

- `ResponseEntity`는 `HttpEntity`를 상속하여 구현된다.
- `HttpEntity`에서는 제네릭타입으로 body가 될 필드 값을 가질 수 있다.
    - 제네릭 타입으로 바깥에서 wrapping될 타입을 지정할 수 있다.
    - wrapping된 객체들은 자동으로 http 규격에서 body에 들어갈 수 있도록 변환된다.
- 필드 타입으로 `HttpHeaders`를 가지고 있는데, 이는 `ResponseBody`와 다르게 **객체 안에서 Header를 설정할 수 있음을 의미한다.**

### ResponseEntity 사용법

```java
public ResponseEntity<MoveResponseDto> move(@PathVariable String name,
    @RequestBody MoveDto moveDto) {
    HttpHeaders headers = new HttpHeaders();
    headers.set("Game", "Chess");
    
    String command = makeMoveCmd(moveDto.getSource(), moveDto.getTarget());
    springChessService.move(name, command, new Commands(command));
    
    MoveResponseDto moveResponseDto = new MoveResponseDto(springChessService
        .continuedGameInfo(name), name);

    return new ResponseEntity<MoveResponseDto>(moveResponseDto, headers, HttpStatus.valueOf(200)); // ResponseEntity를 활용한 응답 생성
}
```

- 타입은 `ResponseEntity<type>`으로 지정한다.

### 여러 타입 리턴

```java
public ResponseEntity<?> getUsers() {
    List<User> users = userService.getUsers();
    return ResponseEntity.ok(users);
}
```

- 여러 타입을 반환하는 경우 와일드 카드(?)를 사용할 수 있다.

## Constructor보다는 Builder

```java
return new ResponseEntity<MoveResponseDto>(moveResponseDto, headers, HttpStatus.valueOf(200));

return ResponseEntity.ok()
    .headers(headers)
    .body(moveResponseDto);
```

- responseEntity를 사용할 땐 constructor보다는 builder 패턴을 사용하는 것을 권장한다.

## ResponseEntity.created()

- `created()` 메소드를 사용할 경우 **새로 생성된 자원의 주소를 함께 반환**해야한다.
- 이는 새롭게 생성된 리소스에 대한 접근url을 location 헤더 값으로 포함시킴으로써 클라이언트 쪽에서 이 정보를 활용해 해당 리소스에 접근할 수 있도록 한다.

## 참고 자료

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/201
- https://tecoble.techcourse.co.kr/post/2021-05-10-response-entity/
- https://devlog-wjdrbs96.tistory.com/182
- https://stir.tistory.com/343
- https://medium.com/@daryl-goh/spring-boot-requestentity-vs-responseentity-requestbody-vs-responsebody-dc808fb0d86c