## Namespace

<img width="673" alt="스크린샷 2023-03-24 오전 9 59 44" src="https://user-images.githubusercontent.com/96467030/227407034-eca35f77-4ca3-42de-abac-c88afd80d025.png">


- 네임스페이스란 Express의 라우팅처럼 **url에 지정된 위치에 따라 신호의 처리를 다르게하는 기술이다.**
- 서버와 클라이언트가 연결되면 실시간 데이터 공유가 가능한데, **소켓을 그냥 사용하게 되면 데이터가 모든 socket으로 들어가게 된다.**
    - 하지만, 특정 페이지에서 소켓이 보내주는 모든 실시간 메시지를 모두 받을 필요는 없다.
    - 따라서, **특정 노드끼리만 연결해주는 것이 namespace**
- 기본 네임스페이스인 /에 신호를 전송하고 수신하는 것이 아니라, 다른 네임스페이스를 만들어서 신호를 각기 독립적으로 처리하는 것이다.
    - 즉, **지정한 Namespace에 있는 소켓 끼리만 통신한다는 개념이다.**

## Namespace 예제 코드

### 서버

```tsx
// 네임스페이스 등록
**const room = io.of('/room');
const chat = io.of('/chat');**

// room 네임스페이스 전용 이벤트
room.on('connection', (socket) => {
  console.log('room 네임스페이스에 접속');
  
  socket.on('disconnect', () => {
     console.log('room 네임스페이스 접속 해제');
  });
  
  socket.emit('newroom', '방 만들어'); // 같은 room 네임스페이스 소켓으로만 이벤트가 날라간다.
});

// chat 네임스페이스 전용 이벤트
chat.on('connection', (socket) => {
  console.log('chat 네임스페이스에 접속');

  socket.on('disconnect', () => {
     console.log('chat 네임스페이스 접속 해제');
     socket.leave(roomId);
  });
  
  socket.emit('join', '참여') // 같은 chat 네임스페이스 소켓으로만 이벤트가 날라간다.
});
```

- `io.of(’/something’)`을 통해 namespace를 `/room`과 `/chat`으로 나누어 지정해주었다.
- **네임스페이스의 연결 처리는 제각각이라 연결 콜백(connection)을 따로따로 등록**해준다.
- 이렇게 되면 namespace 객체는 클라이언트에서 /room, /chat 네임스페이스를 사용하는 소켓과만 통신을 하게 된다.

### 클라이언트 1

```tsx
<script src="/socket.io/socket.io.js"></script>
<script>
  const socket = io.connect('http://localhost:8005/room', { // room 네임스페이스
     path: '/socket.io'
  });

  // newRoom 이벤트 시 room 네임스페이스 에서만 통신 하게 된다.
  socket.on('newRoom', function (data) {
    // ...
  });

  // removeRoom 이벤트 시 room 네임스페이스 에서만 통신 하게 된다.
  socket.on('removeRoom', function (data) {
    // ...
  });
</script>
```

### 클라이언트 2

```tsx
<script src="/socket.io/socket.io.js"></script>
<script>
  const socket = io.connect('http://localhost:8005/chat', {path: '/socket.io'}); // chat 네임스페이스

  // join 이벤트 시 chat 네임스페이스 에서만 통신 하게 된다.
  socket.on('join', function (data) {
	// ...
  });

  // exit 이벤트 시 chat 네임스페이스 에서만 통신 하게 된다.
  socket.on('exit', function (data) {
	// ...
  });
</script>
```

- 클라이언트 코드를 보면 **네임스페이스는 url 주소로 연결**되는 것을 알 수 있다.
- **같은 경로를 가진 서버와 클라이언트 소켓들이 서로 송수신**을 하게 되는 것이다.

## 외부 파일에서 특정 Namespace에 이벤트 보내기

- **io 객체를 전역 변수에서 더 나아가 전 파일에서 공통으로 사용할 수 있는 public 변수로 만들어주는 것**이다.
- public 변수로 만들면 외부 파일에서 만일 io 객체에 접근해 사용할 필요가 있다면 간단하게 `req.app.get(’io’)`로 가져와 소켓 IO 기능을 마음껏 사용할 수 있는 것이다.

### socket.js

```tsx
const SocketIO = require('socket.io');
const io = SocketIO(server, { path: '/socket.io' });
   
// path를 지정한 고유한 io객체를 전역으로 등록. 
// 전역변수로 등록함으로서, 다른 파일에서 바로 io객체를 가져와 소켓 설정을 할 수 있다.
app.set('io', io);
```

### 외부파일.js

```tsx
//^ req.app.get('io') / res.app.get('io') 를 사용해서 익스프레스 객체로 io를 가져와 사용할 수 있게 된다.
const io = req.app.get('io'); // 전역변수로 등록해논 io객체를 가져옴
io.of('/room').emit('newRoom', "message"); // 특정 room네임스페이스에게만 newRoom 이벤트와 메세지를 보냄
```

## Namespace 종류

### Default namespace

```tsx
// 기본 네임스페이스 이벤트 수신
io.on('connection', socket => {
  socket.on('disconnect', () => {});
});

// 기본 네임스페이스 이벤트 송신
io.sockets.emit('hi', 'everyone');
io.emit('hi', 'everyone'); // short form
```

- Default namespace를 / 라고 부르며, **기본적으로 연결되는 Socket.IO 클라이언트와 서버가 기본적으로 수신하는 클라이언트**
- 해당 네임스페이스는 **io.sockets 또는 간단히 io로 식별**된다.

### Custom namespace

```tsx
const nsp = io.of('/my-namespace');

nsp.on('connection', socket => {
  console.log('someone connected');
});

nsp.emit('hi', 'everyone!');
```

- 사용자 정의 네임스페이스를 사용하려면 `io.of`를 사용한다.

## Namespace middleware

```tsx
const cookieParser = require('cookie-parser');
const io = SocketIO(server, { path: '/socket.io' });

// default namespace
io.use((socket, next) => {
  if (isValid(socket.request)) {
    // 외부모듈 미들웨어를 안에다 쓰일수 있다. 미들웨어 확장 원칙에 따라 res, req인자를 준다
    cookieParser(process.env.COOKIE_SECRET)(socket.request, socket.request.res, next);
    next();
  } else {
    // next(인수)를 하면 바로 똑같이 Handling middleware error로 넘어가게 된다.
    next(new Error('invalid'));
  }
});

// custom namespace
io.of('/admin').use(async (socket, next) => {
  const user = await fetchUser(socket.handshake.query);
  if (user.isAdmin) {
    socket.user = user;
    next();
  } else {
    next(new Error('forbidden'));
  }
});
Copy
```

- **모든 소켓에 대해 실행**되는 함수로, **소켓과 다음 등록된 미들웨어로 실행을 선택적으로 조절할 수 있다.**
    - 즉, express의 app.use의 소켓 IO 버전이라고 생각하면 된다.
- **next()를 통해 다음 메소드로 넘길 수 있고, 다른 미들웨어를 소켓 메소드 내에 선언하여 사용할 수 있다.**

## Handling middleware error

```tsx
// ... next(new Error('inavlid'))

socket.on('connect_error', (err) => {
  console.log(err.message); // prints the message associated with the error, e.g. "thou shall not pass" in the example above
});
```

- **next 메소드가 Error 객체와 함께 호출되면 클라이언트는 connect_error 이벤트를 수신한다.**
- express의 에러처리 미들웨어의 소켓 IO 버전이라고 볼 수 있다.

## Compatibility with Express middleware

- **네임스페이스 미들웨어 안에서 기존 Express 미들웨어 모듈과 함께 호환시켜 사용**하려면, 미들웨어 확장 절칙에 따라 **(res, req, next) 인자**를 주어야 한다.
    
    ```tsx
    io.use((socket, next) => {
    	// 외부모듈 미들웨어를 안에다 쓰일수 있다. 미들웨어 확장 원칙에 따라 res, req인자를 준다 (후술)
        cookieParser(process.env.COOKIE_SECRET)(socket.request, socket.request.res, next);
    });
    ```
    
- 공식 문서에는 **wrap이라는 helper 함수 기법**을 제공해주는데, 이 기법을 사용하면 app.use()와 같이 좀 더 깔끔하게 미들웨어들을 배치할 수 있다.
    
    ```tsx
    // 외부 모듈 미들웨어
    const session = require('express-session');
    const passport = require('passport');
    
    // 미들웨어를 인자로 받아 자동으로 (req, res, next) 인자를 붙여줘서 호출하게 만드는 helper 함수
    const wrap = (middleware) => (socket, next) => middleware(socket.request, socket.request.res, next);
    
    // app.use() 와 같이 깔끔하게 미들웨어들을 배치할수 있게 된다.
    io.use(wrap(session({ secret: 'cats' })));
    io.use(wrap(passport.initialize()));
    io.use(wrap(passport.session()));
    
    io.use((socket, next) => {
      if (socket.request.user) {
        next();
      } else {
        next(new Error('unauthorized'))
      }
    });
    ```
    

## Room

<img width="642" alt="스크린샷 2023-03-24 오전 10 45 55" src="https://user-images.githubusercontent.com/96467030/227407123-67a286a0-865a-469e-96ef-96bf2b4d61f4.png">


- Room은 Namespace의 하위 개념이다. (IO → Namespace → Room → Socket)
- **namespace 안에 있는 소켓들을 room으로 쪼개 나눈 것이다.**
- room에 join 되어 있는 클라이언트만 데이터 송수신이 가능해진다.
    - 즉, 각 클라이언트는 소켓을 가지게 되며, 해당 소켓은 네임스페이스를 가지고, 각 네임스페이스는 room을 가진다.
- **네임스페이스를 통해 큰 줄기의 데이터 통신을 만들고 룸을 통해 미세하게 소캣을 연결할 수 있다.**
    
    <img width="584" alt="스크린샷 2023-03-24 오전 10 53 43" src="https://user-images.githubusercontent.com/96467030/227407168-4bd7068d-9eb9-40a6-8883-f51a496199cd.png">
    
- 아래의 코드를 살펴보면 join, to, leave 등 친숙한 단어들이 보이는데, 이는 socket.io에서 채팅방의 논리를 미리 메서드로 구현해놓은 것이다.
    
    
    <img width="640" alt="스크린샷 2023-03-24 오전 10 57 33" src="https://user-images.githubusercontent.com/96467030/227407174-b0e0015b-2520-41c9-ab33-69c92c1ee2e8.png">

    
    - req.headers.referer에는 요청 주소 uri가 들어있다.
    - 이를 정규식으로 파싱해서 **roomID만 추출한 후, socket.join의 인자로 넣어 특정 방에 입장할 수 있다.**
    - socket.leave로 방에서 나갈 수 있다.
    - socket.to를 통해 특정 룸에게만 이벤트를 emit할 수 있다.
    - 즉, **join, leave, to의 개념은 socket.io의 Room(채팅방)을 참가/떠나다/~에게 개념**으로 보면 된다!
        <img width="652" alt="스크린샷 2023-03-24 오전 11 06 06" src="https://user-images.githubusercontent.com/96467030/227407253-dfbd7032-cffe-40b9-af52-252f4607ba48.png">
        

### 예시

- 예를 들어 카톡 단톡방 두 개가 있다고 가정하자.
- 단톡방 1에 메시지를 보내면 단톡방 1의 사용자들은 메시지를 받지만, 다른 단톡방의 사용자들은 그 메시지를 받지 못한다.
- 소켓들의 룸도 단톡방과 비슷한데, **특정 룸에 신호를 보내면 룸 안의 소켓들은 신호를 받지만, 다른 룸에 소속된 소캣들은 신호를 받지 못한다.**

## 외부 파일에서 네임스페이스 안의 룸에 이벤트를 보내기

```tsx
req.app.get('io').of('/chat').to(roomId).emit('chat', chat);
```

- 네임스페이스 포스팅에서 한 것과 유사하게 **to(roomId) 메소드**로 룸 ID를 특정해서 보내면 된다.

## Namespace & Room

```tsx
// 전체 보내기
req.app.get('io').emit('이벤트', 메세지);

// 네임스페이스에 있는 유저한테만 보내기
req.app.get('io').of('네임스페이스').emit('이벤트', 메세지);

// 네임스페이스에 있으면서, 그안에 룸에 있는 유저한테만 보내기
req.app.get('io').of('네임스페이스').to(roomId).emit('이벤트', 메세지);

// 특정 유저한테만 보내기 (1:1대화, 귓속말)
req.app.get('io').to(socket.id).emit('이벤트', 메세지);

// 나를 제외한 모든 유저에게 보내기
req.app.get('io').broadcast.emit('event_name', msg);
```

> **같은 네임스페이스 안에서 소통하여 소켓 송수신을 분리**할 수 있다.
> 

> **네임스페이스 안의 같은 룸 안에서만 소통**하게 하여, 더 세분하게 소켓 송수신을 분리할 수 있다.
> 
- IO : 전역
- 네임스페이스 <path> : 전역에서 나눈 공간
- room ID : 네임스페이스에서 세분히 나눈 공간
- 소켓 ID : 어느 특정 사용자와 소통용

## 참고 자료

- [https://inpa.tistory.com/entry/SOCKET-📚-Namespace-Room-기능#room_개념](https://inpa.tistory.com/entry/SOCKET-%F0%9F%93%9A-Namespace-Room-%EA%B8%B0%EB%8A%A5#room_%EA%B0%9C%EB%85%90)