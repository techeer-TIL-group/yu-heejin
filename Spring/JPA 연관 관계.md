## 연관 관계 정의 규칙

### 단방향, 양방향

- RDB 테이블은 외래 키 하나로 양 테이블 조인이 가능하다.
    - 따라서 관계에 있어서 방향을 따지지 않는다.
- 그러나 객체의 경우, **자신이 갖고 있지 않은 참조에 대해 접근할 수 있는 방법이 없다.**
    - `@ManyToOne`, `@OneToMany` 등으로 관계를 매핑한다 하더라도 반대쪽 객체에서는 접근이 불가능하다.
    - 참조용 필드가 있는 객체만 다른 객체를 참조하는 것이 가능하기 때문이다.
    - 따라서 **두 객체 사이에 하나의 객체만 참조용 필드를 갖고 참조하면 단방향 관계**, **두 객체 모두가 각각 참조용 필드를 갖고 참조하면 양방향 관계**라고 한다.
- 방향 선택 기준은 **비즈니스 로직에서 두 객체가 참조가 필요한지 여부를 고민하면 된다.**
    - Board에서 User 정보 참조가 필요한 경우 Post → User 단방향 참조
    - User에서 User가 작성한 Board 정보들이 필요한 경우 User → Post 단방향 참조
    
    → 위와 같이 비즈니스 로직에 맞게 선택했는데, 두 객체가 서로 단방향 참조를 했다면 양방향 연관 관계가 되는 것이다.
    

### 연관 관계의 주인

- 두 객체가 양방향 관계를 맺는 경우, 연관 관계의 주인을 지정해야 한다.
- 연관 관계의 주인을 지정하는 것은 **두 단방향 관계 중 제어의 권한(테이블 레코드 저장, 수정, 삭제 등)을 갖는 실질적인 관계가 어떤 것인지 JPA에게 알려주는 것이다.**
- **연관 관계의 주인은 연관 관계를 갖는 두 객체 사이에서 CRUD를 할 수 있지만, 주인이 아닌 경우 조회만 가능하다.**
- 연관 관계의 주인이 아닌 객체에서 `mappedBy` 속성을 사용해서 주인을 지정해야 한다.
    - 일반적으로 외래 키가 있는 곳을 연관 관계의 주인으로 지정한다.

### 다중성

- 데이터베이스를 기준으로 다중성을 결정한다.
- 연관 관계는 다음과 같은 대칭성을 갖는다.
    - 일대다 ↔ 다대일
    - 일대일 ↔ 일대일
    - 다대다 ↔ 다대다
- 다대일(N:1)
    - 다대일 단방향에서는 N인 객체에 `@ManyToOne`만 추가한다.
        
        ```java
        @Entity
        public class Post {
            @Id @GeneratedValue
            @Column(name = "POST_ID")
            private Long id;
        
            @Column(name = "TITLE")
            private String title;
        
            @ManyToOne
            @JoinColumn(name = "BOARD_ID")
            private Board board;
            //... getter, setter
        }
        
        @Entity
        public class Board {
            @Id @GeneratedValue
            private Long id;
            private String title;
            //... getter, setter
        }
        ```
        
    - 다대일 양방향으로 만들려면 1인 객체에 `@OneToMany`를 추가하고, 양방향 매핑을 사용했기 때문에 연관 관계의 주인을 `mappedBy`로 지정한다.
        
        ```java
        @Entity
        public class Post {
            @Id @GeneratedValue
            @Column(name = "POST_ID")
            private Long id;
        
            @Column(name = "TITLE")
            private String title;
        
            @ManyToOne
            @JoinColumn(name = "BOARD_ID")
            private Board board;
            //... getter, setter
        }
        
        @Entity
        public class Board {
            @Id @GeneratedValue
            private Long id;
            private String title;
        
            @OneToMany(mappedBy = "board")
            List<Post> posts = new ArrayList<>();
            //... getter, setter
        }
        ```
        
- 일대다(1:N)
    - 일대다 단방향
        
        ```java
        @Entity
        public class Post {
            @Id @GeneratedValue
            @Column(name = "POST_ID")
            private Long id;
        
            @Column(name = "TITLE")
            private String title;
          //... getter, setter
        }
        
        @Entity
        public class Board {
            @Id @GeneratedValue
            private Long id;
            private String title;
        
            @OneToMany
            @JoinColumn(name = "POST_ID") //일대다 단방향을 @JoinColumn필수
            List<Post> posts = new ArrayList<>();
            //... getter, setter
        }
        ```
        
        - 데이터베이스에서는 무조건 N인 쪽에서 외래키를 관리한다.
        - 그러나 일대다 단방향을 사용하면 1인 객체에서 N인 객체를 CRUD할 수 있다.
        - 실무에서는 1:N 단방향은 거의 사용하지 않는다.
    - 일대다 양방향은 사용하는 것을 금지한다.
- 일대일 (1:1)
    - 일대일 단방향
        
        ```java
        @Entity
        public class Post {
            @Id @GeneratedValue
            @Column(name = "POST_ID")
            private Long id;
        
            @Column(name = "TITLE")
            private String title;
            @OneToOne
            @JoinColumn(name = "ATTACH_ID")
            private Attach attach;
            //... getter,setter
        }
        @Entity
        public class Attach {
            @Id @GeneratedValue
            @Column(name = "ATTACH_ID")
            private Long id;
            private String name;
          //... getter, setter
        }
        ```
        
    - 일대일 양방향
        - 위 코드에서 `mappedBy`만 지정하면 된다.

## 양방향 연관 관계(mappedBy)

- 일대다 구조에서 1인 객체가 N인 객체의 정보를 조회하려면 `List<Object>` 타입의 필드를 가져야 한다.
    
    ```java
    @Getter @Setter
    @Entity
    public class Team {
        @Id @GeneratedValue
        private Long id;
        private String teamName;
        @OneToMany
        private List<Member> members = new ArrayList<>();
    }
    ```
    
- 만약 JPA에서 객체가 양방향 참조 관계를 가진다면, 외래키를 갖는 쪽, 즉 연관관계의 주인이 되는 쪽을 정해주어야 한다.
    - 1:N 구조의 경우 N으로 해주면 된다.
- 주인을 결정하는 이유는, 만약 양방향 매핑을 적용할 경우 주인이 아닌 객체도 테이블 접근이 가능하기 때문에 혼란이 올 수 있기 때문이다.
    - 또한, 서로의 객체가 테이블 데이터 값을 변경하려 한다면 무결성을 해칠 가능성도 존재한다.
    - 따라서 **두 객체 중 하나의 객체만 테이블을 관리할 수 있도록 정하는 것이 MappedBy 옵션이다.**

## 참고 자료

- https://jeong-pro.tistory.com/231
- [https://velog.io/@dhk22/JPA-양방향-연관관계](https://velog.io/@dhk22/JPA-%EC%96%91%EB%B0%A9%ED%96%A5-%EC%97%B0%EA%B4%80%EA%B4%80%EA%B3%84)
- [https://velog.io/@wogh126/JPA-양방향-매핑에서-MappedBy가-필요한-이유](https://velog.io/@wogh126/JPA-%EC%96%91%EB%B0%A9%ED%96%A5-%EB%A7%A4%ED%95%91%EC%97%90%EC%84%9C-MappedBy%EA%B0%80-%ED%95%84%EC%9A%94%ED%95%9C-%EC%9D%B4%EC%9C%A0)