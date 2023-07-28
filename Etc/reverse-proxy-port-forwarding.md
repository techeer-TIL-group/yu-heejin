- 포트 포워딩은 nginx의 가장 기본적인 기능이다.
- 한 웹서버 안에는 한 가지의 서버만 존재하지는 않으며, 이러한 서버들은 각각 다른 포트들을 가지고 있다.

## 포트 포워딩의 개념

- 서버의 들어온 요청을 다시 내부의 특정 포트로 보내주는 것이다. (특정 포트 == 특정 서버)
- 웹서버는 요청에 대한 적절한 로컬 포트를 정하기 위한 약속이 필요한데, 다음 두 가지로 정의할 수 있다.
    1. URL 머리(도메인 호스트)로 구분하기 (ex. https://`hello`.host.com)
    2. URL 꼬리(location)로 구분하기 (ex. https://host.com/`hello`)

## 사용 방법

### 도메인 호스팅으로 구분하기

- 입력 URI의 ‘머리’로 구별한다.
- 웹서버에 도메인을 구입해서 적용하면, 그 도메인을 서브 도메인으로 여러 호스팅이 가능하다.
- 예를 들어, 192.168.x.x 아이피 주소를 가진 웹 서버가 있고, 해당 IP에 대해 test.com이라는 도메인과 api.test.com이라는 레코드(호스트)를 추가했다고 하자.
    - 이 때 두 주소로 접속하면 모두 웹서버의 default로 설정되어 있는 서버로 포팅된다.
    - 이제 호스트에 따라 새로운 서버로 리다이렉팅 할 수 있다.
        
        ```
        server {
            listen 80;
            server_name api.test.com	// 이 웹서버에 어떤 방식으로 들어왔는지 확인
            
            location / {
            	proxy_pass http://127.0.0.1:3003;
            }
        }
        ```
        

### URL로 구분하기

```
server {
    listen 80;
    
    location /other/ {
    	proxy_pass http://127.0.0.1:3003/;
    }
}
```

## 참고 자료

- https://zionh.tistory.com/20