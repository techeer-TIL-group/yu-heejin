> 결과적으로 `Resource`를 가져오는 경우 `Path Variable`, 정렬이나 필터링의 경우 `Query Parameter`를 사용한다.
> 

## Query Parameter

```
/users?id=123  # 아이디가 123인 사용자를 가져온다.
```

- 웹 개발자라면 위와 같은 GET 메소드를 사용해 데이터를 전송하는 방법을 많이 접했을 것이다.
- 만약 각각의 사용자를 위한 페이지를 만들려면, **페이지마다 식별된 파라미터 경로**가 필요한데, 위와 같이 `Query Parameter`를 사용할 수 있다.

## Path Variable

```
/users/123  # 아이디가 123인 사용자를 가져온다.
```

- 데이터를 넘겨주는 방법으로 `Path Variable`을 사용할 수도 있다.
- 즉, **경로를 변수로 사용**하는 것이다.

## Query Parameter vs Path Variable

- 어떤 **리소스를 식별**하고 싶으면 `Path Variable`, **정렬이나 필터링** 시 `Query Parameter`를 사용한다.
    
    ```
    /users  # 사용자 목록을 가져온다.
    /users?occupation=programer  # 프로그래머인 사용자 목록을 가져온다.
    /users/123  # 아이디가 123인 사용자를 가져온다.
    ```
    
- 또한, 기본적인 CRUD 기능을 위해 또 다른 URL이나 `Query Parameter`를 정의할 필요는 없지만, **원하는 기능에 맞게 HTTP 메소드를 바꿔야한다.**
    
    ```
    /users [GET] # 사용자 목록을 가져온다.
    /users [POST] # 새로운 사용자를 생성한다.
    /users/123 [PUT] # 사용자를 갱신한다.
    /users/123 [DELETE] # 사용자를 삭제한다.
    ```
    

## 참고 자료

- [https://ryan-han.com/post/translated/pathvariable_queryparam/](https://ryan-han.com/post/translated/pathvariable_queryparam/)