## 쿠키란?

- 쿠키는 **브라우저에 데이터를 저장하기 위한 수단 중 하나**이다.
- 브라우저에서 서버로 요청을 전송할 때 그 요청에 대한 응답에 `Set-Cookie` 헤더가 포함되어 있는 경우, **브라우저는 `Set-Cookie`에 있는 데이터를 저장한다.**
    - 이 저장된 데이터를 쿠키라고 한다.
- Set-Cookie 헤더가 포함된 경우, normal이라는 이름의 쿠키에 yes라는 값이 저장된다.
    
    ![Untitled](https://seob.dev/static/97337a4841fcf5fa74cb24976247cb0d/d68e4/set-cookie-header.png)
    
- 저장된 쿠키는 다음에 다시 브라우저에서 서버로 요청을 보낼 때 Cookie라는 헤더에 같이 전송된다.
    - 서버는 이 헤더를 읽어서 유저를 식별하는 등 필요한 구현을 할 수 있다.
- 쿠키는 이와 같은 과정으로 동작하기 때문에 **주로 서버에서 사용자를 식별하기 위한 수단으로 사용되어 왔다.**

## 쿠키에 대한 도메인 설정

- 쿠키가 유효한 사이트를 명시하기 위해 쿠키에 도메인을 설정할 수도 있다.
    
    ![Untitled](https://seob.dev/static/68ff614820ca22386dbfee5239c760ea/8f77f/cookie-header.png)
    
- 도메인이 설정된 쿠키는 **해당 도메인에서만 유효한 쿠키가 된다.**
    - normal 쿠키는 localhost를 대상으로 쿠키가 설정되었기 떄문에 localhost를 대상으로 한 요청에만 normal 쿠키가 전송된다.
- 도메인을 명시하지 않으면 **기본값으로 쿠키를 보낸 서버의 도메인으로 설정된다.**

## 퍼스트 파티 쿠키와 서드 파티 쿠키

- 만약 page.com이 example.com이 제공하는 이미지인 [example.com/image.png를](http://example.com/image.png를) 사용하고 있다고 가정하자.
    - 이 경우 사용자는 page.com에 접속해 있지만, 브라우저는 [example.com/image.png로](http://example.com/image.png로) 요청을 보낼 것이다.
- 이 때 사용자가 example.com에 대한 쿠키를 가지고 있다면 해당 쿠키가 example.com을 운영하는 서버로 같이 전송된다.
    - 이 때 전송되는 쿠키를 **서드 파티 쿠키(Third-party cookies)라고 부른다.**
    - 즉, 서드 파티 쿠키는 **사용자가 접속한 페이지와 다른 도메인으로 전송하는 쿠키를 말한다.**
    - Referer 헤더와 쿠키에 설정된 도메인이 다른 쿠키라고 할 수 있다.
- 퍼스트 파티 쿠키(First-party cookies)는 반대로 **사용자가 접속한 페이지와 같은 도메인으로 전송되는 쿠키를 말한다.**

## 쿠키와 CSRF 문제

- 쿠키에 별도로 설정을 가하지 않는다면, 크롬을 제외한 브라우저들은 모든 HTTP요청에 대해 쿠키를 전송하게 된다.
- CSRF는 이러한 문제를 노린 공격이며, 다음과 같은 방식이다.
    1. 공격 대상 사이트는 쿠키로 사용자 인증을 수행한다.
    2. 피해자는 공격 대상 사이트에 이미 로그인 되어 있기 때문에 브라우저에 쿠키가 있는 상태이다.
    3. 공격자는 피해자에게 그럴듯한 사이트 링크를 전송하고 누르게한다. (공격 대상 사이트와 다른 도메인)
    4. 링크를 누르면 html문서가 열리고, 이 문서는 공격 대상 사이트에 HTTP 요청을 보낸다.
    5. 이 요청에는 쿠키가 포함(서드 파티 쿠키)되어 있기 떄문에 공격자가 유도한 동작을 실행할 수 있다.

## SameSite 쿠키

- SameSite 쿠키는 앞서 언급한 서드 파티 쿠키의 보안적 문제를 해결하기 위해 만들어진 기술이다.
- **크로스 사이트(Cross-site)로 전송하는 요청의 경우 쿠키의 전송에 제한을 두도록 한다.**

### SameSite 속성값

- None
    - **동일 사이트와 크로스 사이트 모두에 쿠키 전송이 가능**하다.
    - 이로인해 CSRF의 공격에 취약한 등 보안에 취약점을 가지고 있다
    - 크롬 80버전부터 **SameSite를 `None`으로 설정할 경우 쿠키에 암호화된 HTTPS 연결이 필요함을 나타내는 `Secure` 속성을 함께 넣어주어야 한다.**
    - 즉, `SameSite=None; Secure` 이렇게 작성해야 하며 URL은 https로 처리되어야 한다.
- Strict
    - `Strict`로 설정하면 **해당 값으로 설정된 쿠키는 도메인 자체에서 시작된 요청에서만 전송된다.**
    - 즉, SameSite에서만 쿠키의 전송을 허용한다.
    - 가장 제한적인 방식으로 보안에는 완벽하지만 편의성이 떨어진다.
- Lax
    - `Lax` 모드는 기본적으로는 CrossSite 쿠키값 전송을 차단하는 Strict 모드와 동일하지만 **몇가지 예외사항을 두어 CrossSite임에도 일부 요청 방식으로는 쿠키를 보낼 수도 있다.**
    - `Strict` 방식에서 예외사항을 추가한 방식이다.

## 구글 Chrome SameSite 이슈

- 크롬은 SameSite를 명시하지 않은 쿠키는 `None`으로 동작했는데, 2020년부터 SameSite의 기본값이 `Lax`로 변경되었다.
- sameSite 속성의 기본 값이 `None`에서 `Lax`로 변경되면 각종 문제가 일어날 수 있다.
    - 사용자가 사이트를 이용하다가 갑자기 쿠키가 날아가버릴 수 있기 때문!
- 대표적인 예시가 로그인 정보이다.
    - 로그인 후 쿠키를 사용해 유저의 신원을 확인하여 페이지를 전환해도 재인증을 하지 않아도 되도록 구현하고 있다.
    - 하지만 SameSite 이슈로 인해 쿠키값을 찾을 수 없다면 로그인이 되어 있어야 확인할 수 있는 페이지로 전환했을 때 다시 로그인 페이지로 돌아가게 된다.
- 예를 들어 쇼핑 사이트에서 사용자가 구매하고자 하는 물건을 장바구니에 담에 PG(온라인 결제 대행사)사를 통해 결제를 했다고 가정하자.
    - 이 때 쇼**핑몰 사이트와 PG사의 도메인이 서로 다르기 때문에 세션 아이디를 통해 기존의 쿠키값을 찾지 못해 로그인 쿠기값이 유실되는 경우가 발생**할 수 있다.

### SameSite = Lax로 변경하는 이유?

- SameSite의 기본 설정값을 `None`에서 `Lax`로 변경하는 이유는 기본적으로 CSRF(Cross Site Request Forgery) 공격을 막기 위함이다.
    - **CSRF란 사이트 간 요청 위조라는 뜻의 웹사이트 취약점 공격 방식** 중 하나로, 사용자의 의지와는 관련 없이 **공격자가 의도한 행위를 웹사이트에 요청하는 것을 의미한다.**
- 같은 웹 사이트일 땐 당연히 전송되고, 이 외에에는 Top Level Navigation(웹 페이지 이동)과 ‘안전한’ HTTP 메서드 요청의 경우 전송된다.
    - Top Level Navigation에는 유저가 링크`<a>`를 클릭하거나, `window.location.replace` 등으로 인해 자동으로 이뤄지는 이동, 302 리다이렉트를 이용한 이동이 포함된다.
    - 하지만 `<iframe>`이나 `<img>`를 문서에 삽입함으로써 발생하는 http 요청은 ‘Navigation’이라고 할 수 없으니 `Lax` 쿠키가 전송되지 않고, `<iframe>`안에서 페이지를 이동하는 경우는 top level이라고 할 수 없으니 `Lax` 쿠키는 전송되지 않는다.
    - 안전하지 않은 `POST`, `DELETE`같은 요청의 경우, `Lax` 쿠키는 전송되지 않는다.
        - `GET`처럼 서버의 상태를 바꾸지 않을거라고 기대되는 요청에는 전송된다.
- 위 예시는 서드 파티 쿠키에 한하는 것이며, 퍼스트 파티 쿠키는 `Lax`, `Strict`여도 전송된다.

### Lax 방식에서 쿠키값을 전송할 수 있는 경우

```tsx
//a href 링크
<a href=""></a>

//prerender 링크
<link rel="prerender" href=".."/>

//HTTP GET 메소드
<form method="GET" action="...">
```

## ****A cookie associated ... `SameSite` attribute ( Node.js example for SameSite=None; Secure )****

- 크롬에서 도메인이 다른 경우 쿠키가 전송되지 않는 문제가 발생한다.
- 옵션을 다음처럼 설정한다.
    
    ```tsx
    // Set a same-site cookie for first-party contexts
    response.cookie('cookie1', 'value1', { sameSite: 'lax' });
    // Set a cross-site cookie for third-party contexts
    response.cookie('cookie2', 'value2', { sameSite: 'none', secure: true });
    
    cookies.setCookie = (res, token) => {
      res.cookie("access_token", token, {
        sameSite:'none',
        secure: true, // https, ssl 모드에서만
        maxAge: 1000*60*60*24*1, // 1D
        httpOnly: true, // javascript 로 cookie에 접근하지 못하게 한다.
      });
    }
    ```
    

## SameSite 이슈가 발생했을 때 대처 방법

### SameSite Lax → None

- 원래 크롬 80버전 이전에는 SameSite의 Default 값이 none이었다.
- 그러나 Lax로 바뀌면서 문제가 되기 때문에 다시 None으로 바꾸면 문제가 해결된다.

### 리다이렉트 페이지 만들기

- SameSite로 인해 쿠키 값이 누실되는 문제는 다른 도메인을 갔다가 돌아오는 가장 처음 페이지에서만 발생하는 문제일 수 있다.
- 처음에만 쿠키 값을 찾지 못하고 두번째 페이지로 전환될때(같은 도메인에서의 전환)는 정상적으로 쿠키를 찾고 있다면 다른 도메인으로 갔다오는 페이지를 리다이렉트 페이지로 만들어서 문제를 해결하는 방법도 고려해볼 수 있다.

## 참고 자료

- https://coding-factory.tistory.com/843
- https://serpiko.tistory.com/845
- [https://seob.dev/posts/브라우저-쿠키와-SameSite-속성/](https://seob.dev/posts/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EC%BF%A0%ED%82%A4%EC%99%80-SameSite-%EC%86%8D%EC%84%B1/)
- [https://www.inflearn.com/questions/592226/쿠키가-전송은-되는데-브라우저-내부에-저장되지-않습니다](https://www.inflearn.com/questions/592226/%EC%BF%A0%ED%82%A4%EA%B0%80-%EC%A0%84%EC%86%A1%EC%9D%80-%EB%90%98%EB%8A%94%EB%8D%B0-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%82%B4%EB%B6%80%EC%97%90-%EC%A0%80%EC%9E%A5%EB%90%98%EC%A7%80-%EC%95%8A%EC%8A%B5%EB%8B%88%EB%8B%A4)
- https://ifuwanna.tistory.com/223