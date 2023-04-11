## Static File

- 정적 파일이란 직접 값에 변화를 주지 않는 이상 변하지 않는 파일을 의미한다.
- 예를 들어 image, css, js 파일 등을 의미한다.

## 정적 파일 제공하기 - static 미들웨어

```jsx
app.use(express.static('public'));
```

- express 변수에는 `static`이라는 메서드가 포함되어 있다.
    - `static` 미들웨어는 express에서 제공하는 기본 미들웨어이며, express 객체 안에서 꺼내 바로 사용할 수 있다.
- 해당 메서드를 미들웨어로서 로드해준다.
- static의 인자로 전달되는 public은 디렉터리의 이름이다.
- 따라서 **public 이라는 디렉터리 밑에 있는 데이터들은 웹 브라우저의 요청에 따라 서비스를 제공해줄 수 있다.**
- 예를 들어, 사용자가 `localhost:3000/images/cats.jpg`로 접근한다면, 해당 파일을 `public/images/cat.jpg`에 존재하는지 검색하게 된다.
- 위와 같은 방법은 **public에 저장된 파일만을 제공한다는 점에서도 보안적인 이점**이 있다.
- 두 개의 폴더를 허용하고 싶은 경우 다음과 같이 미들웨어를 두 번 사용한다.
    
    ```jsx
    app.use(express.static('public'));
    app.use(express.static('files'));
    ```
    
- 때로는 가상 경로를 지정해주어야 할 때도 있다.
    
    ```jsx
    app.use('/static', express.static('public'));
    ```
    
    - 가상 경로를 사용한 경우 사용자가 public 디렉터리에 있는 파일들에 접근하려면 다음과 같이 접근해야 한다.
        
        ```jsx
        http://localhost:3000/static/images/kitten.jpg
        http://localhost:3000/static/css/style.css
        http://localhost:3000/static/js/app.js
        http://localhost:3000/static/images/bg.png
        http://localhost:3000/static/hello.html
        ```
        
    - 실제 서버의 폴더 경로에는 public이 들어 있더라도 요청 주소에는 public이 들어있지 않다.
    - 따라서 서버의 폴더 경로와 요청 경로가 다르므로 외부인이 서버의 구조를 쉽게 파악할 수 없으며, 이는 보안에 큰 도움이 왼다.
- express.static 메서드는 node 프로세스가 실행되는 디렉터리에 상대적이다. 따라서 절대 경로를 사용하는 것이 안전할 수 있다.
    
    ```jsx
    app.use('/static', express.static(__dirname + '/public'));
    ```
    
- 정적 파일들을 알아서 제공해주기 때문에 일일이 `fs.readFile`로 파일을 직접 읽어서 전송할 필요가 없다.
    - 만약 요청 경로에 해당하는 파일이 없으면 알아서 내부적으로 `next`를 호출한다.
    - 하지만 파일을 발견했다면 다음 미들웨어는 실행되지 않는다.
        - 응답으로 파일을 보내고 next를 호출하지 않기 때문
        
        ```jsx
        // 만일 public폴더안에 index.png라는 사진 파일이 있고, 
        // 사용자가 localhost:3000/index.png 요청을 한다면
        
        app.use(morgan('dev')); // get 200 ~ 요청 로그가 찍힌다.
        app.use('/', express.static(path.join(__dirname, 'public'))); // static경로로서 png파일을 제공하고 끝난다. 즉, next()를 안해버린다.
        app.use(express.json()); // 미들웨어 실행이 안된다.
        app.use(express.urlencoded({ extended: false })); // 미들웨어 실행이 안된다.
        app.use(cookieParser(process.env.COOKIE_SECRET)); // 미들웨어 실행이 안된다.
        ```
        
        ```jsx
        // 만일 public폴더안에 about이라는 경로가 없고, 
        // 사용자가 localhost:3000/about 요청을 한다면
        
        app.use(morgan('dev')); // get 200 ~ 요청 로그가 찍힌다.
        app.use('/', express.static(path.join(__dirname, 'public'))); // 경로에 about이 없으니 next()를 한다.
        app.use(express.json()); // next()를 한다.
        app.use(express.urlencoded({ extended: false })); // next()를 한다.
        app.use(cookieParser(process.env.COOKIE_SECRET)); // next()를 한다.
        
        app.get('/about', (req, res) => {
        	// ... 실행 된다.
        }
        ```
        
- express.static 같은 미들웨어는 정적 파일을 제공할 때 next 대신 res.sendFile 메서드로 응답을 보낸다.
    - 따라서 정적 파일을 제공하는 경우 그 뒤의 미들웨어들은 실행이 안된다.

## 참고 자료

- [https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=pjok1122&logNo=221545195520](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=pjok1122&logNo=221545195520)
- [https://inpa.tistory.com/entry/EXPRESS-📚-static-미들웨어](https://inpa.tistory.com/entry/EXPRESS-%F0%9F%93%9A-static-%EB%AF%B8%EB%93%A4%EC%9B%A8%EC%96%B4)