## @field:[Validate annotation]

- Kotlin으로 Valid 관련 제약 사항을 명시할 때 **Validation 관련 annotation에 `@field:`를 prefix로 붙여야한다.**
- Java에서 사용하던 방식대로 `@NotNull` 이런 식으로 사용하면 **코틀린에서 Java로 convert할 때 constructor에 Valid 어노테이션이 붙는다.**

### validate 동작 과정

```kotlin
package com.kotlin.study.dongambackend.domain.post.dto.request

import com.kotlin.study.dongambackend.common.type.BoardCategoryType
import javax.validation.constraints.NotBlank
import javax.validation.constraints.NotEmpty

data class PostCreateRequest (
    @NotBlank(message = "제목 값은 필수입니다.")
    val title: String?,

    @NotEmpty(message = "내용을 입력해주세요.")
    val content: String?,

    val category: BoardCategoryType,
)
```

- 위와 같은 코드의 경우, 생성자의 위치에서 어노테이션을 달아주었기 때문에 **타겟을 설정하지 않는 경우 생성자쪽에만 어노테이션이 붙기 때문에 필드 프로퍼티 쪽에는 어노테이션이 붙지 않는다.**
- 따라서 다음과 같이 어노테이션을 지정하면 된다.
    
    ```kotlin
    package com.kotlin.study.dongambackend.domain.post.dto.request
    
    import com.kotlin.study.dongambackend.common.type.BoardCategoryType
    import javax.validation.constraints.NotBlank
    import javax.validation.constraints.NotEmpty
    
    data class PostCreateRequest (
        @field:NotBlank(message = "제목 값은 필수입니다.")
        val title: String?,
    
        @field:NotEmpty(message = "내용을 입력해주세요.")
        val content: String?,
    
        val category: BoardCategoryType,
    )
    ```
    

## 클래스 멤버 필드는 nullable로 지정해야한다.

- 코틀린으로 Validation을 적용하려면 **nullable type으로 선언해야한다.**
- `@field:NotNull`을 적용한 필드에 확정형 타입을 선언한 경우 요청 시 해당 필드를 입력하지 않았을 때 원하는 방식의 에러 헨들링을 기대하기 어렵다.
    - **Nullable에 대한 Type Mismatch를 반환하기 때문에 Valid에 대한 원하는 형태의 에러를 반환하지 않는다.**
- 또한, 필드가 Long, Int와 같은 Primitive type인 경우 `@RequestBody` 형태로 받아올 때 해당 필드에 대해 값을 입력하지 않아도 default value로 설정된다.
    - **Java에서는 기본적으로 Primitive 타입에 대한 default 값이 존재하기 때문**이다.
    - 코틀린에서는 Long, Int와 같은 타입은 primitive type과 wrapper type 모두 포함하는데, **nullable하지 않은 확장형 필드로 선언하면 primitive type이 되어 해당 필드에 대해 데이터를 입력하지 않아도 default value로 컴파일러가 지정**해준다.
- 따라서 **Validation을 적용할 땐 nullable type으로 선언하는 것이 좋다.**

```kotlin
package com.kotlin.study.dongambackend.domain.post.dto.request

import com.kotlin.study.dongambackend.common.type.BoardCategoryType
import javax.validation.constraints.NotBlank
import javax.validation.constraints.NotEmpty

data class PostCreateRequest (
    @field:NotBlank(message = "제목 값은 필수입니다.")
    val title: String?,

    @field:NotEmpty(message = "내용을 입력해주세요.")
    val content: String?,

    val category: BoardCategoryType,
)
```

```kotlin

Resolved [org.springframework.http.converter.HttpMessageNotReadableException: 
JSON parse error: Instantiation of 
[simple type, class com.kotlin.study.dongambackend.domain.post.dto.request.PostCreateRequest]
 value failed for JSON property title due to missing (therefore NULL) value 
for creator parameter title which is a non-nullable type; nested exception 
is com.fasterxml.jackson.module.kotlin.MissingKotlinParameterException: 
Instantiation of [simple type, class com.kotlin.study.dongambackend
.domain.post.dto.request.PostCreateRequest] 
**value failed for JSON property title due to missing (therefore NULL)** 
**value for creator parameter title which is a non-nullable type<LF> at 
[Source: (PushbackInputStream); line: 4, column: 1]**
(through reference chain: com.kotlin.study.dongambackend.domain.post.dto.
request.PostCreateRequest["title"])]
```

```kotlin

Resolved [org.springframework.web.bind.MethodArgumentNotValidException: 
Validation failed for argument [0] in public org.springframework.
http.ResponseEntity<kotlin.Unit> 
com.kotlin.study.dongambackend.domain.post.controller.PostController.createPost
(com.kotlin.study.dongambackend.domain.post.dto.request.PostCreateRequest): 
[Field error in object 'postCreateRequest' on field 'title': rejected value [null]; 
**codes [NotBlank.postCreateRequest.title,NotBlank.title,NotBlank.java.lang.String,NotBlank]; 
arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes 
[postCreateRequest.title,title]; arguments []; default message [title]];** 
d**efault message [제목 값은 필수입니다.]] ]**
```

## 참고 자료

- https://beaniejoy.tistory.com/72
- https://mystory-blog.vercel.app/blog/spring/spring-validate
- https://unluckyjung.github.io/kotlin/spring/2022/06/06/kotlin-validation-annotation/