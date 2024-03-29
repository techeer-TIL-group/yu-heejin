## Builder Pattern

```java
public GetQnADetailResponse toGetQnADetailResponse(QnA result) {
    UserInformation member = toMember(result.getMember());

    return GetQnADetailResponse.builder()
        .member(member)
        .title(result.getTitle())
        .content(result.getContent())
        .likes(result.getLikes())
        .dislikes(result.getDislikes())
        .views(result.getViews() + 1)
        .createdAt(result.getCreatedAt())
        .commentCount(result.getComments().size())
        .build();
}
```

- 빌더 패턴은 복잡한 객체의 생성 과정과 표현 방법을 분리하여 다양한 구성의 인스턴스를 만드는 생성 패턴이다.
- 생성자에 들어갈 매개 변수를 메서드 체이닝으로 하나씩 받고, **마지막에 통합적으로 빌드(**`build()`**)해서 객체를 생성한다.**
- 데이터를 유연하게 받아 **다양한 타입의 인스턴스를 생성**할 수 있기 때문에, **클래스의 선택적 매개변수(Optional)가 많은 상황에서 유용하게 사용된다.**

## 다른 생성 방식과의 비교

### 다양한 타입의 생성자 오버로딩

```java
public class GetAllQnAResponse {

    private Long id;
    @JsonInclude(JsonInclude.Include.NON_NULL)
    private String nickname;
    private String title;
    @JsonInclude(JsonInclude.Include.NON_NULL)
    private String content;
    private Integer likes;
    private Integer views;
    private Integer commentCount;
    private LocalDateTime createdAt;

    @QueryProjection
    public GetAllQnAResponse(
        Long id,
        String title,
        Integer likes,
        Integer views,
        Integer commentCount,
        LocalDateTime createdAt
    ) {
        this.id = id;
        this.title = title;
        this.likes = likes;
        this.views = views;
        this.commentCount = commentCount;
        this.createdAt = createdAt;
    }

    @Builder
    public GetAllQnAResponse(
        Long id,
        String title,
        String content,
        Integer likes,
        Integer views,
        Integer commentCount,
        String nickname,
        LocalDateTime createdAt
    ) {
        this.id = id;
        this.title = title;
        this.content = content;
        this.nickname = nickname;
        this.likes = likes;
        this.views = views;
        this.commentCount = commentCount;
        this.createdAt = createdAt;
    }
}
```

- 위 코드를 보면 알 수 있듯이, **클래스 인스턴스 필드들이 많을 수록 생성자에 들어갈 인자의 수가 늘어나 코드의 가독성이 떨어진다.**

### 자바 빈(Java Bean) 패턴

- 매개변수가 없는 기본 생성자로 객체를 생성한 후, Setter를 사용하여 필드 값을 초기화하는 방식이다.
- 하지만 이러한 방식은 객체 생성 시점에 모든 값을 주입하지 않기 때문에 **일관성 문제와 불변성 문제가 발생한다.**

### 빌더(Builder) 패턴

- 위 방식들의 단점을 해결하기 위해 별도의 Builder 클래스를 만들어 값을 입력받고, 최종적으로 `build()` 메소드로 하나의 인스턴스를 생성해 리턴하는 패턴이다.

## 빌더 패턴 만들기

### 어노테이션 없이 만들어보기

아래와 같이 Student 클래스가 있다고 가정하자.

```java
class Student {
    private int id;
    private String name;

    public Student(int id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

위 클래스에서 빌더 패턴을 아래와 같이 만들 수 있다.

```java
class StudentBuilder {
    private int id;
    private String name;

    public StudentBuilder id(int id) {
	    this.id = id;
	    return this;
	  }
	  
	  public StudentBuilder name(String name) {
	    this.name = name;
	    return this;
	  }
	  
	  public Student build(int id, String name) {
		  return new Student(id, name);
		}
}
```

- `return this`를 통해 빌더 클래스 자기 자신을 반환하면서 **메서드 체이닝을 사용할 수 있다.**
- `build()`를 통해 **최종적으로 Student 객체를 생성하여 반환한다.**

### Spring Boot에서 어노테이션 사용하기

```java
@Builder
@AllArgsConstructor
@Entity
public class QnA extends BaseEntity {

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "member_id")
    private Member member;

    @NotNull
    private String title;

    @NotNull
    private String content;

    @ColumnDefault("0")
    private Integer likes;

    @ColumnDefault("0")
    private Integer dislikes;

    @ColumnDefault("0")
    private Integer views;

    private String imageUrl;

    @JsonManagedReference
    @OneToMany(fetch = FetchType.LAZY, cascade = CascadeType.ALL, mappedBy = "qna")
    List<QnAComment> comments = new ArrayList<>();
}
```

- Spring Boot에서는 `@Builder` 어노테이션을 사용하면 쉽게 빌더 패턴을 사용할 수 있다.
- 해당 어노테이션을 사용한 경우, 아래와 같이 클래스로 변환되는 것을 확인할 수 있다. (인텔리제이에서 확인할 수 있다.)
    
    ```java
    public class QnA extends BaseEntity {
    
    		// ...
    		
        public static QnABuilder builder() {
            return new QnABuilder();
        }
    
        public QnABuilder toBuilder() {
            return new QnABuilder().member(this.member).title(this.title).content(this.content).likes(this.likes).dislikes(this.dislikes).views(this.views).imageUrl(this.imageUrl).comments(this.comments);
        }
    
        public static class QnABuilder {
            private Member member;
            private @NotNull String title;
            private @NotNull String content;
            private Integer likes;
            private Integer dislikes;
            private Integer views;
            private String imageUrl;
            private List<QnAComment> comments;
    
            QnABuilder() {
            }
    
            public QnABuilder member(Member member) {
                this.member = member;
                return this;
            }
    
            public QnABuilder title(@NotNull String title) {
                this.title = title;
                return this;
            }
    
            public QnABuilder content(@NotNull String content) {
                this.content = content;
                return this;
            }
    
            public QnABuilder likes(Integer likes) {
                this.likes = likes;
                return this;
            }
    
            public QnABuilder dislikes(Integer dislikes) {
                this.dislikes = dislikes;
                return this;
            }
    
            public QnABuilder views(Integer views) {
                this.views = views;
                return this;
            }
    
            public QnABuilder imageUrl(String imageUrl) {
                this.imageUrl = imageUrl;
                return this;
            }
    
            public QnABuilder comments(List<QnAComment> comments) {
                this.comments = comments;
                return this;
            }
    
            public QnA build() {
                return new QnA(member, title, content, likes, dislikes, views, imageUrl, comments);
            }
    
            public String toString() {
                return "QnA.QnABuilder(member=" + this.member + ", title=" + this.title + ", content=" + this.content + ", likes=" + this.likes + ", dislikes=" + this.dislikes + ", views=" + this.views + ", imageUrl=" + this.imageUrl + ", comments=" + this.comments + ")";
            }
        }
    }
    
    ```
    

## 빌더 패턴을 사용한 코드 리팩토링하기

기존 코드는 아래와 같다.

```java
public QnA toQnA(CreateQnARequest qnaRequest, Member member) {
    if (qnaRequest.getImageUrl() != null && !qnaRequest.getImageUrl().isEmpty()) {
        String imageUrl = qnaRequest.getImageUrl().toString();
        return QnA.builder()
            .member(member)
            .title(qnaRequest.getTitle())
            .content(qnaRequest.getContent())
            .imageUrl(imageUrl.substring(1, imageUrl.length() - 1))
            .build();
    }

    return QnA.builder()
        .member(member)
        .title(qnaRequest.getTitle())
        .content(qnaRequest.getContent())
        .build();
}
```

- 나쁘지 않은 코드같지만, 중복되는 코드가 많아서 약간 지저분(?)해보이는 코드이다.

빌더 패턴을 사용하기 때문에, 아래와 같이 필요한 데이터만 저장하도록 표현하여 간결하게 수정할 수 있다.

```java
public QnA toQnA(CreateQnARequest qnaRequest, Member member) {
    QnA.QnABuilder qna = QnA.builder()
            .member(member)
            .title(qnaRequest.getTitle())
            .content(qnaRequest.getContent());

    if (qnaRequest.getImageUrl() != null && !qnaRequest.getImageUrl().isEmpty()) {
        String imageUrl = qnaRequest.getImageUrl().toString();
        qna.imageUrl(imageUrl.substring(1, imageUrl.length() - 1));
    }

    return qna.build();
}
```

- 최종적으로 `build()`를 통해 QnA 클래스를 만들기 때문에, 마지막으로 return 시 `build()`하면 된다!

## 참고 자료

- [https://inpa.tistory.com/entry/GOF-💠-빌더Builder-패턴-끝판왕-정리](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EB%B9%8C%EB%8D%94Builder-%ED%8C%A8%ED%84%B4-%EB%81%9D%ED%8C%90%EC%99%95-%EC%A0%95%EB%A6%AC)
- [https://medium.com/sjk5766/lombok-어노테이션이-적용된-코드-확인-8d7f455ddb62](https://medium.com/sjk5766/lombok-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98%EC%9D%B4-%EC%A0%81%EC%9A%A9%EB%90%9C-%EC%BD%94%EB%93%9C-%ED%99%95%EC%9D%B8-8d7f455ddb62)
- [https://velog.io/@park2348190/Lombok-Builder의-동작-원리](https://velog.io/@park2348190/Lombok-Builder%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC)