## Redis Publish / Subscribe

<img width="656" alt="스크린샷 2023-03-24 오전 11 25 46" src="https://user-images.githubusercontent.com/96467030/227446941-447cd28e-3079-49ea-98ca-7a95553d6310.png">

- publish / subscribe란 **특정한 주제(topic)에 대해 해당 topic을 구독한 모두에게 메시지를 발행하는 통신 방법**으로, 채널을 구독한 수신자(클라이언트) 모두에게 메세지를 전송하는 것을 의미한다.
- 하나의 클라이언트가 메세지를 publish하면, **이 topic에 연결되어 있는 다수의 클라이언트가 메세지를 받을 수 있는 구조**이다.
- 유튜브 채널 구독을 예시로 들면, 구독과 좋아요(subscribe)를 누르면 나중에 크리에이터가 **새로운 글을 발행(publish)했을 때 구독자에게만 알림(notification)**이 오게 되는 원리라고 보면 된다!
- Pub/Sub 구조에서 사용되는 Queue를 일반적으로 topic이라 한다.
- **Redis의 pub/sub 기능은 주로 채팅 기능이나 푸시 알림 등에 사용**된다.
    - 날씨 정보를 구독한 사람에게 주기적으로 날씨 정보를 보내거나, 특정한 작업을 반복 수행하는 작업자에게 비동기적으로 작업을 보내 처리하도록 하거나, 현재 앱에 로그인한 유저에게 푸시를 발송하는 활동들이 모두 이러한 원리로 만들어진다.
- 이러한 redis의 pub/sub 시스템은 매우 단순한 구조로 되어있다.
    - 채널에 구독 신청을 한 모든 구독자에게 메세지를 전달한다.
    - 하지만, 메세지를 던지는 시스템이기 때문에 **메시지를 따로 보관하지도 않는다.**
    - 즉, **수신자(클라이언트)가 메세지를 받는 것을 보장하지 않아, subscribe 대상이 하나도 없는 상황에서 메세지를 publish 해도 역시 사라진다.**
    - 따라서 일반 메세지 큐처럼 수신 확인을 하지 않는다. (전송 보장을 하지 않음)
- 웹 소켓을 이용할 경우 추가적인 네트워크 통신이 필요하기 때문에 딜레이가 생길 수 있지만, **레디스는 in-memory 기반이라 매우 빨리 메세지를 받을 수 있다.**
    - 따라서 현재 접속 중인 클라이언트에게 짧고 간단한 메시지를 빠르게 보내고 싶을 때, 그리고 전송된 메세지를 따로 저장하거나 수신 확인이 필요 없을 때, 100% 전송 보장은 하지 않아도 되는 데이터를 보낼 때 이용하면 괜찮다.

## Pub/Sub 명령어

<img width="676" alt="스크린샷 2023-03-24 오전 11 41 29" src="https://user-images.githubusercontent.com/96467030/227446979-e5d6757e-14b7-46e4-b316-19e4085d2ad5.png">

- Redis 서버를 매개로, **Redis 클라이언트 간 통신**을 도와준다.
- Redis 클라이언트는 Redis 서버 내 ‘채널’을 생성한다.
- 메시지를 수신하고 싶은 클라이언트는 사전에 해당 채널을 subscribe 한다.
- 메세지를 보내는 클라이언트는 해당 채널에 메시지를 publish 할 수 있다.
- 메시지를 보내는 클라이언트가 메시지를 publish하면, subscribe 중인 클라이언트만 메시지를 수신한다.

| Commands | Syntax | Description |
| --- | --- | --- |
| subscribe | channel [channel ...] | 채널을 구독하여, 메세지를 수신 받는다. (동시에 여러개 구독 가능) |
| publish | channel message | 메시지를 지정한 채널로 송신 |
| pubsub | subcommand [argument [argument ...]] | 서버에 등록된 채널이나 패턴을 조회 |
| psubscribe | pattern [pattern ...] | 채널 이름을 패턴으로 등록 |
| unsubscribe | [channel [channel ...]] | subscribe로 등록한 채널 구독 해제 |
| punsubscribe | [pattern [pattern ...]] | psubscribe로 등록한 패턴 채널 구독 해제 |

## subscribe 명령어

```bash
> subscribe <채널명>
> subscribe <채널명1> <채널명2> ... # 여러 개의 채널을 등록하면, 각각의 채널로 메시지가 발행되면 모두 수신한다.
```

- 파라미터로 명시한 채널을 구독하며, 여러 개의 채널을 동시에 구독할 수도 있다.
- 결과값으로 구독한 채널명과 integer를 반환한다.
- **redis에서는 channel을 생성하는 명령어는 존재하지 않고, subscribe로 채널을 생성하고 구독하는 것으로 보면 된다.**

## publish 명령어

```bash
> publish <채널> <내용>
(integer) 2 # 채널에 메시지를 발행했고, 2개의 클라이언트에게 전달되었음
```

- ch1을 구독하고 있는 subscriber가 생겼으므로, **publisher client에서 publish 명령어를 사용해 메시지를 발행**하면 subscriber client에서는 해당 메세지를 전달받게 된다.
    
    <img width="1139" alt="스크린샷 2023-03-24 오전 11 53 32" src="https://user-images.githubusercontent.com/96467030/227447001-293bfa75-f5ae-4c75-b045-aba7900dff21.png">

    

## pubsub 명령어

```bash
# 활성화된 채널이 없을 때
> pubsub channels
(empty array)

# 아래의 SUBSCRIBE 명령어로 채널을 한 개 활성화 시켰을 때
> pubsub channels
1) "c1"

# 채널을 구독중인 클라이언트 수 확인
> pubsub numsub ch1
1) "ch1"
2) (integer) 1

# 패턴형으로 등록된 클라이언트 수 확인
> pubsub numpat
(integer) 0
```

- admin이 효율적으로 subscriber를 관리할 수 있도록 하는 명령어이다.
- 다음과 같은 세 가지 subcommand가 있다.
    1. channels : 활성화된 채널
    2. numsub : 특정 채널을 구독하고 있는 subscriber의 개수를 확인 (pattern subscription으로 구독하고 있는 subscriber는 count에 포함되지 않음)
    3. numpat : pattern subscription의 subscriber 개수 확인 (특정 채널의 subscriber가 아닌, 전체 subscriber의 개수를 return)

## psubscribe 명령어

```bash
# 패턴을 등록하여 수신시작
> psubscribe c*
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "c*"
3) (integer) 1

# 패턴 채널을 등록된 채널에는 포함되지 않음
127.0.0.1:6379> pubsub channels
1) "c1"

# 패턴 조회 : 1개 확인
127.0.0.1:6379> pubsub numpat
(integer) 1

# 등록하지 않았던 채널 ch2로 메시지 보냄
> publish ch2 "test 2"
(integer) 1

# 패턴으로 구독 신청한 채널에 메시지가 추가됨
> psubscribe c*
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "c*"
3) (integer) 1
1) "pmessage"
2) "c*"
3) "c2"
4) "test 2"
```

- 수신할 채널 이름의 패턴을 등록한다.

### glob-style

1. ‘?’은 한 글자를 대치한다.
    1. h?llo는 hello, hallo, hxllo 같은 것을 의미한다.
2. ‘*’은 공백이나 여러 글자를 대치한다.
    1. h*llo는 hllo, heeeeello 같은 것을 의미한다.
3. h[ae]llo는 ‘a’나 ‘e’만 올 수 있다.
    1. hello, hallo는 가능하고 hillo는 안된다.

## unsubscribe / punsubscribe 명령어

```bash
> unsubscribe ch1*
1) "unsubscribe"
2) "ch*"
3) (integer) 1

> punsubscribe ch1*
1) "punsubscribe"
2) "ch*"
3) (integer) 1
```

- 기본적으로 redis-cli에서는 ctrl + c로 구독을 종료할 수 있지만, 별도 클라이언트로 붙었다면 unsubscribe 명령어를 통해 수신을 중단할 수 있다.
- 마찬가지로 punsubscribe 명령어로 일치하는 패턴의 채널 수신을 중단한다.
- 채널명을 입력하지 않으면 해당 클라이언트에 등록된 모든 채널을 삭제한다.

## 참고 자료

- [https://inpa.tistory.com/entry/REDIS-📚-PUBSUB-기능-소개-채팅-구독-알림](https://inpa.tistory.com/entry/REDIS-%F0%9F%93%9A-PUBSUB-%EA%B8%B0%EB%8A%A5-%EC%86%8C%EA%B0%9C-%EC%B1%84%ED%8C%85-%EA%B5%AC%EB%8F%85-%EC%95%8C%EB%A6%BC)