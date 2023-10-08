> 결론적으로 코틀린에서 Optional은 사용할 필요가 없다.
> 

## 코틀린에서의 Nullable vs Optional

- `Optional`은 자바에서 Null 관련 처리를 해주기 위해 등장했다.
    - `Optional`은 해당 객체가 null일수도 있고 아닐 수도 있다는 것을 의미한다.
- 하지만 코틀린은 `Nullable` 타입을 따로 처리하기 때문에 `Optional`을 사용할 필요가 없다.
    - 코틀린은 자바와 달리 아래와 같이 Nullable 타입을 따로 지정할 수 있다.
        
        ```kotlin
        // Non-Null (null 할당 불가능)
        val title: String
        var contents: String
        
        // Nullable (null 할당 가능)
        val title: String?
        var contents: String?
        ```
        
    - 코틀린이 자바와 호환되기 때문에 사용할 수는 있지만, 코틀린만의 장점을 느낄 수 없다.

## Kotlin nullable 연산자

- `?.` : 앞이 null이 아니면 뒤의 함수 또는 프로퍼티에 접근한다. (safe-call)
- `?:` : 앞이 null이면 뒤의 값으로 대체한다. (elvis)
- `!!` : non-null 연산자로 Java처럼 아무것도 처리하지 않는다. (사용 지양)

## 코틀린을 위한 JPA 확장 함수

- `findById` 메서드는 JPA에서 기본적으로 `Optional`을 반환한다.
- `findByIdOrNull`을 사용하면 `Optional`을 사용하지 않고 Null을 반환한다.
    
    ```kotlin
    @Transactional(readOnly = true)
    fun getPostById(postId: Long): Post? {
        return postRepository.findByIdOrNull(postId)
          ?: throw NoSuchElementException()
    }
    ```
    

## JPA custom method를 추가하는 경우

```kotlin
package com.kotlin.study.dongambackend.domain.post.repository

import com.kotlin.study.dongambackend.domain.post.entity.PostLike
import org.springframework.data.jpa.repository.JpaRepository
import org.springframework.data.jpa.repository.Query
import java.util.*

interface PostLikeRepository : JpaRepository<PostLike, Long> {

    @Query(
        nativeQuery = true,
        value = "SELECT * FROM post_like WHERE user_id = :userId AND post_id = :postId"
    )
    fun findById(userId: Long, postId: Long): PostLike?
}
```

- 반환 타입을 `Optional`이 아닌 Nullable 타입으로 설정하면 된다.

## 기존 프로젝트 리팩토링

~~뭣모르고 막 써서 부끄럽다..^-^;~~

### 기존 코드

```kotlin
package com.kotlin.study.dongambackend.domain.post.service

import com.kotlin.study.dongambackend.domain.post.dto.entitykey.PostLikeKey
import com.kotlin.study.dongambackend.domain.post.dto.request.PostCreateRequest
import com.kotlin.study.dongambackend.domain.post.dto.request.PostUpdateRequest
import com.kotlin.study.dongambackend.domain.post.entity.Post
import com.kotlin.study.dongambackend.domain.post.entity.PostLike
import com.kotlin.study.dongambackend.domain.post.mapper.PostMapper
import com.kotlin.study.dongambackend.domain.post.repository.PostLikeRepository
import com.kotlin.study.dongambackend.domain.post.repository.PostQueryDslRepository
import com.kotlin.study.dongambackend.domain.post.repository.PostRepository

import org.springframework.data.domain.Pageable
import org.springframework.stereotype.Service
import org.springframework.transaction.annotation.Transactional;

@Service
class PostService(
    val postRepository: PostRepository,
    val postQueryDslRepository: PostQueryDslRepository,
    val postLikeRepository: PostLikeRepository,
    val postMapper: PostMapper
) {

    @Transactional(readOnly = true)
    fun getPostById(postId: Long): Post {
        return postRepository.findById(postId).orElseThrow()
    }

    fun updatePost(postUpdateRequest: PostUpdateRequest, postId: Long) {
        val post = postRepository.findById(postId).orElseThrow()
        post.updatePost(postUpdateRequest)
        postRepository.save(post)
    }

    fun clickPostLike(postId: Long, userId: Long) {
        if (isExistedPost(postId)) {
            val postLike = postLikeRepository.findById(userId, postId).orElseGet{ PostLike(PostLikeKey(userId, postId)) }
            postLike.updatePostLike()
            postLikeRepository.save(postLike)
        }
    }

    fun isExistedPost(postId: Long): Boolean {
        return postRepository.findById(postId).isPresent
    }
}
```

### 리팩토링 코드

```kotlin
package com.kotlin.study.dongambackend.domain.post.service

import com.kotlin.study.dongambackend.domain.post.dto.entitykey.PostLikeKey
import com.kotlin.study.dongambackend.domain.post.dto.request.PostCreateRequest
import com.kotlin.study.dongambackend.domain.post.dto.request.PostUpdateRequest
import com.kotlin.study.dongambackend.domain.post.entity.Post
import com.kotlin.study.dongambackend.domain.post.entity.PostLike
import com.kotlin.study.dongambackend.domain.post.mapper.PostMapper
import com.kotlin.study.dongambackend.domain.post.repository.PostLikeRepository
import com.kotlin.study.dongambackend.domain.post.repository.PostQueryDslRepository
import com.kotlin.study.dongambackend.domain.post.repository.PostRepository

import org.springframework.data.domain.Pageable
import org.springframework.data.repository.findByIdOrNull
import org.springframework.stereotype.Service
import org.springframework.transaction.annotation.Transactional;

@Service
class PostService(
    val postRepository: PostRepository,
    val postQueryDslRepository: PostQueryDslRepository,
    val postLikeRepository: PostLikeRepository,
    val postMapper: PostMapper
) {

    @Transactional(readOnly = true)
    fun getPostById(postId: Long): Post? {
        return postRepository.findByIdOrNull(postId)
            ?: throw NoSuchElementException()
    }

    fun updatePost(postUpdateRequest: PostUpdateRequest, postId: Long) {
        val post = postRepository.findByIdOrNull(postId)
            ?: throw NoSuchElementException()
        post.updatePost(postUpdateRequest)
        postRepository.save(post)
    }

    fun clickPostLike(postId: Long, userId: Long) {
        if (isExistedPost(postId)) {
            val postLike = postLikeRepository.findById(userId, postId)
                ?: PostLike(PostLikeKey(userId, postId))
            postLike.updatePostLike()
            postLikeRepository.save(postLike)
        }
    }

    fun isExistedPost(postId: Long): Boolean {
        return postRepository.findById(postId).isPresent
    }
}
```

## 참고 자료

- https://ttl-blog.tistory.com/840
- https://hwanchang.tistory.com/8
- [https://velog.io/@cmplxn/Optional-과-nullable](https://velog.io/@cmplxn/Optional-%EA%B3%BC-nullable)