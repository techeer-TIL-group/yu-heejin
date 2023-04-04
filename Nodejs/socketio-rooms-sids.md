## Socket.io의 adapter

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcvh5WQ%2FbtrgDcRY7S1%2FqUCU41id0qJjUfkdFdrk7k%2Fimg.png)

- 우리가 만드는 채팅 서버는 하나의 서버에서 돌아가지만, 사이즈가 커지게 되는 경우 하나의 서버로 돌리지 못하는 경우가 발생한다.
- 이런 경우 특정 소켓은 A서버, 다른 소켓은 B서버에서 돌리는 경우가 생기는데, 이 때 **server A에 연결된 클라이언트는 Server B에 연결된 클라이언트와 소통을 할 수 없다는 문제점이 발생**한다.
- 이러한 경우를 대처하기 위해 만들어진 것이 adapter
    - 두 서버를 연결해 데이터를 전송한다.
- 현재 이 adapter는 메모리에 존재하는데, 실제 서버에서는 DB와 같은 곳에 연결된다.

## rooms & sids

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPIPka%2FbtrgB3uvidn%2Fokx2VxgKhYpG6EDPaZFG4k%2Fimg.png)

### rooms

- rooms에는 **private room과 public room**이 있다.
- private room은 **서버와 브라우저 간의 연결**을 의미한다.
    - 따라서 personal ID로 구성되어 있다.
- public room은 내가 생성한 이름이 앞에 들어가 있게 되며(Key), Set 안에는 personal ID가 들어간다.
    - 해당 Set에는 해당 public room에 연결된 personal ID들이 들어간다.

### sids

- sid는 **personal ID가 key 값으로 들어가 있으며, value에는 personal ID가 접속해있는 방이 들어가 있다.**
    - 즉, 이는 rooms에는 방과 personal ID가 모두 들어있고, sids에는 personal ID만 들어가있다는 의미와 같다.

## 방에 있는 사람 수 세기

- rooms에 있던 key 값들의 value 값의 개수가 해당 room에 있는 사람 수가 된다.

## 참고 자료

- [https://ssocoit.tistory.com/206](https://ssocoit.tistory.com/206)