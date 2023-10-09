## @Enumerated 어노테이션

- 엔티티 매핑에서 Enum 타입을 사용할 때 사용하는 어노테이션

## 타입 종류

### EnumType.ORDINAL

- enum 순서(숫자)값을 DB에 저장

### EnumType.STRING

- enum 이름(문자열) 값을 DB에 저장

### 예시

```kotlin
package com.kotlin.study.dongambackend.common.type

enum class BoardCategoryType {
    자유게시판,
    장터게시판;
}
```

```kotlin
package com.kotlin.study.dongambackend.domain.post.entity

import com.kotlin.study.dongambackend.common.entity.BaseTimeEntity
import com.kotlin.study.dongambackend.common.type.BoardCategoryType
import com.kotlin.study.dongambackend.domain.post.dto.request.PostUpdateRequest
import lombok.AllArgsConstructor
import lombok.Builder
import lombok.NoArgsConstructor
import org.hibernate.annotations.ColumnDefault
import org.hibernate.annotations.DynamicInsert
import org.hibernate.annotations.SQLDelete
import javax.validation.constraints.NotNull
import javax.persistence.*

@NoArgsConstructor
@DynamicInsert
@SQLDelete(sql = "UPDATE post SET is_deleted = true WHERE id = ?")
@Entity
class Post(

    // TODO: userId 참조 필요
    @Column(name = "user_id", nullable = false)
    val userId: Long,

    @NotNull
    var title: String,

    var content: String,

    ***@NotNull
    @Enumerated(EnumType.STRING)
    val category: BoardCategoryType,***

    @ColumnDefault("0")
    val likes: Int? = 0,

    @ColumnDefault("0")
    val views: Int? = 0,

    @Column(name = "is_deleted")
    @ColumnDefault("false")
    val isDeleted: Boolean? = false,

    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    val id: Long? = null
) : BaseTimeEntity() {

    fun updatePost(postUpdateRequest: PostUpdateRequest) {
        title = postUpdateRequest.title
        content = postUpdateRequest.content
    }
}
```

- `EnumType.ORDINAL` → 자유게시판 = 1, 장터게시판 = 2로 저장된다.
- `EnumType.STRING` → 자유게시판, 장터게시판 로 저장된다.
- 추후 확장성을 생각하면 `EnumType.ORDINAL`은 서수 변경의 위험이 있으므로 추천하지 않는다.

## 참고 자료

- [https://dandev.tistory.com/entry/JPA-Enumerated란-무엇이며-어떻게-사용할까-🤔](https://dandev.tistory.com/entry/JPA-Enumerated%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%A0%EA%B9%8C-%F0%9F%A4%94)
- https://lovethefeel.tistory.com/72
- https://lng1982.tistory.com/280