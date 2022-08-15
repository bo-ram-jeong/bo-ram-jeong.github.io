---
layout: single
title:  "[에브리데이] localStorage 이용한 로그인 및 로그아웃"

categories:
  - Team Project
tags:
  - login
  - logout
  - localStorage
  - react

published: true
---

<br/>
안녕하세요.
<br/>오늘은 [에브리데이] 프로젝트에서 localStorage를 사용해서 구현한 로그인 및 로그아웃을 포스팅해보려고 합니다.
<br/><br/>localStorage는 간단히 말하면 로컬에 저장하는 임시저장소라고 할 수 있습니다. windows 전역 객체의 LocalStorage라는 컬렉션을 통해 저장, 조회가 이루어집니다.
<br/><br/>
이 객체는 아래 method와 property를 제공합니다.
- setItem(key, value) – 키-값 쌍을 보관
- getItem(key) – 키에 해당하는 값을 받아옴
- removeItem(key) – 키와 해당 값을 삭제
- clear() – 모든 것을 삭제
- key(index) – 인덱스(index)에 해당하는 키를 받아옴
- length – 저장된 항목의 개수를 얻음

<br/>이 객체는 Map과 유사하여, setItem/getItem/removeItem을 지원합니다. 하지만 인덱스를 사용해 키에 접근할 수 있다는 점(key(index))에서 차이가 있습니다.

<br/><br/>
# login.js
<br/>로그인 버튼을 눌렀을 시, id, pw, type(user or manager), 로그인유지 체크 여부 데이터를 담아 미리 정의해둔 UserAPI를 통해 로그인 api요청을 보냅니다.<br/>
```javascript
import React, { useState } from 'react'
import './Login.css'
import { Link } from 'react-router-dom';
import { FormControlLabel, Checkbox } from '@mui/material';

// import { useDispatch } from 'react-redux';
// import { loginUser } from '../../_actions/user_action';
import * as UserAPI from '../../api/Users';
import { Message } from '../../component/Message';
import { useNavigate } from 'react-router-dom';

function Login(props) {
    const [idVal, setIdVal] = useState("");
    const [pwVal, setPwVal] = useState("");
    const [checked, setChecked] = useState(false);
    // const { state } = useLocation();  //이전페이지에서 받은 type값(user인지 manager인지)
    const navigate = useNavigate();
    // const dispatch = useDispatch();
    
    const handleCheckBox = (event) => {
        setChecked(event.target.checked);
      };

    const handleBtn = (event) => {
        // event.preventDefualt();
        let isKeptLogin = '';
        if (checked) {
            isKeptLogin = 'Y'
        } else {
            isKeptLogin = 'N'
        }
        const data = {
            loginId: idVal,
            password: pwVal,
            type: 'USER',
            isKeptLogin: isKeptLogin,
        }
        UserAPI.login(data).then(response => {
            props.loginCallBack(true);
            navigate('/');
        }).catch(error => {
            console.log(JSON.stringify(error));
            Message.error(error.message);
        });
        // redux사용
        // dispatch(loginUser(data))
        //     .then(response => {
        //         if(response.payload.loginSuccess){
        //             props.history.phsh('/')
        //             // navigate("/");
        //         } else{
        //             alert('Error');
        //         }
        //     })
    }

    return (
        <div className="login-contain">
            <div className="login-content">
                <div >
                    <div className="login-header img-class">
                        <img src={"/images/logo.png"} id="img-id" alt="로고이미지" />
                    </div>
                    <div className="login-header">
                        <p style={{ color: 'dimgray', fontWeight: 'bold', fontStyle: 'italic' }}>지금 에브리데이를 시작해보세요!</p>
                    </div>
                </div>
                <input type="id" className="login-input" placeholder="아이디"
                    value={idVal} onChange={(e) => { setIdVal(e.target.value) }} />
                <input type="password" className="login-input" placeholder="비밀번호"
                    value={pwVal} onChange={(e) => setPwVal(e.target.value)} />
                <button onClick={handleBtn} type="submit" id="login-btn">로그인</button>
                <div>
                    <div className="login-footer">
                        <FormControlLabel control={<Checkbox value="remember" color="default" size="small" checked={checked} onChange={handleCheckBox} />}
                            label="로그인 유지" />
                    </div>
                    <div className="login-footer">
                        <Link id="forgot-link" to='/forgot'>아이디/비밀번호 찾기</Link>
                    </div>
                </div>

                <div>
                    <p style={{ color: 'gray', textAlign: 'center', fontSize: '0.8rem' }}>에브리데이에 처음이신가요? <Link id="register-link" to='/register'>회원가입</Link></p>
                </div>
            </div>
        </div>
    )
}

export default Login

```
<br/>
# Users.jsx
<br/>login.jsx에서 바로 Axios 요청을 보내지 않고 Users.jsx에 모두 정의해놓았습니다. 또한 NewPromise 컴포넌트로 한번더 감싸놓았는데 NewPromise의 코드는 아래에 있습니다.<br/>
```javascript
import Axios from "../component/Axios/Axios";
import { NewPromise } from "../component/Common";

//이메일인증
export const authenticate = (data) => NewPromise(Axios.post('/email-authenticate', data));
//이메일인증 코드 확인
export const authenticateCodeCheck = (data) => NewPromise(Axios.post('/check-authenticationcode', data));

//가입
export const join = (data) => NewPromise(Axios.post('/users', data));

//로그인
export const login = (data) => NewPromise(Axios.post('/login', data));
//로그아웃
export const logout = () => NewPromise(Axios.get('/logout'));
//탈퇴
export const resign = () => NewPromise(Axios.delete('/users'));

.
.
.

```
<br/>

# Axios.jsx
Axios 컴포넌트를 생성하여 axios를 사용할때마다 헤더를 매번 넣지 않기 위해, 에러가 발생했을때 공통으로 처리하기 위해 Axios 요청 관련한 설정값은 이 파일에 모두 정리해두었습니다.
그리고 요청하기 직전에 interceptors를 사용하여 localStorage에서 SESSION_TOKEN_KEY 값을 가져와 header에 토큰을 넣어서 서버에 보낼 수 있게끔 하였습니다. 이로써 매번 헤더에 토큰을 넣어 서버에 보내는 일을 공통화하였습니다.
```javascript
import CommonAxios from 'axios';

const Axios = CommonAxios.create({
    // timeout: 30000,
    headers: {
        'Content-Type': 'application/json; charset=UTF-8;',
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Headers': '*',
    },
});

if (process.env.NODE_ENV === 'development') {
    Axios.defaults.baseURL = 'http://localhost:8080'
}

export const SESSION_TOKEN_KEY = "__EVERYDAY__auth__";

Axios.interceptors.request.use(function (config) {
    const token = localStorage.getItem(SESSION_TOKEN_KEY);  //api요청시 토큰키 값 넣어서 요청
    // config.headers.Authorization = "Bearer" + token;
    config.headers.Authorization = token;
    config.data = JSON.stringify(config.data);
    return config;
});
.
.
.
export default Axios;

```
<br/>

# Common.jsx
이 파일에서 NewPromise를 export하였습니다. api요청이 성공했을 때, config의 url이 로그인 관련된 path이면 localStorage에 SESSION_TOKEN_KEY 키값으로 서버에서 받은 response의 headers의 토큰값을 저장하도록 하였습니다. 
```javascript
import * as CommonJs from "../lib/Common";
import {SESSION_TOKEN_KEY} from '../component/Axios/Axios';
export const NewPromise = (promise) => {
    return new Promise(function (resolve, reject) {
        promise
            .then((response) => {
                if (200 === response.status) {
                    if (response.config.url === '/login' || response.config.url === '/adminlogin') {
                        localStorage.setItem(SESSION_TOKEN_KEY, response.headers.authorization);    //토큰키에 응답받은 토큰값 set
                    }
                    resolve(response.data);
                } else {
                    reject({error: {}, message: response.statusText});
                }
            })
            .catch((error) => {
                const errorMessage = CommonJs.extractErrorMessage(error);
                reject({error: error, message: errorMessage});
            });
    });
};
```
<br/>
# ModalContainer.jsx
로그아웃 코드입니다. 로그아웃과 탈퇴하기 모두 localStorage.removeItem()을 통해 토큰값을 지웁니다.
```javascript
const handleListItemClick = (event, idx) => {
        if (Number(idx) === 0) { //내가 쓴 글
            navigate('/myarticle', {state: {headTitle:'내가 쓴 글', typeId: 1}});
            props.handleClose(false);
        }
        else if (Number(idx) === 1) { //댓글 단 글
            navigate('/mycommentarticle', {state: {headTitle:'댓글 단 글', typeId: 2}});
            props.handleClose(false);
        }
        else if (Number(idx) === 2) { //좋아요 한 글
            navigate('/mylikearticle', {state: {headTitle:'좋아요 한 글', typeId: 3}});
            props.handleClose(false);
        }
        else if (Number(idx) === 3) { //로그아웃                                   //인자값으로 받은 idx가 3일 때, 로그아웃 로직 실행
            UserAPI.logout().then(response => {
                console.log(JSON.stringify(response));
                localStorage.removeItem(SESSION_TOKEN_KEY);
                loginCallBack(false);
                navigate("/");
            }).catch(error => { //만료된 토큰이거나 존재하지않는 토큰이면 강제로그아웃 
                console.log(JSON.stringify(error));
                Message.error(error.message);
                localStorage.removeItem(SESSION_TOKEN_KEY);
                loginCallBack(false);
                navigate("/");
            });
        }
        else if (Number(idx) === 4) { //탈퇴하기                                    //인자값으로 받은 idx가 4일 때, 탈퇴하기 로직 실행
            UserAPI.resign().then(response => {
                console.log(JSON.stringify(response));
                localStorage.removeItem(SESSION_TOKEN_KEY);
                loginCallBack(false);
                navigate("/");
            }).catch(error => { 
                console.log(JSON.stringify(error));
                Message.error(error.message);
            });
        }
.
.
.
<List>
  {myDataList.map(item => (
    <ListItemButton
      key={item.text}
      sx={{ padding: "0.5rem 0rem 0.5rem 0.5rem" }}
      onClick={(event) => handleListItemClick(event, item.idx)}       //리스트 클릭 시 handleListItemClick 함수 호출
    >
      <ListItemIcon>{item.icon}</ListItemIcon>
      <ListItemText primary={item.text} sx={{ marginLeft: "-1rem" }} />
    </ListItemButton>
  ))}
</List>
.
.
.

```
<br/>

# 어려웠던 점
아무래도 로그인 정보를 어디에 담을지 어떤 라이브러리를 사용할지 정말 고민이 많았던 것 같습니다. 
<br/>login.jsx 주석된 코드를 보시면 짐작하셨듯이 처음엔 상태 관리 라이브러리인 Redux를 이용하여 로그인을 구현하였습니다. 
<br/><br/>로그인 관리와 저장을 설명하는 여러 강의와 블로그를 참고하여 최종적으로 <a href="https://loy124.tistory.com/249">이곳</a> 에서 도움을 받아 코드를 작성해보았는데, 아무래도 리덕스에 대해 정확한
이해와 개념을 가지고 작성하는 것이 아니다 보니 똑같이 작성하여도 알 수 없는 이유로 제대로 구현이 되지 않았습니다. <br/><br/>결과적으로 비교적 사용이 쉬운 localStorage를 사용하여 로그인을 구현하였지만,
다음 시도에서는 리덕스를 통해 다시 구현해볼 예정입니다. 완성되면 다시 포스팅해보도록 하겠습니다.

<br/>
# localStoarage 이용한 로그인 장점 :wink:
먼저 로그인 정보인 토큰 값, JWT를 저장하기에 CSRF 공격에는 안전합니다. 
<br/>**CSRF(Cross Site Request Forgery)**
<br/>: 정상적인 request를 가로채 백엔드 서버에 변조된 request를 보내 악의적인 동작을 수행하는 공격을 의미(피해자 정보 수정, 정보 열람) 
<br/><a href="https://www.imperva.com/learn/application-security/csrf-cross-site-request-forgery/">→ CSRF 자세히 보고싶다면?</a>
<br/><br/>
그 이유는 자동으로 request에 담기는 쿠키와는 다르게 js 코드에 의해 헤더에 담기므로 XSS를 뚫지 않는 이상, 공격자가 정상적인 사용자인 척 request를 보내기가 어렵습니다.

<br/>
# localStoarage 이용한 로그인 단점 :disappointed:
하지만, XSS에 취약하여 공격자가 localStorage에 접근하는 Js 코드 한 줄만 주입하면 localStorage를 공격자가 맘대로 드나들 수 있습니다.
<br/>**XSS(Cross Site Scripting)**
<br/>: 공격자가 상대방의 브라우저에 스크립트가 실행되도록 하여, 사용자의 세션을 가로채거나 웹사이트를 변조하거나 악의적 콘텐츠를 삽입하거나 피싱 공격을 진행하는 것을 말함
<br/><a href="https://nordvpn.com/ko/blog/xss-attack/">→ XSS 자세히 보고싶다면?</a>
<br/><br/>따라서, 보안을 강화하기 위한 좋은 방법으로는 refresh token을 사용하는 방법도 있고 <a href="https://velog.io/@yaytomato/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%90%EC%84%9C-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EC%B2%98%EB%A6%AC%ED%95%98%EA%B8%B0">이곳1,</a> 
<a href="https://dev.to/cotter/localstorage-vs-cookies-all-you-need-to-know-about-storing-jwt-tokens-securely-in-the-front-end-15id">이곳2</a> 를 참조할 수 있습니다.
<br/><br/>
# 마무리
구현하는데 앞서 여유를 가지고 이 방법, 저 방법 모두 시도해보지 못했던 것이 아쉬움이 남는 것 같습니다. 
<br/><br/>이번 프로젝트에서는 access token만 사용해보았지만 다음 번엔 refresh token을 사용하여 '로그인 유지' api요청을 보낼때 사용자의 로그인 유지 유효기간이 지나면
refresh token을 request에 담아 새로운 access token을 발급받아 진행할 수 있도록 해보고 싶습니다.
<br/><br/>아쉬움이 남는 부분이었지만 결과적으로 localStorage로는 구현을 잘 마무리할 수 있었고 로그인 구현에 대한 다양한 방법에 대해서도 많이 알아볼 수 있는 시간이었어서 의미있는 시간이었던 것 같습니다.

<br/> <a href="https://github.com/ram-yeon/everyday">→ [에브리데이] 프로젝트 GitHub 보러가기</a>
<h5>:page_with_curl: Acknowledgments</h5>
- <a href="https://www.geeksforgeeks.org/how-to-log-out-user-from-app-using-reactjs/">GeeksForGeeks</a>
- <a href="https://ko.javascript.info/localstorage">localStorage</a>



