# HTTP(Hyper Text Transfer Protocol)

- 사용자와 서버 간에 이용되는 요청(request)과 응답(response)에 대한 규약
- 사용자의 웹 브라우저가 웹 사이트 서버에 접속하고, 이미지나 페이지 내용에 대한 정보를 요청하고, 웹 사이트 서버는 요청받은 내용을 사용자에게 주는 일련의 과정들이 해당 규약 내에서 이루어진다.
- Application Layer (7계층)에서 작동하는 프로토콜로, TCP/IP를 기반으로 한다.
- 암호화되지 않은 평문을 전송하는 프로토콜이므로, 패킷 감청을 통해 제 3자가 정보를 들여다볼 수 있다.
    - 이러한 위험을 해결하기 위해 등장한 것이 HTTPS이다.

# HTTPS (HTTP Secure)

- HTTP에 Secure의 S가 붙은 프로토콜로, HTTP에 데이터 암호화가 추가된 프로토콜이다.
    - SSL, TLS와 같은 보안 프로토콜을 기반으로 HTTP로서 통신하는 프로토콜
- 모든 데이터 통신 내용이 암호화되기 때문에 제 3자가 패킷을 감청해도 어떤 정보인지 알 수 없다.
- 정보를 주고받는 서버가 신뢰할 수 있는 서버인지 판별할 수 있도록 도와준다.

## SSL(Secure Socket Layer)/TLS(Transport Layer Security)

![image](https://github.com/user-attachments/assets/8b590b9b-2a50-4a4e-b7b8-411cfb61df75)


> 7계층의 HTTP와 4계층의 TCP 사이에 위치한다.
> 

### SSL

- 보안 소켓 계층
- 사용자와 서버 간의 데이터를 암호화하는 표준 기술
- 대칭키 방식이나 공개키 방식같은 데이터 암호화 기법들을 통해 동작한다.
    - 이 때 SSL 인증서가 등장하는데, 이는 공인된 업체(CA, Certificate Authority)가 보장한다.
    - 따라서 SSL 인증서는 공인된 업체를 통해 구입 가능하다.

### TLS

- 전송 계층 보안
- SSL보다 더 개선된 버전

# SSL 인증서

- SSL 인증서는 SSL 프로토콜에서 사용되는 인증서로, 공인된 CA 기관에서 발급한다.

## 인증서의 역할

- Client가 접속한 사이트가 신뢰할 수 있는 사이트임을 보증한다.
- 통신 과정에서 사용할 Server의 공개 키를 Client에 전달한다.

## 인증서에 담겨있는 정보

- CA 정보와 사이트 도메인 등
- 서버의 공개 키

# SSL/TLS에서의 암호화 방식

> 두 방식의 장단점을 고려해 **SSL/TLS에서는 실제 데이터를 주고받기 위해 대칭키**를 사용하고, **해당 대칭키를 주고받기 위해 공개키 방식**을 사용한다.
> 

## 대칭 키 방식

- 동일한 하나의 키로 암호화 및 복호화하는 방식
- 공개키 방식에 비해 속도가 빠르지만, 암호 통신을 위해 대칭키를 주고 받는 문제에서 키가 유출되는 등의 문제가 발생할 수 있다.

## 공개 키(비대칭키) 방식

- 한 쌍의 공개키-비밀키를 통해 암호화 및 복호화하는 방식
    - 공개키로 암호화하면 비밀키로 복호화, 비밀키로 암호화하면 공개키로 복호화
- 대칭키 방식에 비해 보안이 뛰어나지만 상대적으로 속도가 느리다.

# SSL Handshake (TLS 1.2)

![image](https://github.com/user-attachments/assets/e3f230db-088d-4ce9-9fa9-7919497fe17a)


1. Server는 CA를 통해 SSL 인증서를 생성한다.
    
    ![image](https://github.com/user-attachments/assets/050c3aef-3730-4907-87db-edfa0790eca1)

    
2. CA는 CA의 비밀키를 이용해 사이트 정보와 사이트 공개 키를 암호화해 인증서를 생성하고 사이트에 전달한다.
    
    ![image](https://github.com/user-attachments/assets/f39a0751-2ec0-455f-9f93-edcb3815edda)

    
3. Client가 Server에 TLS 연결을 시도하며 패킷을 보낸다.
    1. 사용 가능한 ciper suite 목록
        
        <aside>
        💡 Cipher Suite란?
        Cipher Suite는 클라이언트와 서버가 안전한 연결을 수립하기 위해 사용하는 암호화 알고리즘의 집합을 의미합니다. Cipher Suite는 여러 구성 요소를 포함하여, 데이터 암호화, 인증, 메시지 무결성 등을 담당하는 알고리즘을 정의합니다. 이 과정에서 클라이언트와 서버는 서로 지원 가능한 Cipher Suite를 비교하여 가장 적합한 것을 선택합니다.
        
        </aside>
        
    2. session id
    3. TLS Protocol version
    4. Client Random Byte (무작위 키)
4. Server는 Client로부터 Client Hello 패킷을 받고, Cipher Suite 목록에서 하나를 선택한다.
    1. 선택한 Cipher Suite
    2. TLS Protocol Version
    3. Server Random Byte (무작위 키)
5. Server의 TLS 인증서를 Client에 전달한다.
    
    ![image](https://github.com/user-attachments/assets/014de627-9635-417e-b821-f061a56b7fa1)

    
    이 때 TLS 인증서는 CA의 비밀키로 암호화된 상태이며, CA의 공개키는 누구에게나 공개되어 있다.
    
6. Client는 CA의 공개키를 이용해 TLS 인증서를 복호화한다.
    
    ![image](https://github.com/user-attachments/assets/02a3b137-c920-4c4d-8c20-3b3034531a7f)

    
    복호화에 성공하면 해당 인증서는 CA가 서명한 진짜 인증서임을 검증한 것으로 볼 수 있다.
    
7. 클라이언트는 랜덤으로 Pre-master key를 생성하고, TLS 인증서 복호화를 통해 얻어낸 서버의 공개 키로 암호화해 서버로 전송한다.
    
    ![image](https://github.com/user-attachments/assets/febae572-5a8f-4d7f-ad19-4a816f157a87)

    
8. 서버는 자신의 비밀키로 복호화해 Pre-master key를 얻어낸다.
    
    ![image](https://github.com/user-attachments/assets/870d5e21-f8ba-4f04-8090-6d9e06a1c25a)

9. 서버와 클라이언트는 주고받은 Client Random Byte, Server Random Byte, Pre-master key를 이용해 각자 독립적으로 master key를 만들어내고, 일련의 과정을 통해 이를 session key로 만들어낸다.
    
    ![image](https://github.com/user-attachments/assets/b853f7b1-fb61-46ad-bddb-0d922ae5eb3b)

    
    Client와 Server가 만든 세션 키는 동일하고, 이를 앞으로 데이터를 주고 받을 때 암호화할 대칭 키로 사용한다.
    

# 참고 자료

- https://blog.naver.com/patchwork_corp/222467994034?
- https://run-it.tistory.com/30
- [https://velog.io/@bambookim/HTTPS와-SSLTLS](https://velog.io/@bambookim/HTTPS%EC%99%80-SSLTLS)
