## Passport 모듈

- 이름 그대로 서비스를 사용할 수 있게끔 해주는 여권 같은 역할을 하는 모듈
- 회원 가입과 로그인을 직접 구현할 수도 있지만, 세션과 쿠키 처리 등 복잡한 작업이 많다.
    - 따라서, 검증된 모듈인 Passport를 사용하면 좋다.
- **클라이언트가 서버에 요청할 자격이 있는지 인증할 때 passport 미들웨어를 사용한다!**
- 비밀번호 이외에 구글, 페이스북, 카카오톡 같은 기존의 **SNS 서비스 계정을 이용해 로그인하는데 많이 사용된다.**
- 정리하자면, **passport 모듈은 로그인 절차를 확실하게 하기 위해 사용하는 라이브러리이다.**

## passport.js 라이브러리

- strategy(전략)에 따른 요청으로 인증하기 위한 목적으로 사용

### strategy 종류 (로그인 인증 방식)

1. Local Strategy (passport-local) : 로컬 DB에서 로그인 인증 방식
2. Social Authentication (passport-kakao, passport-twitter 등) : 소셜 네트워크 로그인 인증 방식

### API 동작

- 사용자가 passport에 인증 요청 → passport는 인증 성공/실패 시 어떤 제어를 할 지 결정

## passport 처리 과정

### 초기 로그인 과정

<img width="698" alt="스크린샷 2023-03-27 오후 9 10 35" src="https://user-images.githubusercontent.com/96467030/227942887-c6427ca7-5b4f-41b1-ba1e-c4749028f4e0.png">
<img width="700" alt="스크린샷 2023-03-27 오후 9 11 01" src="https://user-images.githubusercontent.com/96467030/227942890-cd8ec718-0055-44e8-8717-6b3baee54729.png">
<img width="703" alt="스크린샷 2023-03-27 오후 9 13 25" src="https://user-images.githubusercontent.com/96467030/227942893-66a46439-3a62-45ed-b298-77c3e44bd89a.png">

1. 로그인 요청이 라우터로 들어온다.
2. 미들웨어를 거쳐 passport.authenticate() 호출
3. authenticate에서 passport/localStrategy.js 호출
4. 로그인 전략을 실행하고 done()을 호출하면, 다시 passport.authenticate() 라우터로 돌아가 다음 미들웨어 실행
5. done() 정보를 토대로 로그인 성공 시 사용자 정보 객체와 함께 req.login()을 자동으로 호출
6. req.login 메서드가 passport.serializeUser()호출 (passport/index.js)
7. req.session에 사용자 아이디 키 값만 저장 (메모리 최적화를 위해서)
8. passport.deserializeUser()로 바로 넘어가서 sql 조회 후 req.user 객체 등록 후, done() 을 반환하여 req.login 미들웨어로 다시 되돌아간다.
9. 미들웨어 처리 후, res.redirect(’/’) 을 응답하면, 세션 쿠키를 브라우저에 보내게 된다.
10. 로그인 완료 처리 (이제 세션 쿠키를 통해 통신하며 로그인됨 상태를 알 수 있다.)

### passport 로그인 이후 과정

<img width="521" alt="스크린샷 2023-03-27 오후 9 16 37" src="https://user-images.githubusercontent.com/96467030/227942896-3e082de2-01c3-4bcb-ae0a-928a04146c0a.png">

1. 모든 요청에 passport.session() 미들웨어가 passport.deserializeUser() 메서드를 매번 호출한다.
2. deserializeUser에서 req.session에 저장된 아이디로 데이터베이스에서 사용자 조회
3. 조회된 사용자 전체 정보를 req.user 객체에 저장
4. 이제부터 라우터에서 req.user를 공용적으로 사용 가능하게 된다.

### done() 함수 인자값에 따른 여러 동작

<img width="665" alt="스크린샷 2023-03-27 오후 9 18 11" src="https://user-images.githubusercontent.com/96467030/227942901-47015257-0798-4c42-ad36-1b5ac1b288cb.png">

## passport 구조 코드

```jsx
...
const passport = require('passport');

...
const passportConfig = require('./passport');

...
const authRouter = require('./routes/auth'); // 인증 라우터

const app = express();
passportConfig(); // 패스포트 설정

...
app.use(cookieParser(process.env.COOKIE_SECRET));
app.use(
   session({
      resave: false,
      saveUninitialized: false,
      secret: process.env.COOKIE_SECRET,
      cookie: {
         httpOnly: true,
         secure: false,
      },
   }),
);
//! express-session에 의존하므로 뒤에 위치해야 함
app.use(passport.initialize()); // 요청 객체에 passport 설정을 심음
app.use(passport.session()); // req.session 객체에 passport정보를 추가 저장
// passport.session()이 실행되면, 세션쿠키 정보를 바탕으로 해서 passport/index.js의 deserializeUser()가 실행하게 한다.

//* 라우터
app.use('/auth', authRouter);

...
```

- passport.initialize 미들웨어는 요청(req 객체)에 passport 설정을 심는다.
- passport.session 미들웨어는 req.session 객체에 passport 인증 완료 정보를 저장한다.
- **req.session 객체는 express-session에서 생성하는 것이므로, passport 미들웨어는 express-session 미들웨어보다 뒤에 연결해야 한다.**

```jsx
//* 사용자 미들웨어를 직접 구현

exports.isLoggedIn = (req, res, next) => {
   // isAuthenticated()로 검사해 로그인이 되어있으면
   if (req.isAuthenticated()) {
      next(); // 다음 미들웨어
   } else {
      res.status(403).send('로그인 필요');
   }
};

exports.isNotLoggedIn = (req, res, next) => {
   if (!req.isAuthenticated()) {
      next(); // 로그인 안되어있으면 다음 미들웨어
   } else {
      const message = encodeURIComponent('로그인한 상태입니다.');
      res.redirect(`/?error=${message}`);
   }
};
```

- 자신이 만든 간단한 사용자 미들웨어 함수 파일
- 로그인을 꼭 해야되는 페이지와 하지 않아도 되는 페이지를 구분하는 미들웨어 작성
- req.isAuthenticated 함수를 이용해 요청에 인증 여부 확인
- Passport는 req 객체에 isAuthenticated라는 메서드를 자동으로 만들어준다.
    - 로그인이 되어있으면 req.isAuthenticated()가 true, 아니라면 false
    - 해당 메서드를 통해 로그인 여부를 파악할 수 있다.

```jsx
const express = require('express');
const passport = require('passport');
const bcrypt = require('bcrypt');
const { isLoggedIn, isNotLoggedIn } = require('./middlewares'); // 내가 만든 사용자 미들웨어
const User = require('../models/user');

const router = express.Router();

//* 회원 가입
// 사용자 미들웨어 isNotLoggedIn을 통과해야 async (req, res, next) => 미들웨어 실행
router.post('/join', isNotLoggedIn, async (req, res, next) => {
   const { email, nick, password } = req.body; // 프론트에서 보낸 폼데이터를 받는다.

   try {
      // 기존에 이메일로 가입한 사람이 있나 검사 (중복 가입 방지)
      const exUser = await User.findOne({ where: { email } });
      if (exUser) {
         return res.redirect('/join?error=exist'); // 에러페이지로 바로 리다이렉트
      }

      // 정상적인 회원가입 절차면 해시화
      const hash = await bcrypt.hash(password, 12);

      // DB에 해당 회원정보 생성
      await User.create({
         email,
         nick,
         password: hash, // 비밀번호에 해시문자를 넣어준다.
      });

      return res.redirect('/');
   } catch (error) {
      console.error(error);
      return next(error);
   }
});

/* **************************************************************************************** */

//* 로그인 요청
// 사용자 미들웨어 isNotLoggedIn 통과해야 async (req, res, next) => 미들웨어 실행
router.post('/login', isNotLoggedIn, (req, res, next) => {
   //? local로 실행이 되면 localstrategy.js를 찾아 실행한다.
   passport.authenticate('local', (authError, user, info) => {
      //? (authError, user, info) => 이 콜백 미들웨어는 localstrategy에서 done()이 호출되면 실행된다.
      //? localstrategy에 done()함수에 로직 처리에 따라 1,2,3번째 인자에 넣는 순서가 달랐는데 그 이유가 바로 이것이다.

      // done(err)가 처리된 경우
      if (authError) {
         console.error(authError);
         return next(authError); // 에러처리 미들웨어로 보낸다.
      }
      // done(null, false, { message: '비밀번호가 일치하지 않습니다.' }) 가 처리된 경우
      if (!user) {
         // done()의 3번째 인자 { message: '비밀번호가 일치하지 않습니다.' }가 실행
         return res.redirect(`/?loginError=${info.message}`);
      }

      //? done(null, exUser)가 처리된경우, 즉 로그인이 성공(user가 false가 아닌 경우), passport/index.js로 가서 실행시킨다.
      return req.login(user, loginError => {
         //? loginError => 미들웨어는 passport/index.js의 passport.deserializeUser((id, done) => 가 done()이 되면 실행하게 된다.
         // 만일 done(err) 가 됬다면,
         if (loginError) {
            console.error(loginError);
            return next(loginError);
         }
         // done(null, user)로 로직이 성공적이라면, 세션에 사용자 정보를 저장해놔서 로그인 상태가 된다.
         return res.redirect('/');
      });
   })(req, res, next); //! 미들웨어 내의 미들웨어에는 콜백을 실행시키기위해 (req, res, next)를 붙인다.
});

/* **************************************************************************************** */

//* 로그아웃 (isLoggedIn 상태일 경우)
router.get('/logout', isLoggedIn, (req, res) => {
   // req.user (사용자 정보가 안에 들어있다. 당연히 로그인되어있으니 로그아웃하려는 거니까)
   req.logout();
   req.session.destroy(); // 로그인인증 수단으로 사용한 세션쿠키를 지우고 파괴한다. 세션쿠키가 없다는 말은 즉 로그아웃 인 말.
   res.redirect('/');
});

module.exports = router;
```

- 회원가입, 로그인, 로그아웃 처리를 담당하는 라우터
- 로그인 인증에 관해 passport 폴더로 인증 전략을 요청한다.
- `/logout` 경로로 접근했을 때 로그인이 되어있지 않다면 접근할 수 없도록 해야하기 때문에 isLoggedIn을 미들웨어로 넣어주었다.
- `/join` 경로로 접근했을 때 로그아웃이 되어있지 않다면, (즉, 현재 로그인한 상태라면) 접근할 수 없도록 해야하기 때문에 isNotLoggedIn을 미들웨어로 넣어주었다.

```jsx
const passport = require('passport');
const local = require('./localStrategy'); // 로컬서버로 로그인할때
//const kakao = require('./kakaoStrategy'); // 카카오서버로 로그인할때
const User = require('../models/user');

module.exports = () => {
   /*
    * ㅇ 직렬화(Serialization) : 객체를 직렬화하여 전송 가능한 형태로 만드는 것.
    * ㅇ 역직렬화(Deserialization) : 직렬화된 파일 등을 역으로 직렬화하여 다시 객체의 형태로 만드는 것.
    */

   //? req.login(user, ...) 가 실행되면, serializeUser가 실행된다.
   //? 즉 로그인 과정을 할때만 실행
   passport.serializeUser((user, done) => {
      // req.login(user, ...)의 user가 일로 와서 값을 이용할수 있는 것이다.
      done(null, user.id);
      // req.session객체에 어떤 데이터를 저장할 지 선택.
      // user.id만을 세션객체에 넣음. 사용자의 온갖 정보를 모두 들고있으면, 
      // 서버 자원낭비기 때문에 사용자 아이디만 저장 그리고 데이터를 deserializeUser애 전달함
      // 세션에는 { id: 3, 'connect.sid' : s%23842309482 } 가 저장됨
   });

   //? deserializeUser는 serializeUser()가 done하거나 passport.session()이 실행되면 실행된다.
   //? 즉, 서버 요청이 올때마다 항상 실행하여 로그인 유저 정보를 불러와 이용한다.
   passport.deserializeUser((id, done) => {
      // req.session에 저장된 사용자 아이디를 바탕으로 DB 조회로 사용자 정보를 얻어낸 후 req.user에 저장. 
      // 즉, id를 sql로 조회해서 전체 정보를 가져오는 복구 로직이다.
      User.findOne({ where: { id } })
         .then(user => done(null, user)) //? done()이 되면 이제 다시 req.login(user, ...) 쪽으로 되돌아가 다음 미들웨어를 실행하게 된다.
         .catch(err => done(err));
   });

   //^ 위의 이러한 일련의 과정은, 그냥 처음부터 user객체를 통째로 주면 될껄 뭘 직렬화/역직렬화를 하는 이유는
   //^ 세션 메모리가 한정되어있기때문에 효율적으로 하기위해, user.id값 하나만으로 받아와서, 
   //^ 이를 deserialize 복구해서 사용하는 식으로 하기 위해서다.

   /* ---------------------------------------------------------------------- */

   local();
   //kakao();
};
```

- 인증 전략을 등록하고, 데이터를 저장하거나 불러올 때 이용되는 파일

`passport.serializeUser`

- strategy에서 로그인 성공 시 호출하는 done(null, user) 함수의 두 번째 인자 user를 전달받아 세션(req.session.passport.user)에 저장
- 보통 세션의 무게를 줄이기 위해 user의 id만 세션에 저장

`passport.deserializeUser`

- 서버로 들어오는 요청마다 세션 정보를 실제 DB와 비교
- 해당 유저 정보가 있으면 done을 통해 req.user에 사용자 전체 정보를 저장
    - 다른 미들웨어에서 req.user를 공통적으로 사용 가능
- serializeUser에서 done으로 넘겨주는 user가 deserializeUser의 첫번째 매개변수로 전달되기 때문에 둘의 타입은 항상 일치 필요

```jsx
const passport = require('passport');
const LocalStrategy = require('passport-local').Strategy;
const bcrypt = require('bcrypt');

const User = require('../models/user');

module.exports = () => {
   //? auth 라우터에서 /login 요청이 오면 local설정대로 이쪽이 실행되게 된다.
   passport.use(
      new LocalStrategy(
         {
            //* req.body 객체인자 하고 키값이 일치해야 한다.
            usernameField: 'email', // req.body.email
            passwordField: 'password', // req.body.password
            /*
            session: true, // 세션에 저장 여부
            passReqToCallback: false, 
            	express의 req 객체에 접근 가능 여부. true일 때, 뒤의 callback 함수에서 req 인자가 더 붙음. 
           		async (req, email, password, done) => { } 가 됨
            */
         },
         //* 콜백함수의  email과 password는 위에서 설정한 필드이다. 위에서 객체가 전송되면 콜백이 실행된다.
         async (email, password, done) => {
            try {
               // 가입된 회원인지 아닌지 확인
               const exUser = await User.findOne({ where: { email } });
               // 만일 가입된 회원이면
               if (exUser) {
                  // 해시비번을 비교
                  const result = await bcrypt.compare(password, exUser.password);
                  if (result) {
                     done(null, exUser); //? 성공이면 done()의 2번째 인수에 선언
                  } else {
                     done(null, false, { message: '비밀번호가 일치하지 않습니다.' }); //? 실패면 done()의 2번째 인수는 false로 주고 3번째 인수에 선언
                  }
                  //? done()을 호출하면, /login 요청온 auth 라우터로 다시 돌아가서 미들웨어 콜백을 실행하게 된다.
               }
               // DB에 해당 이메일이 없다면, 회원 가입 한적이 없다.
               else {
                  done(null, false, { message: '가입되지 않은 회원입니다.' });
               }
            } catch (error) {
               console.error(error);
               done(error); //? done()의 첫번째 함수는 err용. 특별한것 없는 평소에는 null로 처리.
            }
         },
      ),
   );
};
```

- 로컬 인증 전략 절차 코드가 있는 파일이며, 라우터에서 요청이 들어오면 실행된다.

`LocalStrategy`

- local 로그인을 위한 전략
- usernameField / passwordField : 프론트 단의 폼태그에서 요청된 값(req.body.*)이 오게 된다.
- session : 세션 저장 여부
- passReqToCallback : express의 req 객체에 접근 가능 여부
    - true 일 때, 뒤의 callback 함수에서 req 인자가 더 붙는다 (req, email, password, done) ⇒ {}
- 첫번째 인자에서 id, pw가 전송되면, 두번째 인자 콜백 함수 실행
    - 실제 전략은 async 함수에서 실행
- DB에서 비교하여 done 함수를 이용해 user 객체 전송 또는 에러 리턴
- done(에러, 성공, 실패값)
    - 첫 번째 인자: DB 조회 시 발생하는 서버 에러. 무조건 실패하는 경우에만 사용
    - 두 번째 인자: 성공했을 때 return 할 값
    - 세 번쨰 인자: 사용자가 임의로 실패를 만들고 싶을 때 사용
        - ex. 위에서 비밀번호가 틀렸다는 에러

## 참고 자료

- [https://inpa.tistory.com/entry/NODE-📚-Passport-모듈-그림으로-처리과정-💯-이해하자](https://inpa.tistory.com/entry/NODE-%F0%9F%93%9A-Passport-%EB%AA%A8%EB%93%88-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%B2%98%EB%A6%AC%EA%B3%BC%EC%A0%95-%F0%9F%92%AF-%EC%9D%B4%ED%95%B4%ED%95%98%EC%9E%90)
- [https://chanyeong.com/blog/post/28](https://chanyeong.com/blog/post/28)