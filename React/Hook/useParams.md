## useParam이란?

- 리액트에서 제공하는 Hook으로, **동적으로 라우팅을 생성**하기 위해 사용한다.
    - 즉, 하드코딩을 막는 방법 중 하나!
- URL에 포함되어있는 **Key, Value 형식의 객체를 반환**해주는 역할을 한다.
    - Route 부분에서 Key를 지정하고, 해당하는 Key에 적합한 Value를 넣어 URL을 변경시키면 u**seParams를 통해 Key, Value 객체를 반환받아 확인**할 수 있다.
- 반환받은 Value를 통해 게시글을 불러오거나 검색 목록을 변경시키는 등 다양한 기능으로 확장시켜 사용할 수 있다.

## 사용 방법

1. 라우트 생성
    
    ```tsx
    import React from 'react';
    import { BrowserRouter, Routes, Route } from 'react-router-dom';
    
    import PortfolioPage from '../pages/portfolio/PortfolioPage';
    import IndexPage from '../pages/IndexPage';
    import PortfolioDetailPage from '../pages/portfolio/PortfolioDetailPage';
    
    function PageRoutes() {
      return (
        <div style={{
          marginLeft: '30px',
        }}>
        <BrowserRouter>
          <Routes>
            <Route path="/" Component={IndexPage}></Route>
            <Route path="/portfolio" Component={PortfolioPage}></Route>
            **<Route path="/portfolio/detail/:id" Component={PortfolioDetailPage}></Route>**
          </Routes>
        </BrowserRouter>
        </div>
      );
    }
    
    export default PageRoutes;
    ```
    
    - path 옵션에 :가 붙어있다.
    - :id는 다른 라우트의 일반주소와 달리 **변수로 취급**된다.
    - useParams의 URL은 Key, Value 형식으로 짝지어 들어가기 때문에 숫자나 문자를 넣어 주소를 이동시킬 수 있다.
2. 파라미터를 이용해 axios 요청보내기
    
    ```tsx
    import React, { useEffect, useState } from 'react';
    **import { useParams } from 'react-router-dom';**
    import axios from 'axios';
    import Slider from 'rc-slider';
    import 'rc-slider/assets/index.css';
    
    function PortfolioDetailPage() {
      const [portfolioResult, setPortfolioResult] = useState<PortfolioProps[] | undefined>();
      **const contentId = useParams();**
    
      const getData = async () => {
        try {
          const responseData = await axios({
            url: `http://localhost:3001/api/portfolio/${contentId.id}`,
            method: 'get',
            headers: {
              contentType: 'application/json',
            },
          });
    
          if (responseData.data.code == 200) {
            setPortfolioResult(responseData.data.data);
          } else {
            alert('서버에서 오류가 발생했습니다. 잠시 후 다시 시도해주세요.');
          }
        } catch (error: any) {
          console.error(error.response.data);
        }
      };
    
      useEffect(() => {
        getData();
        console.log(portfolioResult);
      }, []);
    
    // 코드 생략
    ```
    
    - useParams는 react가 아니라 react-router-dom에서 가져와야한다.
    - contentId를 그대로 사용하면 객체 타입이기 때문에 불가능하다.
        - .와 같은 체이닝을 통해 꺼내서 사용해야한다.

## 참고 자료

- [https://phsun102.tistory.com/m/92](https://phsun102.tistory.com/m/92)
- [https://jae04099.tistory.com/entry/React-React-Router와-useParams](https://jae04099.tistory.com/entry/React-React-Router%EC%99%80-useParams)
- [https://velog.io/@boyfromthewell/React-5.-상세페이지-만들기](https://velog.io/@boyfromthewell/React-5.-%EC%83%81%EC%84%B8%ED%8E%98%EC%9D%B4%EC%A7%80-%EB%A7%8C%EB%93%A4%EA%B8%B0)