- 일반적으로 QueryDsl에서 데이터를 받아오면 `List<Tuple>` 형태로 받아오게 된다.
    - selectFrom으로 entity 전체를 받아오지 않고 필드를 지정하면 이러한 형식으로 받아온다.
        
        ```kotlin
        package com.kotlin.study.dongambackend.domain.post.repository
        
        import com.kotlin.study.dongambackend.domain.post.entity.Post
        import com.kotlin.study.dongambackend.domain.post.entity.QPost
        import com.querydsl.jpa.impl.JPAQueryFactory
        import org.springframework.stereotype.Repository
        
        import org.springframework.data.domain.Pageable
        
        @Repository
        class PostQueryDslRepository(val queryDslFactory: JPAQueryFactory) {
            val qPost = QPost.post;
        
            /**
             * isDeleted = true인 게시물 제외한 전체 게시물 리스트 페이지네이션
             */
            fun findAllPost(pageable: Pageable): List<Post> {
                return queryDslFactory
                    .selectFrom(qPost)
                    .offset(pageable.offset)
                    .limit(pageable.pageSize.toLong())
                    .where(qPost.isDeleted.eq(false))
                    .fetch()
            }
        }
        ```
        
        ```java
        package com.trip.api.chat.repository;
        
        import com.querydsl.core.Tuple;
        import com.querydsl.jpa.impl.JPAQueryFactory;
        import com.trip.api.chat.entity.ChatRoom;
        import com.trip.api.chat.entity.QChatRoom;
        import com.trip.api.chat.entity.QChatRoomMember;
        import lombok.RequiredArgsConstructor;
        import org.springframework.stereotype.Repository;
        
        import java.util.List;
        
        @RequiredArgsConstructor
        @Repository
        public class ChatRoomQueryDslRepository<T> {
        
            private final JPAQueryFactory jpaQueryFactory;
        
            private QChatRoom qChatRoom = QChatRoom.chatRoom;
            private QChatRoomMember qChatRoomMember = QChatRoomMember.chatRoomMember;
        
            // TODO: 페이지네이션 적용 여부 확인
            public List<Tuple> findAllMyChatRoom(Long memberId) {
                return jpaQueryFactory
                        .select(qChatRoom.id, qChatRoom.name)
                        .from(qChatRoom)
                        .join(qChatRoomMember).on(qChatRoom.id.eq(qChatRoomMember.id.chatRoomId))
                        .where(qChatRoomMember.id.memberId.eq(memberId))
                        .fetch();
            }
        }
        ```
        
- 하지만 **튜플 타입을 그대로 가져와 컨트롤러에서 return하면 Serialize가 되지 않는다는 오류가 발생**한다.
- 이 때, `Projections`를 사용하면 된다.
    
    ```java
    package com.trip.api.chat.repository;
    
    import com.querydsl.core.types.Projections;
    import com.querydsl.jpa.impl.JPAQueryFactory;
    
    import com.trip.api.chat.dto.response.GetMyChatRoomResponse;
    import com.trip.api.chat.entity.QChatRoom;
    import com.trip.api.chat.entity.QChatRoomMember;
    
    import lombok.RequiredArgsConstructor;
    import org.springframework.stereotype.Repository;
    
    import java.util.List;
    
    @RequiredArgsConstructor
    @Repository
    public class ChatRoomQueryDslRepository<T> {
    
        private final JPAQueryFactory jpaQueryFactory;
        private QChatRoom qChatRoom = QChatRoom.chatRoom;
        private QChatRoomMember qChatRoomMember = QChatRoomMember.chatRoomMember;
    
        // TODO: 페이지네이션 적용 여부 확인
        public List<GetMyChatRoomResponse> findAllMyChatRoom(Long memberId) {
            return jpaQueryFactory
                    .select(
                        Projections.constructor(
                            GetMyChatRoomResponse.class,
                            qChatRoom.id.as("chatRoomId"),
                            qChatRoom.name.as("title")
                        )
                    )
                    .from(qChatRoom)
                    .join(qChatRoomMember).on(qChatRoom.id.eq(qChatRoomMember.id.chatRoomId))
                    .where(
                        qChatRoomMember.id.memberId.eq(memberId),
                        qChatRoomMember.isExited.eq(false),
                        qChatRoom.isDeleted.eq(false)
                    )
                    .fetch();
        }
    }
    ```
    
    - `Projections.constructor()`로 Projection할 클래스와 column을 지정해주면 된다.

## 참고 자료

- https://blog.leocat.kr/notes/2019/11/14/querydsl-result-handling-projection