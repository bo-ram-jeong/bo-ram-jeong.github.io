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
<br/><br/>이 컴포넌트는 웹 애플리케이션에 HTML5의 History API를 사용하여 페이지를 새로 불러오지 않고도 주소를 변경하고 현재 주소의 경로에 관련된 정보를 리액트 컴포넌트에서 사용할 수 있도록 하는 유용한 컴포넌트입니다.
<br/><br/>
# App.jsx
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
바로 App.js에 다음과 같이 Route 설정을 해도 되지만, 저는 점점 프로젝트가 진행되면서 코드 리팩토링을 하였기 때문에 로그인 전 / 후에 따른 분기처리를 위해 AfterLoginContainer 컴포넌트와 BfterLoginContainer를 먼저 만들어서 넣었고 각 컴포넌트 안엔 다시 AfterLoginRouter 컴포넌트와 BoforLoginRouter를 만들어 넣어주었습니다.
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
<br/><br/>그 이유는 아래의 이미지에서처럼 1,2번 영역은 고정되고 2번 안의 메뉴를 누를때마다 3번의 영역만 라우트영역으로 정하여 해당 영역만 contents가 바뀔 수 있게 하기 위해서입니다.
<br/>
# 라우트가 적용된 모습
![image](https://user-images.githubusercontent.com/84834172/184620061-773792b3-d330-47ff-8e62-2cc485890406.png)
![image](https://user-images.githubusercontent.com/84834172/184629005-299bb7e1-cd32-47e7-b100-97309e1b740f.png)

<br/>
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
<br/><br/>만약 위의 해당하지 않는 url path가 입력될 경우, 페이지가 없다는 에러페이지를 보여줄 수 있도록 마지막에 path로 &#42; 별모양 특수문자를 넣어주었습니다.
<br/><br/>
![image](https://user-images.githubusercontent.com/84834172/184621582-cb3842d9-9c47-45c4-8850-e3f64bfadf08.png)
<br/><br/>
또한 중간에 ```<Route path="/hotboard/detail/*" element={<BoardDetail />} />``` 를 보면 path 맨 마지막에도 &#42; 별모양 특수문자를 넣어주었습니다. 이 이유는 게시글 목록 중 상세를 보기 위해 리스트 하나를 클릭 했을 때, url path에 맨 마지막에는 게시글의 고유 id값을 넣어주기 위함입니다. 

<br/><br/>예를 들어 다른 페이지로 이동할 때는 Link 컴포넌트를 사용하거나 navigate()를 이용하여 다른 페이지로 이동할 수 있는데요. 저는 아래의 코드에서처럼 navigate()를 이용하였습니다. 만약 Link 컴포넌트를 사용 시에는 ```<Link to="/example">링크 이름</Link>``` 과 같이 작성하면 됩니다.
<br/>
# Link 컴포넌트 사용하여 다른 페이지로 이동

**1) 게시글 클릭 시, clickBoardList() 호출**

```javascript
<List sx={{ marginTop: "-0.4rem" }}>
  {post.map(item => (
    <ListItem
      button
      key={item.id}
      onClick={() => clickBoardList(item.id)}> 
      .
      .
```
<br/> 
이 때, item.id에는 서버로부터 게시글 목록을 불러오는 요청 api 통신 성공 후, 받은 게시글 고유 id값이 들어있으며 이 값은 clickBoardList() 호출 시 파라미터로 전달합니다.
<br/><br/>
**2) navigate()를 이용하여 다른 페이지로 이동**
```javascript
const clickBoardList = (itemId) => {
  navigate('/' + boardTypeToLowerCase + 'board/detail/' + itemId, { state: { postId: itemId, headTitle: title } })
  .
  .
}
```
<br/> 
navigate는 다음과 같이 작성하면 되는데, ```navigate('url') or navigate('url', 다음페이지로 전달할 데이터(이 데이터는 받을 페이지에서 useLocation으로 받음))```
<br/>실제 url에는 예를 들어 '/freeboard/detail/27' 과 같이 나타나게 됩니다.
<br/>**+ 추가로** 위에 언급한 useLocation을 설명드리자면 데이터를 받을 때는 아래와 같이 받아서 필요한 곳에 해당 변수를 사용하면 됩니다.
```javascript
const location = useLocation();
const postId = location.state.postId;
const headTitle = location.state.headTitle;
```

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
<br/>
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
# 코드
BeforeLoginContainr과 BeforeLoginRouter 컴포넌트도 AfterLoginContainr, AfterLoginRouter 컴포넌트와 마찬가지로 라우트 작성 규칙에 따라 작성하였습니다.

<br/>
# 어려웠던 점
처음에 라우팅에 대한 기본적인 개념은 이해되지만 막상 바로 코드에 적용하고 사용하는 것이 어려웠던 것 같습니다.
<br/>그리고 지난 21년 11월, 리액트 라우터가 v5 → v6 로 업데이트 된지 모르고 실수로 v5를 사용하는 블로그를 보고 익히다가 초반엔 더 혼란을 겪었던 것 같습니다.
하지만 바로 v6 튜토리얼들을 찾아보았고 다행히 새로 접하는 입문자들을 위해 설명이 된 가이드들 덕분에 한걸음 진행할 수 있었습니다.
<br/><br/><a href="https://reactrouter.com/docs/en/v6/getting-started/overview"> → 도움 된 가이드 보러가기</a>

<br/>
# Router를 쓰면서 느꼈던 장점
라우팅을 사용함으로써 서버에게 별다른 요청을 보내지 않고 클라이언트의 브라우저 단에서만 여러 페이지들을 방문할 수 있는 기능의 편리성을 느꼈습니다. 
쉽게 말해, 기존에 페이지 이동을 하려면 새로고침이 필수였지만 라우터를 사용함에 따라 컴포넌트 업데이트를 통하여 새로고침을 하지 않아도 페이지를 이동 할 수 있게 된 것입니다.
<br/><br/>
예를 들어 저의 코드 경우에는,
NavBar와 LeftBar는 고정시키고 컨텐츠 영역만 라우팅을 설정하여 해당 영역만 컨텐츠가 바뀔 수 있도록 하는 부분이었는데, 라우팅을 통해 원하는 영역만 원하는 페이지 이동을 할 수 있었습니다.
<br/><br/><br/>
# 마무리
**라우팅의 구현에 있어 가장 중요한 핵심 세 가지**
<br/>
- 현재 URL에 맞는 UI(즉, 컴포넌트)를 렌더링 할 수 있어야 한다.
- 페이지의 리로드 없이 다른 페이지를 방문할 수 있는 내비게이션 기능이 있어야 한다.
- 사용자의 액션(앞으로 가기, 뒤로 가기 등)에 의해 URL이 변경될 때 이를 감지하고 처리할 수 있어야 한다.
<br/><br/>위의 내용에 맞게, 라우팅을 용도에 맞게 잘 사용하기 위해 노력했던 것 같습니다. 
제가 라우터를 사용하기 위해 헤맸던 시간만큼 결과적으로 결국 제가 라우팅의 개념에 대해서 한발 더 가까워질 수 있었던 시간이었고 이 포스팅을 통해 리액트 라우터에 대해 처음 접하는 분들께 라우터를 사용하는 방법에 대해 조금이나마 보탬이 되었으면 좋겠습니다.
<br/><br/>
다음 프로젝트에서는 많이 사용하는 URL 파라미터(ex. '/example/:name' 같은 형식)와 쿼리스트링(ex. '/example?page=1&keyword=ex' 같은 형식)에 대해서도 공부해서 적용해볼 것이고 아직 더 알아야할 게 많은 라우팅에 대해서도 계속해서 더 효율적인 코드를 만들 수 있도록 공부해볼 것입니다.

<br/> <a href="https://github.com/ram-yeon/everyday">→ [에브리데이] 프로젝트 GitHub 보러가기</a>
<h5>:page_with_curl: Acknowledgments</h5>
- <a href="https://reactrouter.com/docs/en/v6/getting-started/overview">React Router - Quick Start Overview </a>



