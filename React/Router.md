## Router 사용 시 주의사항

- Router를 사용할 경우, 컴포넌트가 다르면 인식이 안되는 경우가 발생한다.
- 다음과 같이 사용한다.
    1. index.tsx에 BrowserRouter추가
        
        ```tsx
        const root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
        root.render(
          <BrowserRouter>
            <App />
          </BrowserRouter>,
        );
        ```
        
        - React.StrictMode를 삭제하고 다음과 같이 추가한다.
    2. 위와 같이 추가하면 Routes 파일을 BrowserRouter로 감쌀 필요가 없다.
        
        ```tsx
        function PageRoutes() {
          return (
            <BodyBackground>
              <Routes>
                <Route path="/" Component={IndexPage} />
                <Route path="/portfolio" Component={PortfolioPage} />
                <Route path="/portfolio/detail/:id" Component={PortfolioDetailPage} />
              </Routes>
            </BodyBackground>
          );
        }
        ```
        
    3. Link, NavLink 추가
        
        ```tsx
        return (
              <MenuBackgroundBox>
                <div>
                  {/* a 태그 대신 useNav, Link, navLink 등 사용 권장*/}
                  <Link to="/">
                    <MenuLogo src={LogoImage}/>
                  </Link>
                    <MenuUl>
                      <MenuLi>대시보드</MenuLi>
                      <MenuLi>정산관리</MenuLi>
                      <MenuLi>
                        <NavLink to="/portfolio" style={({isActive}) => {
                          return isActive ? activeStyle : deactiveStyle;
                        }}>포트폴리오</NavLink>
                      </MenuLi>
                      <MenuLi>E-BOOK제작</MenuLi>
                      <MenuLi>고객센터</MenuLi>
                    </MenuUl>
                  <UserProfileImage src={BasicProfile}/> ella
              </div>
            </MenuBackgroundBox>
          );
        ```
        
- [https://aridom.tistory.com/35](https://aridom.tistory.com/35)