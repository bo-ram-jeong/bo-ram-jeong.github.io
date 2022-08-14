---
layout: single
title:  "[에브리데이] MUI를 이용한 화면 구현"

categories:
  - Team Project
tags:
  - mui
  - react

published: true
---

<br/>
안녕하세요.
<br/>앞 포스팅에서 언급한 에브리타임 카피사이트 [에브리데이] 프로젝트에서 화면 구현 시, 주로 사용했던 MUI에 대해 포스팅해보려고 합니다.
<br/>MUI는 React UI를 만들 때 컴포넌트 형태로 사용할 수 있도록 도움을 주는 라이브러리이며, MUI를 이용하면 Material 디자인 스타일이 적용된 UI를 구현할 수 있습니다. 공식 홈페이지는 <a href="https://mui.com/">이곳</a> 에서 확인할 수 있으며, 컴포넌트를 사용하기 위한 npm 설치 또한 확인할 수 있습니다.
<br/><br/>
MUI 컴포넌트를 사용하면 Material Design 스타일의 형식으로 만들어지지만 저는 카피사이트라는 목적에 맞게 별도의 CSS 또는 makeStyles()을 사용하여 MUI 컴포넌트를 커스텀하였습니다. (기존에 API 개발까지 개발한 전체코드였기 때문에 해당 포스팅 주제와 관련없는 컴포넌트, 함수 등의 코드는 생략하였습니다)
<br/><br/>
<a href="https://github.com/ram-yeon/everyday">→ [에브리데이] 프로젝트 GitHub 보러가기</a>
<details>
<summary>[에브리데이] 프로젝트</summary>
<h5>기간</h5>
2022.04.01 - 2022.07.30
<h5>월별 TASK</h5>
<h5>4월</h5> 
기획
- 어떤 기술을 중심으로 어떤 주제로 프로젝트를 진행할 것인지 정한 후, 프로토타입 제작<br/>
- ERD Cloud를 이용한 DB 설계<br/>
- REST API 설계<br/>
- Spring Boot와 React 각각 로컬에서 프로젝트 생성 후 연동
<h5>5월</h5>
화면 구현
<br/>
- 디렉토리 및 파일 구조 설계 및 구성<br/>
- JS, CSS 사용<br/>
- MUI 컴포넌트 사용<br/>
- 코드 리팩토링
<h5>6월 / 7월</h5>
API 개발
<br/>
- 각 메뉴 및 기능마다 API 요청 및 데이터 처리<br/>
- 코드 리팩토링(ex. Axios 요청/응답 부분 공통화)
</details>


<br/>
# BoardList.jsx
**게시글 목록**
<br/><br/>

![image](https://user-images.githubusercontent.com/84834172/184496349-554a15ab-ac9f-4762-a58b-6eb4e35d0bca.png)

```javascript
import React, { useEffect, useState } from 'react'
import { useNavigate } from 'react-router-dom';

import { makeStyles } from "@material-ui/core";
import { Box } from '@mui/material/';
import List from '@mui/material/List';
import ListItem from '@mui/material/ListItem';
import ListItemText from '@mui/material/ListItemText';
import ListItemIcon from '@mui/material/ListItemIcon';
import Pagination from '@mui/material/Pagination';
import Stack from '@mui/material/Stack';

import BorderColorIcon from '@mui/icons-material/BorderColor';
import FavoriteBorderOutlinedIcon from '@mui/icons-material/FavoriteBorderOutlined';    //MUI에서 제공하는 좋아요 아이콘
import TextsmsOutlinedIcon from '@mui/icons-material/TextsmsOutlined';                  //MUI에서 제공하는 좋아요 댓글 아이콘
import VisibilityOutlinedIcon from '@mui/icons-material/VisibilityOutlined';            //MUI에서 제공하는 좋아요 조회수 아이콘
import InsertPhotoOutlinedIcon from '@mui/icons-material/InsertPhotoOutlined';          //MUI에서 제공하는 좋아요 사진첨부 아이콘
    .
    .
    .
const useStyles = makeStyles(() => ({     //makeStyles hook의 문법에 맞게 내용은 CSS에 작성하는 것과 같이 작성
    writeBoxBtn: {
        border: "2px lightgray solid",
        color: "gray",
        backgroundColor: "#F6F6F6",
        fontSize: "0.9rem",
        textAlign: "left",
        cursor: "pointer",
        margin: "0.3rem auto",
    },
}));

function BoardList(props) {
    const {
        title,
        boardType,
    } = props;

    const classes = useStyles();      //makeStyles hook으로 작성한 함수를 호출한 결과를 classes 변수에 저장
    const navigate = useNavigate();

    const [post, setPost] = useState([]);
    const [page, setPage] = useState(1);
    const [totalPages, setTotalPages] = useState(1);
    const [show, setShow] = useState(false);

    const handleWriteBoxShow = (value) => {
        setShow(value);
    }
    const handleChange = (event, value) => {
        setPage(value);

    .
    .
    .
    
    return (
        <div>
            <Box border="2px lightgray solid" color="black" fontWeight="bold" fontSize="1.4rem" textAlign="left" p={1.5}>
                {title}
            </Box>
            {   //HOT게시물이면 글작성박스 안보이도록
                (boardType !== "HOT") ?
                    <Box p={1.8} className={classes.writeBoxBtn} onClick={() => setShow(!show)}>
                        새 글을 작성하세요.
                        <BorderColorIcon sx={{ float: "right" }} />
                    </Box>
                    : null
            }
            {show && <WriteBox boardType={boardType} handleWriteBoxShow={handleWriteBoxShow} handleIsInitialize={handleIsInitialize} />}
            <List sx={{ marginTop: "-0.4rem" }}>
                {post.map(item => (
                    <ListItem
                        sx={{ border: "1px gray solid", height: "15vh" }}
                        button
                        key={item.id}
                        onClick={() => clickBoardList(item.id)}>
                        <div>
                            <ListItemText primary={item.postTitle}
                                primaryTypographyProps = ``` {{
                                    color: 'black',
                                    width: "30rem",
                                    overflow: "hidden",
                                    whiteSpace: "nowrap",
                                    textOverflow: "ellipsis",
                                }} ``` />
                            <ListItemText
                                primary={item.postContent.replaceAll("\n", " ")}
                                primaryTypographyProps= ``` {{
                                    color: 'gray',
                                    width: "30rem",
                                    fontSize: '0.8rem',
                                    overflow: "hidden",
                                    whiteSpace: "nowrap",
                                    textOverflow: "ellipsis",
                                }} ``` />
                            <ListItemText primary={item.date}
                                primaryTypographyProps= ``` {{
                                    color: 'gray',
                                    fontSize: '0.5rem',
                                    width: "10rem",
                                }} ``` />
                            <ListItemText primary={item.user}
                                primaryTypographyProps= ``` {{
                                    fontSize: '0.7rem',
                                    width: "5rem",
                                    color: "#C00000"
                                }} ``` />
                        </div>
                        <ListItemIcon sx={{ color: '#C00000', marginLeft: "30%" }}><FavoriteBorderOutlinedIcon sx={{ fontSize: '1rem' }} /></ListItemIcon>
                        <ListItemText primary={item.likeCount}
                            primaryTypographyProps= ``` {{ 
                                color: '#C00000',
                                width: "1rem",
                                fontSize: "0.5rem",
                                margin: "0.5rem auto auto -2.2rem"
                            }} ``` />

                        <ListItemIcon sx={{ color: '#0CDAE0', marginLeft: "-0.5rem" }}><TextsmsOutlinedIcon sx={{ fontSize: '1rem' }} /></ListItemIcon>
                        <ListItemText primary={item.commentCount}
                            primaryTypographyProps= ``` {{
                                color: '#0CDAE0',
                                width: "1rem",
                                fontSize: "0.5rem",
                                margin: "0.5rem auto auto -2.2rem"
                            }} ``` />
                        <ListItemIcon sx={{ color: '#6666ff', marginLeft: "-1rem" }}><VisibilityOutlinedIcon sx={{ fontSize: '1rem' }} /></ListItemIcon>
                        <ListItemText primary={item.views}
                            primaryTypographyProps=``` {{
                                color: '#6666ff',
                                width: "1rem",
                                fontSize: "0.5rem",
                                margin: "0.5rem auto auto -2.2rem"
                            }}  ``` />
                        <ListItemIcon sx={{ color: 'gray', marginLeft: "-0.6rem" }}><InsertPhotoOutlinedIcon sx={{ fontSize: '1rem' }} /></ListItemIcon>
                        <ListItemText primary={item.fileCount}
                            primaryTypographyProps= ``` {{
                                color: 'gray',
                                width: "1rem",
                                fontSize: "0.5rem",
                                margin: "0.5rem auto auto -2.2rem"
                            }} ``` />

                    </ListItem>
                ))}
            </List>

            <Stack spacing={2} style={{ marginTop: '1.5rem' }}>
                <Pagination count={totalPages} page={page} onChange={handleChange} />
            </Stack>
        </div>
    )
}
export default BoardList;
```
<br/>
# 코드
다음 MUI 컴포넌트를 사용하였습니다. ```<Box/> <List/> <ListItem/> <ListItemText/> <ListItemIcon/> <Stack/>``` <br/>
List 관련 컴포넌트 같은 경우, List 안에 ListItem 그 안에 ListItemText, ListItemIcon 컴포넌트를 배치하여 리스트 하나에 보여주고 싶은 내용들로 구성할 수 있습니다.<br/>
그리고 List 안에 post배열은 ListItemText 등의 컴포넌트에 들어갈 데이터가 담긴 배열이므로 map을 통해 모든 리스트의 값을 가져올 수 있습니다.
좋아요, 댓글 등과 같은 아이콘이 필요한 부분은 <a href="https://mui.com/material-ui/material-icons/#main-content">이곳</a> MUI가 제공하는 것을 사용하였습니다.
또한 생략되었지만 primaryTypographyProps 안에 color, width 등과 같은 style을 정의할 수 있습니다.
<br/>
ex)       
```
primaryTypographyProps={{                            
  color: 'black', 
  width: "30rem", 
  overflow: "hidden", 
  whiteSpace: "nowrap",
  textOverflow: "ellipsis", 
}}
```
<br/>
# 보완이 필요한 부분
아무래도 편리한 style작성을 위해 makeStyles()과 필요한 코드 줄에 style을 적용하니 코드가 길어지고 가독성이 떨어지는 것을 느꼈습니다. 별도의 파일로 작성하여 코드를 정리하는 보완이 필요할 것 같습니다.
<br/><br/>

# BoardDetail.jsx
**게시글 상세**
<br/><br/>
![image](https://user-images.githubusercontent.com/84834172/184504435-7ae45957-0dca-459e-81e4-c3cbb63bb12c.png)

~~~javascript

import React, { useState, useEffect } from 'react';
import { Link, useNavigate, useLocation } from 'react-router-dom';

import { makeStyles, Typography } from "@material-ui/core";
import { Box } from '@mui/material/';
import AccountCircleIcon from '@mui/icons-material/AccountCircle';
import FavoriteOutlinedIcon from '@mui/icons-material/FavoriteOutlined';                //색채워진좋아요
import FavoriteBorderOutlinedIcon from '@mui/icons-material/FavoriteBorderOutlined';    //좋아요
import TextsmsOutlinedIcon from '@mui/icons-material/TextsmsOutlined';                  //댓글
import VisibilityOutlinedIcon from '@mui/icons-material/VisibilityOutlined';            //조회수
import InsertPhotoOutlinedIcon from '@mui/icons-material/InsertPhotoOutlined';          //사진첨부

const useStyles = makeStyles((theme) => ({
    headLink: {
        textDecoration: 'none',
        cursor: 'pointer',
        color: 'black',
        "&:hover": {
            color: '#C00000',
        }
    },
    writerIcon: {
        color: "gray",
        [theme.breakpoints.up("sm")]: {
        },
    },
    postUpdate: {
        color: "gray",
        fontSize: "0.8rem",
        display: "inline",
        cursor: "pointer",
    },
    postDelete: {
        color: "gray",
        fontSize: "0.8rem",
        display: "inline",
        marginLeft: "1rem",
        cursor: "pointer",
    },
    replyDelete: {
        color: "gray",
        fontSize: "0.6rem",
        cursor: "pointer",
    },
    writer: {
        fontWeight: "bold",
        fontSize: "0.9rem",
    },
    date: {
        color: "gray",
        fontSize: "0.7rem",
    },
    listBtn: {
        width: "10%",
        height: "2.5rem",
        background: "#C00000",
        color: "white",
        border: "none",
        cursor: "pointer",
        boxShadow: "0.1rem 0.1rem 0.3rem 0.1rem gray",
        borderRadius: "0.5rem",
        marginTop: "1rem",
        float: "right",
    },
}));

function BoardDetail() {
    const location = useLocation();
    const postId = location.state.postId;
    const headTitle = location.state.headTitle;
    const classes = useStyles();
    .
    .
    .
    return (
        <div>
            <Box border="2px lightgray solid" color="black" fontWeight="bold" fontSize="1.4rem" textAlign="left" p={1.5}>
                <Link to={'/' + boardTypeToLowerCase + 'board'} className={classes.headLink}>{headTitle}</Link>
            </Box>
            {!displayEditBox ?
                <div>
                    <Box border="2px lightgray solid" color="black" textAlign="left" marginTop="0.3rem" p={2} >
                        <AccountCircleIcon className={classes.writerIcon} />
                        {tokenJson.sub === writerLoginId && (
                            <div style={{ float: "right" }}>
                                <Typography className={classes.postUpdate} onClick={() => editPost(true)}>수정</Typography>
                                <Typography className={classes.postDelete} onClick={deletePost}>삭제</Typography>
                            </div>
                        )}
                        <Typography className={classes.writer}>{writer}</Typography>
                        <Typography className={classes.date}>{displayDateFormat(registrationDate)}</Typography>
                        <Typography style={{ fontSize: '1.8rem', marginTop: "1rem" }}><strong>{title}</strong></Typography>
                        <Typography style={{ margin: "0.5rem auto auto 0.3rem" }}>
                            {contents.split("\n").map((data) => { 
                                return (<span>{data}<br /></span>);
                            })}
                        </Typography>

                        <div style={{ margin: "2rem auto auto 0.3rem" }}>
                            {
                                (!likeState) ?
                                    <FavoriteBorderOutlinedIcon sx={{ fontSize: '1rem', color: '#C00000', cursor: 'pointer' }} onClick={clickLike} />
                                    :
                                    <FavoriteOutlinedIcon sx={{ fontSize: '1rem', color: '#C00000', cursor: 'pointer' }} onClick={clickLike} />
                            }<span style={{ marginLeft: "0.2rem", fontSize: "0.7rem", color: '#C00000', cursor: 'pointer' }} onClick={clickLike}>{likeCount}</span>
                            <TextsmsOutlinedIcon sx={{ fontSize: '1rem', color: '#0CDAE0', marginLeft: "1rem" }} /><span style={{ marginLeft: "0.2rem", fontSize: "0.7rem", color: '#0CDAE0' }}>{commentCount}</span>
                            <VisibilityOutlinedIcon sx={{ fontSize: '1rem', color: '#6666ff', marginLeft: "1rem" }} /><span style={{ marginLeft: "0.2rem", fontSize: "0.7rem", color: '#6666ff' }}>{views}</span>
                            <InsertPhotoOutlinedIcon sx={{ fontSize: '1rem', color: 'gray', marginLeft: "1rem" }} /><span style={{ marginLeft: "0.2rem", fontSize: "0.7rem", color: 'gray' }}>{fileCount}</span>
                        </div>
                    </Box >

                    {/* 댓글 */}
                    <CommentList key={comment.commentId} comment={comment} postId={postId} handleIsInitialize={handleIsInitialize} />

                    {/* 게시판따라경로분기처리 */}
                    <Link to={'/' + boardTypeToLowerCase + 'board'}><button className={classes.listBtn}>목록</button></Link>
                </div>
                :
                <EditBox boardType={boardType} postId={postId} writtenTitle={title} writtenContents={contents}
                    editPost={editPost} handleIsInitialize={handleIsInitialize} />
            }
        </div>
    )
}
export default BoardDetail

~~~
<br/>
# 코드
게시글 상세페이지에서는 ```<Box/> <Typography/> ..``` 컴포넌트를 사용하였습니다.
<br/><Typography/>는 @material-ui/core 패키지로 부터 <Typography/> 컴포넌트를 불러와 사용할 수 있고 이를 사용함으로써 다양한 스타일의 텍스트를 연출할 수 있습니다.
위 코드에서처럼 속성으로 style을 지정할 수도 있지만 텍스트의 크기는 다음처럼 variant prop으로 제어할 수 있고 이 때, variant="h1"을 사용하면 <h1/> 태그로 마크업이 됩니다. ```<Typography variant="h1">Typo</Typography>``` 
<br/> 또한 텍스트는의 색상은 color prop으로, align은 align prop으로 다음과 같이 제어할 수 있습니다.
<br/> ```<Typography color="textSecondary">Color</Typography> ```
<br/> ```<<Typography align="center">Center</Typography>```

<br/><br/>

# NavBar.jsx
**Navbar 메뉴**
<br/>
![image](https://user-images.githubusercontent.com/84834172/184507097-2ff34f6e-6963-40dd-8058-848288e1f45a.png)

```javascript

import React, { useState } from 'react'
import ModalContainer from './ModalContainer';
import { Link, useNavigate } from 'react-router-dom';

import { AppBar, makeStyles, Toolbar, Typography } from "@material-ui/core";
import { Search } from '@mui/icons-material';
import { InputBase } from '@mui/material';

import { Avatar } from 'antd';

const useStyles = makeStyles((theme) => ({
  toolbar: {
    display: "flex",
    justifyContent: "space-between"
  },
  imgLogo: {
    display: "flex",
    margin: "1rem 1rem 1rem 2rem",
    [theme.breakpoints.down("sm")]: {
      display: "flex",
      height: "2rem",
      margin: "2rem 0rem",
    },
  },
  textLogo: {
    color: "black",
    [theme.breakpoints.down("sm")]: {
      fontSize: "0.4rem",
      paddingLeft: "0.8rem"
    },
  },
  schoolName: {
    color: "black",
    fontSize: "1.3rem",
    [theme.breakpoints.down("sm")]: {
      fontSize: "1rem",
      paddingLeft: "0.8rem"
    },
  },
  search: {
    color: "gray",
    paddingLeft: "0.7rem",
    display: "flex",
    height: "2.5rem",
    width: "25%",
    marginLeft: "55%",
    marginTop: "5px",
    border: "2px lightgray solid",
    // backgroundColor: alpha(theme.palette.common.white, 1),
    borderRadius: theme.shape.borderRadius,
    [theme.breakpoints.down("sm")]: {
      display: "none",
    },
  },
  input: {
    marginLeft: theme.spacing(1),
    width: "100%",
  },
  myImg: {
    marginLeft: "1rem",
    marginTop: "0.3rem",
    cursor: "pointer",
  },
  adminLogout: {
    marginLeft: "82%",
    cursor: "pointer",
    border: "none",
    background: "white",
    color: "gray",
    textDecoration: "underline",
  }
}));

function NavBar(props) {
  const classes = useStyles({});
  const navigate = useNavigate();
  
  const [open, setOpen] = useState(false);
  
  const [schoolName, setSchoolName] = useState('');
  const [id, setId] = useState('');
  const [name, setName] = useState('');
  const [nickname, setNickname] = useState('');
  
  const [searchKeyword, setSearchKeyword] = useState('');
  .
  .
  .
  return (
    <>
      {
        (tokenJson.account_authority === 'MANAGER')                                                               //관리자일때,
          ?
          <AppBar position="fixed" style={{ background: 'white', height: "5rem" }}>
            <Toolbar className={classes.Toolbar}>
              <Link to='/'><Avatar alt="로고이미지" src={"/images/smallLogo.png"} className={classes.imgLogo}></Avatar></Link>
              <div>
                <Typography className={classes.textLogo} style={{ fontWeight:'bold', fontSize: '0.8rem', color: '#C00000' }}>
                  에브리데이
                </Typography>
              </div>
              <button className={classes.adminLogout} onClick={adminLogoutBtn}>로그아웃</button>
            </Toolbar>
          </AppBar>
          :                                                                                                         //사용자일때,
          <AppBar position="fixed" style={{ background: 'white', height: "5rem" }}>
            <Toolbar className={classes.Toolbar}>
              <Link to='/'><Avatar alt="로고이미지" src={"/images/smallLogo.png"} className={classes.imgLogo}></Avatar></Link>
              <div>
                <Typography className={classes.textLogo} style={{ fontWeight:'bold', fontSize: '0.8rem', color: '#C00000' }}>
                  에브리데이
                </Typography>
                <Typography className={classes.schoolName}>
                  {schoolName}
                </Typography>
              </div>

              <div className={classes.search}>
                <Search sx={{ marginTop: '0.3rem' }} />
                <InputBase
                  value={searchKeyword}
                  onChange={(e) => { setSearchKeyword(e.target.value); onChangeKeyword(searchKeyword); }}
                  placeholder="전체 게시판의 글을 검색해보세요!"
                  className={classes.input} />
              </div>
              <div className={classes.myImg}>
                <Avatar alt="My계정 이미지" src={"/images/myImg.png"}
                  onClick={handleOpen} />
              </div>
              <ModalContainer
                open={open}
                handleClose={handleClose}
                loginCallBack={props.loginCallBack}
                id={id}
                name={name}
                nickname={nickname}
              >
              </ModalContainer>
            </Toolbar>
          </AppBar>
      }
    </>
  );
}
export default NavBar

```
<br/>
# 코드
```<AppBar/> <Toolbar/>```를 이용하여 navbar를 만들었으며 ```<InputBase/>```를 사용하여 검색박스를 만들었습니다. 또한 사람형태의 이미지가 있는 아이콘을 누르면 아래 Modal의 이미지처럼 모달창이 뜨면서 다른메뉴들을 볼 수 있도록 먼저 ```<ModalContainer/>```라는 별도의 컴포넌트 파일을 만들었고 모달창에 뜨는 메뉴와 정보들은 ModalContainer.jsx안에 작성하였습니다. 모달창에 뜨는 아이디와 같은 필요한 정보들은 NavBar.jsx에서 props로 전달하였고 모달창이 open, close 될 수 있도록 해당 속성도 props로 전달해주었습니다.
<br/>
중간에 Ant Design에서 제공하는 ```<Avatar/>``` 컴포넌트를 사용하여 이미지를 추가하였는데 이 부분은 MUI의 ```<Avatar/>``` 컴포넌트를 사용하여도 무방합니다.

# 리팩토링이 필요한 부분
검색박스에 경우 MUI에 제공하는 검색 컴포넌트를 사용하지 않고 돋보기 아이콘 + InputBase컴포넌트를 조합하였더니 다소 디자인적으로 아쉬웠던 것 같습니다. 이 부분은 MUI에서 제공하는 <a href=<https://mui.com/material-ui/react-app-bar/">이곳</a> 에서 가이드를 따르는 것도 좋은 방법이 될 것 같습니다.
<br/><br/>

# ModalContainer.jsx
**Modal**
<br/><br/>
![image](https://user-images.githubusercontent.com/84834172/184506873-f644c008-939e-4857-b382-3b4f6bc7c8ff.png)

```javascript

import React from 'react'
import { useNavigate } from 'react-router-dom';

import { Container, Modal, makeStyles, ListItemIcon } from "@material-ui/core";
import ChatIcon from '@mui/icons-material/Chat';
import FavoriteOutlinedIcon from '@mui/icons-material/FavoriteOutlined';                
import TextsmsIcon from '@mui/icons-material/Textsms';
import ExitToAppIcon from '@mui/icons-material/ExitToApp';
import SentimentDissatisfiedIcon from '@mui/icons-material/SentimentDissatisfied';
import List from '@mui/material/List';
import ListItemButton from '@mui/material/ListItemButton';
import ListItemText from '@mui/material/ListItemText';

import { Avatar } from 'antd';

const useStyles = makeStyles((theme) => ({
    myImg: {
        fontSize: 1rem,
        [theme.breakpoints.down("sm")]: {
          fontSize: 0.5rem,
        },
    },
    modal: {
        width: 200,
        height: 470,
        backgroundColor: "white",
        position: "absolute",
        top: 80,
        right: 50,
        margin: "auto",
        textAlign: "center",
        paddingTop: "3rem",
        outline: "none",
        [theme.breakpoints.down("sm")]: {
        },
    },
}));

function ModalContainer(props) {
    const {
        open,
        handleClose,
        loginCallBack,
        id,
        name,
        nickname,
    } = props;
    
    const classes = useStyles();
    const navigate = useNavigate();
    
    const myDataList = [
        {
            text: "내가 쓴 글",
            icon: <ChatIcon sx={{color:'#7FBEFF'}}/>,
            idx: '0',
        },
        {
            text: "댓글 단 글",
            icon: <TextsmsIcon sx={{color:'#0CDAE0'}}/>,
            idx: '1',
        },
        {
            text: "좋아요 한 글",
            icon: <FavoriteOutlinedIcon sx={{color:'#C00000'}}/>,
            idx: '2',
        },
        {
            text: "로그아웃",
            icon: <ExitToAppIcon />,
            idx: '3'
        },
        {
            text: "탈퇴하기",
            icon: <SentimentDissatisfiedIcon />,
            idx: '4'
        }
    ]
    .
    .
    .  
    return (
        <div>
            <Modal
                open={open}
                onClose={handleClose}
            >
                <Container className={classes.modal}>
                    <Avatar alt="My계정 이미지" src={"/images/myImg.png"} />
                    <p style={{ fontWeight: "bold" }}>{nickname}</p>
                    <div style={{ color: "gray", fontSize: "0.8rem" }}>{name}</div>
                    <div style={{ color: "gray", fontSize: "0.8rem", marginBottom: "1rem" }}>{id}</div>
                    <hr />
                    <List>
                        {myDataList.map(item => (
                            <ListItemButton
                                key={item.text}
                                sx={{ padding: "0.5rem 0rem 0.5rem 0.5rem" }}
                                onClick={(event) => handleListItemClick(event, item.idx)}
                            >
                                <ListItemIcon>{item.icon}</ListItemIcon>
                                <ListItemText primary={item.text} sx={{ marginLeft: "-1rem" }} />
                            </ListItemButton>
                        ))}
                    </List>
                </Container>
            </Modal>
        </div>
    )
};
export default ModalContainer

```
<br/>
# 코드
앞서 NavBar.jsx에서 props 속성으로 보내주었던 변수들은 각각 open, handleClose, loginCallBack, id, name, nickname 변수로 다시 받아 사용하였고, 위의 게시글 목록 부분에서 사용한 ```<List/>``` 컴포넌트를 Modal에서도 사용하였습니다. 그리고 myDataList 배열에는 필요한 text, icon, idx 정보를 넣었고 해당 배열을 map으로 돌면서 모달창에 리스트로 뿌려줄 수 있도록 하였습니다. 

# 보완이 필요한 부분
해당 모달창이 open될 경우, 뒷 배경이 어둡게 되는데 이것을 없애는 방법을 찾아보았지만 찾지 못하였습니다. 용도에 따라 모달을 잘 이용하지 못한 것 같아 아쉬웠던 것 같습니다. 다시 구현하게 된다면 다이얼로그나 다른 컴포넌트를 찾아 보완이 필요할 것 같습니다.
<br/><br/>
# 어려웠던 점
디자인적인 부분을 생각하며 그것을 코드로 옮기고 그대로 생각하는 것처럼 화면에 반영되지 않는 것이 어려웠습니다. 또한 화면 해상도을 고려하여 반응형으로 CSS를 작성하는 것이 쉽지 않았던 것 같습니다.
<br/><br/>
# 마무리
MUI 컴포넌트를 사용하면서 수고와 시간비용을 절약할 수 있어, 유용하게 사용하였습니다. 
<br/>하지만 그저 MUI의 오픈 소스 코드를 재사용하는 것이 아닌, 어떻게 내 프로젝트에 맞게 커스텀하고 응용하며 어떻게 내 코드에 녹일 것인지가 중요하다고 생각했기 때문에 그 부분을 고민하며 개발을 진행했던 것 같습니다.
<br/>다음에 UI 라이브러리를 사용하게 된다면, Bootstrap, Ant Design, semantic UI 과 같은 더 다양한 라이브러리를 가지고 커스텀해보고 싶습니다. 
<h5>:page_with_curl: Acknowledgments</h5>
- <a href="https://mui.com/">MUI</a>



