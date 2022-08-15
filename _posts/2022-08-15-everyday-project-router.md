---
layout: single
title:  "[에브리데이] React Router 사용"

categories:
  - Team Project
tags:
  - router
  - react
  - v6

published: true
---

<br/>
안녕하세요.
<br/>오늘은 [에브리데이] 프로젝트 관련하여 라우터를 어떻게 사용하였는지 포스팅해보려고 합니다.
<br/>URL에 따라서 그에 상응하는 화면을 전송해주는 것을 Routing이라 합니다. 쉽게 말해 사용자가 요청한 URL에 따라 해당 URL에 맞는 페이지를 보여주는 것입니다. 
<br/><br/>라우팅 라이브러리 중
가장 많이 사용하는 것이 리액트 라우터(React Router)이고 라우터는 리액트에서 비교적 쉽게 라우팅이 가능하도록 도와줍니다.
이 라우터를 저는 프로젝트에서 어떻게 사용하였는지 보여드리겠습니다.
<br/><br/><br/><br/>
# index.js
```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { BrowserRouter } from "react-router-dom";

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>  
      <BrowserRouter>
        <App />
      </BrowserRouter>
  </React.StrictMode>
);
reportWebVitals();
```
<br/>
# 코드
먼저 프로젝트에 리액트 라우터를 적용하기 위해 src/index.js 파일에 react-router-dom에 내장되어 있는 BrowserRouter라는 컴포넌트를 사용하여 감싸줍니다.
<br/>이 컴포넌트는 웹 애플리케이션에 HTML5의 History API를 사용하여 페이지를 새로 불러오지 않고도 주소를 변경하고 현재 주소의 경로에 관련된 정보를 리액트 컴포넌트에서 사용할 수 있도록 하는 정말 유용한 컴포넌트입니다.
<br/><br/>
# App.jsx
<br/>
```javascript
import React, { useState } from 'react';
import './App.css';
import AfterLoginContainer from './container/AfterLoginContainer';
import BeforeLoginContainer from './container/BeforeLoginContainer';
import { SESSION_TOKEN_KEY } from './component/Axios/Axios';

function App() {

  function isValidLoginToken() {
    const token = localStorage.getItem(SESSION_TOKEN_KEY);
    if (token) {
      return true; //token jwt 에 적합한 토큰인지 체크하는 로직도 필요
    }
    return false;
  }

  const [isLogin, setIsLogin] = useState(isValidLoginToken());

  function loginCallBack(login) {
    setIsLogin(login);
  }
  
  return (
    <div className='stop-dragging'>
      {
        //로그인 후
        isLogin &&
        <AfterLoginContainer loginCallBack={loginCallBack} />
      }
      {
        //로그인 전
        !isLogin &&
        <BeforeLoginContainer loginCallBack={loginCallBack} />
      }
    </div>
  );
}

export default App;
```
<br/>
# 코드
바로 App.js에 다음과 같이 Route 설정을 해도 되지만, 저는 점점 프로젝트가 진행되면서 코드 리팩토링을 하였기 때문에 로그인 전/후에 따라 AfterLoginContainer 컴포넌트와 BfterLoginContainer를 먼저 만들어서 넣었고 각 컴포넌트 안엔 다시 AfterLoginRouter 컴포넌트와 BoforLoginRouter를 만들어 넣어주었습니다.
<br/>
```javascript
<Routes>
  <Route path="url path" element={보여줄 컴포넌트명} />
  <Route path="/example" element={<Example />} />
</Routes>
```
<br/><br/>

# AfterLoginContainer.jsx
```javascript
import React from 'react';
import { Grid } from '@material-ui/core';
import AfterLoginRouter from './AfterLoginRouter';
import NavBar from './NavBar';
import LeftBar from './LeftBar';
import Footer from './Footer';

function AfterLoginContainer(props) {
    return (
        <div>
            <NavBar loginCallBack={props.loginCallBack} />
            <Grid container>
                    <Grid item sm={2} xs={2}>
                        <LeftBar />
                    </Grid>
                <Grid item sm={8} xs={10} style={{ margin: '8rem auto auto 100px' }}>
                    <AfterLoginRouter />
                </Grid>
            </Grid>
            <Footer />
        </div>
    )
}

export default AfterLoginContainer
```
<br/>
# 코드
AfterLoginContainer.jsx 에서 AterLoginRouter 컴포넌트를 넣어주었는데요. 이 때, Grid 컴포넌트를 사용하여 NavBar / LeftBar / AfterLoginRouter 의 배치를 나누었습니다. 
<br/>그 이유는 아래의 이미지에서처럼 2번 안의 메뉴를 누를때마다 3번의 영역만 라우트영역으로 정하여 해당 영역만 contents가 바뀔 수 있게 하기 위해서입니다.

# 라우트가 적용된 모습
<img src="https://user-images.githubusercontent.com/84834172/184620061-773792b3-d330-47ff-8e62-2cc485890406.png" width=""300px/> 
<img src="https://user-images.githubusercontent.com/84834172/184621817-02a6d2d4-48cf-4ce6-ad21-cd84018f7a74.png" width=""300px/>


# AfterLoginRouter.jsx
```javascript
import React from 'react';
import { Routes, Route } from 'react-router-dom';

import MainBoard from '../pages/MainBoard';
import NoticeBoard from '../pages/Notice/NoticeBoard';
import HotBoardList from '../pages/HotBoard/HotBoardList';
import InformationBoardList from '../pages/InfoBoard/InfoBoardList';
import FreeBoardList from '../pages/FreeBoard/FreeBoardList';
import ClubBoardList from '../pages/ClubBoard/ClubBoardList';

import BoardDetail from '../component/Table/BoardDetail';
import NoticeBoardDetail from '../component/Table/NoticeBoardDetail';
import BoardListAboutMe from '../component/Table/BoardListAboutMe';
import SearchBoardList from '../component/Table/SearchBoardList';

import UseTerms from '../pages/UseTerms';
import PrivacyPolicy from '../pages/PrivacyPolicy';
import CommunityRule from '../pages/CommunityRule';

import Error from "../component/Error";

function AfterLoginRouter() {
    return (
        <div>
            <Routes>
                <Route path="/" element={<MainBoard />} />

                <Route path="/noticeboard" element={<NoticeBoard />} />
                <Route path="/hotboard" element={<HotBoardList />} />
                <Route path="/infoboard" element={<InformationBoardList />} />
                <Route path="/freeboard" element={<FreeBoardList />} />
                <Route path="/clubboard" element={<ClubBoardList />} />

                <Route path="/hotboard/detail/*" element={<BoardDetail />} />
                <Route path="/infoboard/detail/*" element={<BoardDetail />} />
                <Route path="/freeboard/detail/*" element={<BoardDetail />} />
                <Route path="/clubboard/detail/*" element={<BoardDetail />} />
                <Route path="/noticeboard/detail/*" element={<NoticeBoardDetail />} />
                
                <Route path="/myarticle" element={<BoardListAboutMe />} />
                <Route path="/mycommentarticle" element={<BoardListAboutMe />} />
                <Route path="/mylikearticle" element={<BoardListAboutMe />} />
                <Route path="/mySearch" element={<SearchBoardList />} />                   

                <Route path="/useterms" element={<UseTerms />} />
                <Route path="/privacypolicy" element={<PrivacyPolicy />} />
                <Route path="/communityRule" element={<CommunityRule />} />

                <Route path="*" element={<Error />} />
            </Routes>
        </div>
    )
}

export default AfterLoginRouter
```
<br/>
# 코드
그리고 드디어 AfterLoginRouter.jsx 에는 Routes 컴포넌트 안에 Route 컴포넌트를 배치함으로써 라우트 설정을 완성할 수 있습니다.
이때 Route 컴포넌트 속성으로는 path와 element를 넣어 각각 url path와 보여주고 싶은 컴포넌트명을 넣을 수 있습니다.
<br/>그리고 만약 위의 해당하지 않는 url path가 입력될 경우, 페이지가 없다는 에러페이지를 보여줄 수 있도록 마지막에 path로 &#42; 별모양 특수문자를 넣어주었습니다.
<br/>
![image](https://user-images.githubusercontent.com/84834172/184621582-cb3842d9-9c47-45c4-8850-e3f64bfadf08.png)

<br/><br/>
# BeforeLoginContainer.jsx
```javascript
import React from 'react';
import BeforeLoginRouter from './BeforeLoginRouter';

function BeforeLoginContainer(props) {
    return (
        <div>
            <BeforeLoginRouter loginCallBack={props.loginCallBack} />
        </div>
    )
}

export default BeforeLoginContainer

```
<br/><br/>
# BeforeLoginRouter.jsx
```javascript
import React from 'react';
import { Routes, Route } from 'react-router-dom';

import Landing from '../pages/Landing/Landing.jsx';
import Login from '../pages/Login/Login.jsx';
import AdminLogin from '../pages/Login/AdminLogin.jsx';
import Forgot from '../pages/Forgot/Forgot.jsx';

import UserPassword from '../pages/Forgot/UserPassword/UserPassword.jsx';
import UserRegister from '../pages/UserRegister/UserRegister.jsx';
import Certification from '../pages/Certification/Certification.jsx';

import Error from "../component/Error";

function BeforeLoginRouter(props) {
    return (
        <div>
            <Routes>
                <Route path="/" element={<Landing />} />
                <Route path="/login" element={<Login loginCallBack={props.loginCallBack} />} />
                <Route path="/adminlogin" element={<AdminLogin loginCallBack={props.loginCallBack} />} />
                <Route path="/forgot" element={<Forgot />} />

                <Route path="/forgot/password" element={<UserPassword />} />
                <Route path="/register" element={<UserRegister />} />
                <Route path="/certification" element={<Certification />} />

                <Route path="*" element={<Error />} />
            </Routes>
        </div>
    )
}

export default BeforeLoginRouter
```
<br/>
# 코드
BeforeLoginContainr과 BeforeLoginRouter 컴포넌트도 마찬가지로 AfterLoginContainr, AfterLoginRouter 컴포넌트와  라우트 작성 규칙에 따라 작성하였습니다.

<br/><br/>
# 어려웠던 점
내용
<br/><br/>
# Router를 쓰면서 느꼈던 장점
내용
<br/><br/>
# 마무리
내용
<br/><br/> <a href="https://github.com/ram-yeon/everyday">→ [에브리데이] 프로젝트 GitHub 보러가기</a>
<h5>:page_with_curl: Acknowledgments</h5>
- <a href="https://reactrouter.com/docs/en/v6/getting-started/overview">Quick Start Overview - React Router</a>



