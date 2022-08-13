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
<br/>MUI는 React UI를 만들 때 컴포넌트 형태로 사용할 수 있도록 도움을 주는 라이브러리이며, MUI를 이용하면 Material 디자인 스타일이 적용된 UI를 구현할 수 있습니다.
<br/><br/>
MUI 컴포넌트를 사용하면 Material Design 스타일의 형식으로 만들어지지만 저의 코드는 카피사이트였기 때문에 목적에 맞게 에브리타임 사이트를 카피하기 위해 별도의 CSS 또는  makeStyles()을 사용하여 디자인을 수정하였습니다. 그리고 기존에 API 개발까지 개발한 전체코드였기 때문에 해당 포스팅 주제와 관련없는 컴포넌트, 함수 등의 코드는 생략하였습니다.

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
<br/>
![image](https://user-images.githubusercontent.com/84834172/184496349-554a15ab-ac9f-4762-a58b-6eb4e35d0bca.png)

```
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
                                primaryTypographyProps={{
                                    color: 'black',
                                    width: "30rem",
                                    overflow: "hidden",
                                    whiteSpace: "nowrap",
                                    textOverflow: "ellipsis",
                                }} />
                            <ListItemText
                                primary={item.postContent.replaceAll("\n", " ")}
                                primaryTypographyProps={{
                                    color: 'gray',
                                    width: "30rem",
                                    fontSize: '0.8rem',
                                    overflow: "hidden",
                                    whiteSpace: "nowrap",
                                    textOverflow: "ellipsis",
                                }} />
                            <ListItemText primary={item.date}
                                primaryTypographyProps={{
                                    color: 'gray',
                                    fontSize: '0.5rem',
                                    width: "10rem",
                                }} />
                            <ListItemText primary={item.user}
                                primaryTypographyProps={{
                                    fontSize: '0.7rem',
                                    width: "5rem",
                                    color: "#C00000"
                                }} />
                        </div>
                        <ListItemIcon sx={{ color: '#C00000', marginLeft: "30%" }}><FavoriteBorderOutlinedIcon sx={{ fontSize: '1rem' }} /></ListItemIcon>
                        <ListItemText primary={item.likeCount}
                            primaryTypographyProps={{
                                color: '#C00000',
                                width: "1rem",
                                fontSize: "0.5rem",
                                margin: "0.5rem auto auto -2.2rem"
                            }} />

                        <ListItemIcon sx={{ color: '#0CDAE0', marginLeft: "-0.5rem" }}><TextsmsOutlinedIcon sx={{ fontSize: '1rem' }} /></ListItemIcon>
                        <ListItemText primary={item.commentCount}
                            primaryTypographyProps={{
                                color: '#0CDAE0',
                                width: "1rem",
                                fontSize: "0.5rem",
                                margin: "0.5rem auto auto -2.2rem"
                            }} />
                        <ListItemIcon sx={{ color: '#6666ff', marginLeft: "-1rem" }}><VisibilityOutlinedIcon sx={{ fontSize: '1rem' }} /></ListItemIcon>
                        <ListItemText primary={item.views}
                            primaryTypographyProps={{
                                color: '#6666ff',
                                width: "1rem",
                                fontSize: "0.5rem",
                                margin: "0.5rem auto auto -2.2rem"
                            }} />
                        <ListItemIcon sx={{ color: 'gray', marginLeft: "-0.6rem" }}><InsertPhotoOutlinedIcon sx={{ fontSize: '1rem' }} /></ListItemIcon>
                        <ListItemText primary={item.fileCount}
                            primaryTypographyProps={{
                                color: 'gray',
                                width: "1rem",
                                fontSize: "0.5rem",
                                margin: "0.5rem auto auto -2.2rem"
                            }} />

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
다음 MUI 컴포넌트를 사용하였습니다. -> <Box/> <List/> <ListItem/> <ListItemText/> <ListItemIcon/> <Stack/> <br/>
List 관련 컴포넌트 같은 경우, List 안에 ListItem 그 안에 ListItemText, ListItemIcon 컴포넌트를 배치하여 리스트 하나에 보여주고 싶은 내용들로 구성할 수 있습니다.<br/>
그리고 List 안에 post배열은 ListItemText 등의 컴포넌트에 들어갈 데이터가 담긴 배열이므로 map을 통해 모든 리스트의 값을 가져올 수 있습니다.
좋아요, 댓글 등과 같은 아이콘이 필요한 부분은 <a href="https://mui.com/material-ui/material-icons/#main-content">이곳</a> MUI가 제공하는 것을 사용하였습니다.

# 리팩토링이 필요한 부분
아무래도 편리한 style작성을 위해 makeStyles()과 필요한 코드 줄에 style을 적용하니 코드가 길어지고 가독성이 떨어지는 것을 느꼈습니다. 별도의 파일로 작성하여 코드를 정리하는 보완이 필요할 것 같습니다.

# BoardDetail.jsx
**ModalContainer.jsx**
<br/>
```

```
<br/>
# ModalContainer.jsx
**Modal**
<br/>
```

```


# 어려웠던 점
디자인적인 부분을 생각하며 그것을 코드로 옮기고 그대로 생각하는 것처럼 화면에 반영되지 않는 것이 어려웠습니다. 또한 화면 해상도을 고려하여 반응형으로 CSS를 작성하는 것이 쉽지 않았던 것 같습니다.
<br/><br/>
# 마무리
MUI 컴포넌트를 사용하면서 수고와 시간비용을 절약할 수 있어, 유용하게 사용하였습니다. 
<br/>하지만 그저 MUI의 오픈 소스 코드를 재사용하는 것이 아닌, 어떻게 내 프로젝트에 맞게 커스텀하고 응용하며 어떻게 내 코드에 녹일 것인지가 중요하다고 생각했기 때문에 그 부분을 고민하며 개발을 진행했던 것 같습니다.
<br/>다음 프로젝트엔 Bootstrap, Ant Design, semantic UI 더 다양한 UI 라이브러리를 가지고 커스텀해보고 장단점을 비교하며 
<h5>:page_with_curl: Acknowledgments</h5>
- <a href="https://mui.com/">MUI</a>



