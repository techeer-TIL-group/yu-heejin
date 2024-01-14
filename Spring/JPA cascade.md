## 참고 자료

- https://tecoble.techcourse.co.kr/post/2023-08-14-JPA-Cascade/
- [https://velog.io/@max9106/JPA엔티티-상태-Cascade](https://velog.io/@max9106/JPA%EC%97%94%ED%8B%B0%ED%8B%B0-%EC%83%81%ED%83%9C-Cascade)
- https://easybrother0103.tistory.com/70

## JPA Cascade

![https://tecoble.techcourse.co.kr/post/2023-08-14-JPA-Cascade/](https://tecoble.techcourse.co.kr/static/0fa31508247e8d3f056612a63cc302d1/f6c2e/2023-08-14-JPA_01.png)

https://tecoble.techcourse.co.kr/post/2023-08-14-JPA-Cascade/

- JPA Cascade를 활용하면 어떤 엔티티와 다른 엔티티가 밀접한 연관성이 있을 때에 대한 관리가 매우 수월해진다.
- 즉, A라는 엔티티에 어떤 작업을 수행했을 때, **그 작업이 연관된 B라는 엔티티도 이루어져야 한다면 JPA Cascade를 유용하게 사용할 수 있다.**
- Entity의 상태 변화를 전파 시키는 옵션으로, 만약 Entity에 상태 변화가 있으면 연관되어 있는 엔티티에도 상태 변화를 전이시킨다.

## Entity 상태

> cascade 옵션은 아래의 상태 변화를 전파시키는 과정이다.
> 

### Transient

- JPA가 알지 못하는 상태를 의미한다.
- 객체를 생성하거나 변경하여도 JPA가 그 객체를 인지하지 못하고 있는 상태

### Persistent

- JPA가 관리중인 상태를 의미한다.
- Persistent 상태가 되더라도 바로 insert가 발생하여 데이터베이스에 저장하는 것이 아닌, **Persistent에서 관리하고 있던 객체가 데이터베이스에 넣는 시점에 데이터를 저장한다.**
- 1차 캐싱, Dirty checking, Write Behind 등 기능 제공

### Detached

- JPA가 더이상 객체를 관리하지 않는 상태를 의미한다.

### Removed

- JPA가 현재 시점에는 관리하고 있지만 곧 삭제하기로 한 상태를 의미한다.

## JPA Cascade Types

### PERSIST

- 엔티티를 영속화할 때 연관된 엔티티도 함께 영속화하는 옵션
- 부모와 자식 엔티티를 한 번에 영속화할 수 있다.

### MERGE

- 엔티티 상태를 병합할 때 연관된 엔티티도 함께 병합하는 옵션

### REMOVE

- 엔티티를 제거할 때 연관된 엔티티들도 함께 제거한다.
- 부모 엔티티를 삭제할 경우 연관된 자식 객체들도 삭제된다.

### REFRESH

- 엔티티를 새로고침할 때 연관된 엔티티들도 함께 새로고침한다.
- ‘새로고침’의 의미는 데이터베이스로부터 실제 레코드의 값을 즉시 로딩해 덮어 씌운다는 의미이다.

### DETACH

- 엔티티를 영속성 컨텍스트로부터 분리하면 연관된 엔티티들도 분리된다.

### ALL

- 위 모든 Cascade Type들이 적용된다.

## JPA Cascade가 위험한 이유

### 참조 무결성 제약조건 위반 가능성

- 참조 무결성 제약 조건이란, RDB에서 relation은 참조할 수 없는 외래 키를 가져서는 안된다는 조건을 의미한다.
- `CascadeType.REMOVE` 혹은 `CascadeType.ALL` 옵션을 사용하면 엔티티 삭제 시 **연관된 엔티티들이 전부 삭제되기 때문에 참조 무결성 제약 조건을 위반할 수도 있다.**
- 예를 들어, Comment와 Post가 다대일이고, Comment 엔티티에 `CascadeType.REMOVE`를 지정한 경우, comment1을 삭제했을 때 연관 엔티티 Post까지 삭제될 우려가 있다.
    
    ```java
    // Comment.java
    @Entity
    public class Comment {
    
        @ManyToOne(cascade = CascadeType.REMOVE)
        @JoinColumn(name = "post_id")
        private Post post;
        // ...생략
    }
    ```
    

### 양방향 연관관계 매핑 시 충돌 가능성

- Comment, Post가 있고, 연관 관계의 주인이 Comment일 때, Post 필드에 `cascadeType.PERSIST`를 지정하는 경우 문제가 생길 수 있다.
    
    ```java
    // Post.java
    @Entity
    public class Post {
    
        @OneToMany(mappedBy = "post", cascade = CascadeType.PERSIST)
        private List<Comment> comments = new ArrayList<>();
    ```
    
- comment를 삭제하더라도 post를 save하면 다시 comment값이 복원되는 문제가 발생한다.
    
    ```java
    @Test
    void bidirectional_bad_case() {
        Post post = new Post();
        Comment comment1 = new Comment();
        Comment comment2 = new Comment();
    
        post.addComment(comment1);
        post.addComment(comment2);
    
        commentRepository.delete(comment1);
        postRepository.save(post);
    
        assertThat(commentRepository.existsById(comment1.getId())).isTrue();
      }
    ```
    
    - 이처럼 영속화에 대한 관리 지점이 두 곳이면 데이터 값을 예측할 수 없는 문제가 발생한다.
    - 이를 해결하기 위해서는 post에서도 comment를 제거하도록 편의 메소드를 구현해야 한다.

## 언제 사용하는가?

- JPA Cascade는 **엔티티 간 관계가 명확할 때 사용하는 것을 권장**한다.
- **게시물과 댓글의 관계처럼 부모 - 자식 구조가 명확**하다면 Cascade를 사용하는 것이 괜찮은 선택이다.
    - 그러나 하나의 자식에 여러 부모가 대응되는 경우(수강중인 수업과 학생의 관계 등)에는 사용하지 않는 것이 좋다.
    - 여러 부모가 대응되는 순간 충돌 문제가 발생하기 때문이다.