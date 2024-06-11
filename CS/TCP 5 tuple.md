## TCP 5 tuple

- 네트워크 통신에서 5가지 요소를 하나의 key로 정의한 내용
- 통신을 통해 전달되는 모든 데이터 포맷은 5-tuple이라는 규격에 맞춰 흐르게 된다

### 요소

1. source ip (출발지 ip)
2. destination ip (목적지 ip)
3. protocol (프로토콜) - TCP/UDP
    1. 주로 tcp, udp를 구분하며 0은 tcp, 1은 udp로 전달한다.
    2. 전달 데이터 포맷은 주로 tinyint
4. source port (출발지 port)
5. destination port (목적지 port)

## 참고 자료

- [5 tuple 이란? (tistory.com)](https://test-myid.tistory.com/11)
- [[스터디] 우리는 어떻게 다양한 사이트를 여행 다닐 수 있을까 - (2) TCP/IP 데이터 전송과정에 대한 이해와 소켓 (velog.io)](https://velog.io/@seungrok-yoon/how-we-travel-internet-2)