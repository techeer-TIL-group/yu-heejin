> Adapter는 **모든 클라이언트나 일부 클라이언트에 이벤트를 중계해주는 서버측 컴포넌트**이다.
> 

> Socket 서버가 여러개로 늘어날 때, 기본 메모리 Adapter를 다른 Adapter로 교체해야 하며, 그래야 이벤트들이 모든 클라이언트에서 적절하게 라우팅 될 수 있다.
> 

## Adapter란?

![Untitled](https://socket.io/assets/images/mongo-adapter-88a4451b9d19d21c8d92d9a7586df15b.png)

- socket.io에서 **Adapter는 다른 서버들 사이에 실시간 어플리케이션을 동기화**하는 역할을 맡는다.
- 원래 서버의 메모리에서 기본 Adapter를 사용하고 있다.
    - socket.io의 Adapter가 아닌 기본 제공되는 Adapter
- **서버는 클라이언트와의 커넥션들을 메모리에 저장하며, 데이터베이스에는 아무것도 저장하지 않는다.**
- 서버는 모든 클라이언트에 커넥션을 열어놓아야 하기 때문에 많은 브라우저, 창에서 커넥션이 들어오면 서버는 그 연결들을 감당하지 못해 두 세개씩 늘어나기 시작한다.
    - 이렇게 늘어난 서버는 더이상 같은 memory pool을 사용하지 않는다. 즉, 각 서버는 다른 메모리를 사용하고 있다.
    - 이 뜻은, **어떤 두 명의 사용자가 있다고 가정할 때 그 두 명의 유저는 똑같은 클라이언트 화면을 보고 있지만 다른 서버에 연결되어 있는 상태가 된다는 의미이다!**
    - 이러한 경우에 adapter를 사용해야 한다.
- **adapter는 만약 서버 A에 있는 클라이언트를 이용하는 유저가 서버 B에 있는 유저에게 무언가를 보내고 싶을 때 동기적으로 보낼 수 있게 해준다.**
- **adapter는 누가 연결되었는지, 현재 어플리케이션에 room이 얼마나 있는지를 알려줄 수 있다.**

## Adapter 사용해보기

```jsx
rooms: Map(2) {
  //... 현재 애플리케이션에 있는 모든 room들을 볼 수 있다.
}
sids: Map(2) {
  //... socket id들을 볼 수 있다.
}
```

- socket은 그 자체로 방을 하나 갖고 있다.
    - 따라서 **socket id는 세션의 클라이언트 식별자, 즉 유저 아이디임과 동시에 socket의 private한 방의 아이디라고 이해할 수 있다.**
- 이 adapter를 콘솔로 찍어봤을 때 **rooms는 이 socket.id 들인 private room과 join을 통해 만들어진 public room이 함께 모여있는 객체**이다.
- sids는 socket id만 모여있는 객체이니 rooms는 sids를 포함한다고 볼 수 있다.

### 서버 코드

```jsx
// server 
// 현재 서버 안에 있는 모든 방의 배열 얻기
function publicRooms() {
  const {
    sockets: {
      adapter: { sids, rooms },
    },
  } = wsServer;

  const publicRooms = [];

  rooms.forEach((_, key) => {
    if (sids.get(key) === undefined) {
      publicRooms.push(key);
    }
  });
  return publicRooms; 
}

// 현재 서버 개수 알아내기
function countRoom(roomName) { 
  return wsServer.sockets.adapter.rooms.get(roomName)?.size;
}
  
wsServer.on("connection", (socket) => {
  socket.on("join_room", (roomName) => {
    socket.join(roomName);
    wsServer.to(roomName).emit("join_room", countRoom(roomName));
    // 자신을 포함한 모두에게 메시지 전달
    wsServer.sockets.emit("room_change", publicRooms());
  });
  socket.on("disconnecting", () => {
    socket.rooms.forEach((room) =>
      socket.to(room).emit("bye", countRoom(room) - 1)
    );
  });
  socket.on("disconnect", () => {
    wsServer.sockets.emit("room_change", publicRooms());
  });
```

- `wsServer.sockets.emit()`
    - **애플리케이션에 연결된 모든 socket에 메시지**를 보낸다.
    - 예를 들어, 다른 방안에 있지만 다른 클라이언트에서 생성된 방에 대한 알림을 받을 수 있다.

### 클라이언트 코드

```jsx
socket.on("room_change", (rooms) => {
  const roomList = welcome.querySelector("ul");
  roomList.innerHTML = "";
  // 만약 배열 요소가 하나 있었는데 나가서 다시 빈배열을 전달받으면 화면에 반영되지 않는다. 
  // 빈배열이지만 화면에는 방이 하나 있는 것처럼 나오는데, 이유는 아무 코드가 없다면 빈배열일때 아무것도 하지 않기 때문이다. 
  // 따라서 아래와 같은 코드가 필요하다.
  if(rooms.length === 0) {
    return;
  }
  rooms.forEach(room => {
    const li = document.createElement("li");
    li.innerText = room;
    roomList.append(li);
  }
});
```

- 클라이언트 파일에서 데이터를 전달받아 생성되어 있는 방들을 화면에 보여줄 수 있다.
- `socket.to(roomName).emit()`
    - 방에서 자신을 제외한 모두에게 메시지 전달
- `wsServer.to(roomName).emit()`
    - 방에서 자신을 포함한 모든 유저에게 메시지 전달

## 참고 자료

- [https://velog.io/@forest0501/TIL-15-Socket.IO-Adapter22.07.14](https://velog.io/@forest0501/TIL-15-Socket.IO-Adapter22.07.14)