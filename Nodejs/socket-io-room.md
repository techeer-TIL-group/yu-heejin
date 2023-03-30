- **Rooms는 접속된 클라이언트들을 룸으로 나눠서 관리할 수 있는 수단**을 제공한다.
- 어떤 룸에 있는 클라이언트 모두에 이벤트를 보내는 것(emit)을 쉽게 할 수 있게 해준다.

## Join과 Leave

- 특정 소켓을 어떤 룸에 join하려면 다음과 같은 방법으로 join() 함수를 사용한다.
    
    ```jsx
    socket.join('room');
    ```
    
- leave할 때는 leave() 함수를 사용한다.
    
    ```jsx
    socket.leave('room');
    ```
    
- 활용 예시
    
    ```jsx
    socket.on('subscribe', function(data) { socket.join(data.room); });
    socket.on('unsubscribe', function(data) { socket.leave(data.room); });
    ```
    

## 이벤트 보내기(emitting)

- 룸에 있는 모든 클라이언트에게 이벤트를 보내는 방법
    
    ```jsx
    socket.broadcast.to('room');
    
    // or
    
    io.sockets.in('room');
    ```
    

## 소켓에 브로드캐스트(Broadcast)

- 룸 내에 있는 자신 이외의 모든 클라이언트에 브로드캐스트한다.
    
    ```jsx
    io.sockets.on('connection', function(socket) {
    	// 해당 소켓 이외의 룸 내의 모두에게 송신한다.
    	socket.broadcast.to('room').emit('event_name', data)
    });
    ```
    
- 룸을 지정하지 않으면 자신을 제외한 접속된 모든 클라이언트에 전송
    
    ```jsx
    io.sockets.on('connection', function(socket) {
    	// 해당 소켓 이외의 모두에게 송신
    	socket.broadcast.emit('event_name', data);
    });
    ```
    

## io.sockets 사용하기

- 특정 룸 내의 모든 클라이언트에 송신
    
    ```jsx
    io.sockets.in('room').emit('event_name', data);
    ```
    
- 모든 클라이언트에 송신
    
    ```jsx
    io.sockets.emit('event_name', data);
    ```
    
- 특정 네임 스페이스 내의 어떤 room 내에 있는 모든 클라이언트에 송신
    
    ```jsx
    io.of('namespace').in('room').emit('event_name', data);
    ```
    

## room 정보 구하기

### 모든 room 리스트

```jsx
io.sockets.manager.rooms
```

- room의 이름이 키인 해쉬
- 값은 그 **room의 소켓 아이디들의 배열**이다.
- room 명 앞에 `/` 가 붙어 있다는 점에 주의한다.
    - 내부적으로 쓰이기 때문에 join(), leave()할 때 `/` 을 붙일 필요는 없다.

### room 내의 모든 클라이언트

```jsx
io.sockets.clients('room')
```

- **room 내의 모든 클라이언트(socket 인스턴스)의 리스트**를 반환한다.
- `io.of(’namespace’).clients(’room’)` 처럼 네임 영역을 지정할 수도 있다.

### 특정 클라이언트가 join해 있는 모든 룸

```jsx
io.sockets.manager.roomClients[socket.id];
```

- socket이 join해 있는 모든 room들의 이름이 구해진다.
- room 명에 `/` 가 붙어 있음에 주의한다.
- 또한, 모든 클라이언트는 기본적으로 무명의 룸 “ ”에 들어 있다.

## 참고 자료

- [https://gipyeonglee.tistory.com/99](https://gipyeonglee.tistory.com/99)