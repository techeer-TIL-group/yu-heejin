## 클라이언트와 서버의 비동기 통신

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsxcOu%2Fbtq4eKsIpSZ%2FkntlVrm6YznC7PKyBHemH1%2Fimg.png)

- 클라이언트에서 서버로 통신하는 메시지를 `요청(request) 메시지`라고 하며, 서버에서 클라이언트로 통신하는 메세지를 `응답(response) 메시지`라고 한다.
- 웹에서 화면 전환(새로고침) 없이 이루어지는 동작들은 대부분 **비동기 통신**으로 이루어진다.
- 비동기 통신을 하기 위해서는 **클라이언트에서 서버로 요청 메세지를 보낼 때 본문에 데이터를 담아서 보내**야 하고, **서버에서 클라이언트로 응답을 보낼 때에도 본문에 데이터를 담에서 보내**야한다.
    - 이 본문이 바로 **body** 이다.
    - 즉, 요청 본문 requestBody, 응답 본문 responseBody를 담아서 보내야 한다.
    - 이때, **본문에 담기는 데이터 형식**은 여러가지 형태가 있지만 가장 대표적으로 사용되는 것은 **JSON**이다.
    - 즉, 비동기식 클라이언트-서버 통신을 위해 JSON 형식의 데이터를 주고 받는 것이다.
- 스프링에서도 **클라이언트에서 전송한 xml 데이터나 json 등등 데이터를 컨트롤러에서 DOM 객체나 자바 객체로 변환해서 송수신**할 수 있다.
- `@RequestBody` 어노테이션과 `@ResponseBody` 어노테이션이 **각각 HTTP 요청 바디를 자바 객체로 변환하고 자바 객체를 다시 HTTP 응답 바디로 변환**해준다.

### 요청 본문(Request Body)에 담긴 값을 자바 객체로 변환?

- `@RequestBody`를 통해서 자바 객체로 conversion(변환)을 하는데, 이때 `HttpMessageConverter`를 사용한다.
- `@ResponseBody`가 붙은 파라미터에는 HTTP 요청의 본문 body 부분이 그대로 전달된다.
- `RequestMappingHandlerAdapter`에는 `HttpMessageConverter` 타입의 메세지 변환기가 여러개 등록되어 있다.

## @RequestBody

```kotlin
@PostMapping
fun createPost(@RequestBody postCreateRequest: PostCreateRequest): PostResponse {
    // @RequestBody 어노테이션 안 붙이면 오류가 발생한다.
    return postService.createPost(postCreateRequest)
}
```

- 해당 어노테이션이 붙은 파라미터에는 http 요청의 본문(body)이 그대로 전달된다.
- **xml, json 기반의 메시지를 사용하는 요청**의 경우 이 방법이 매우 유용하다.
- HTTP 요청의 **body 내용을 통째로 자바 객체로 변환해서 매핑된 메소드 파라미터로 전달해준다.**

## @ResponseBody

- **자바 객체를 HTTP 요청의 body 내용으로 매핑**하여 클라이언트로 전송한다.
- `@ResponseBody`가 붙은 파라미터가 있으면 **HTTP 요청의 미디어 타입과 파라미터의 타입을 먼저 확인한다.**
- 메세지 변환기 중에서 해당 미디어 타입과 파라미터 타입을 처리할 수 있다면, **HTTP 요청의 본문 부분을 통째로 변환해 지정된 메소드 파라미터로 전달해준다.**
- 즉, **@ResponseBody 어노테이션을 사용하면 http 요청 body를 자바 객체 → JSON 로 전달받을 수 있다.**

## @RestController

- `@Controller`와 달리 `@RestController`는 **리턴 값에 자동으로 @ResponseBody가 붙기 때문에** 별도의 어노테이션을 명시해주지 않아도 **HTTP 응답 데이터(body)에 자바 객체가 매핑되어 전달된다. (class → JSON)**
- `@Controller` 인 경우 **body를 자바 객체로 받기 위해 @ResponseBody 어노테이션을 반드시 명시해주어야 한다.**

## 참고 자료

- [https://cheershennah.tistory.com/179](https://cheershennah.tistory.com/179)