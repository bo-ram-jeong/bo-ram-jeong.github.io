---
layout: single
title:  "[에브리데이] 게시글 등록 / 조회 / 수정 / 삭제 API 개발"

categories:
  - Team Project
tags:
  - CRUD
  - API
  - 게시글
  - 게시글 등록
  - 게시글 수정
  - 게시글 삭제
  - react

published: true
---

<br/>
안녕하세요.
<br/><br/>오늘은 [에브리데이] 프로젝트에서 게시글의 등록부터 조회, 수정, 삭제까지 API를 연동하여 개발한 부분을 바로 포스팅해보려고 합니다.
<br/><br/>
# BoardList.js
```javascript
import React, { useEffect, useState } from 'react'
import { useNavigate } from 'react-router-dom';
import { makeStyles } from "@material-ui/core";
import WriteBox from './WriteBox';

import { Box } from '@mui/material/';
import BorderColorIcon from '@mui/icons-material/BorderColor';
import FavoriteBorderOutlinedIcon from '@mui/icons-material/FavoriteBorderOutlined';    //좋아요
import TextsmsOutlinedIcon from '@mui/icons-material/TextsmsOutlined';                  //댓글
import VisibilityOutlinedIcon from '@mui/icons-material/VisibilityOutlined';            //조회수
import InsertPhotoOutlinedIcon from '@mui/icons-material/InsertPhotoOutlined';          //사진첨부

import List from '@mui/material/List';
import ListItem from '@mui/material/ListItem';
import ListItemText from '@mui/material/ListItemText';
import ListItemIcon from '@mui/material/ListItemIcon';

import Stack from '@mui/material/Stack';

import { displayDateFormat } from "../CommentTool";
import Pagination from '@mui/material/Pagination';
import * as BoardAPI from '../../api/Board';
import { Message } from '../../component/Message';
import { SESSION_TOKEN_KEY } from '../../component/Axios/Axios';

const useStyles = makeStyles((theme) => ({
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
    const boardTypeToLowerCase = boardType.toLowerCase();
    const classes = useStyles();
    const navigate = useNavigate();

    let token = localStorage.getItem(SESSION_TOKEN_KEY);
    const tokenJson = JSON.parse(atob(token.split(".")[1]));

    const [post, setPost] = useState([]);
    const [page, setPage] = useState(1);
    const [totalPages, setTotalPages] = useState(1);
    const [isInitialize, setIsInitialize] = useState(false);
    const [show, setShow] = useState(false);

    const handleIsInitialize = (value) => {
        setIsInitialize(value);
    }
    const handleWriteBoxShow = (value) => {
        setShow(value);
    }
    const handleChange = (event, value) => {
        setPage(value);

        getBoardList({
            boardType: boardType,
            page: value - 1,
        });
    };

    const getBoardList = (apiRequestData) => {
        if (tokenJson.account_authority === "USER") {
            //게시판 별 게시글 목록 조회
            BoardAPI.eachBoardSelect(apiRequestData).then(response => {
                if (response.data.hasOwnProperty('content')) {
                    const postItems = [];

                    response.data.content.forEach((v, i) => {
                        const title = JSON.stringify(v.title).replaceAll("\"", "");
                        const contents = (v.contents).replaceAll("\"", "");
                        const registrationDate = displayDateFormat(JSON.stringify(v.registrationDate).replaceAll("\"", ""));
                        const writer = JSON.stringify(v.writer).replaceAll("\"", "");
                        const likeCount = JSON.stringify(v.likeCount);
                        const commentCount = JSON.stringify(v.commentCount);
                        const views = JSON.stringify(v.views);
                        const fileCount = JSON.stringify(v.fileCount);

                        postItems.push({
                            user: writer, postTitle: title, postContent: contents, date: registrationDate,
                            likeCount: likeCount, commentCount: commentCount, fileCount: fileCount, views: views, id: v.id
                        });
                    })
                    setPost(postItems);
                }
                if (response.data.hasOwnProperty('totalPages')) {
                    setTotalPages(response.data.totalPages);
                }
            }).catch(error => {
                console.log(JSON.stringify(error));
                Message.error(error.message);
            }).finally(() => {
                handleIsInitialize(true);
            });
        }
    };

    useEffect(() => {
        if (!isInitialize) {
            getBoardList({
                boardType: boardType,
                page: 0,
            });
        }
    });
   
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
# WriteBox.jsx
```javascript
//글작성 박스
import React, { useState } from 'react'
import { makeStyles } from "@material-ui/core";
import { Box, TextField } from '@mui/material/';
import BorderColorIcon from '@mui/icons-material/BorderColor';
import InsertPhotoOutlinedIcon from '@mui/icons-material/InsertPhotoOutlined';
import { FormControlLabel, Checkbox } from '@mui/material';

import * as BoardAPI from '../../api/Board';
import { Message } from '../../component/Message';
import { SESSION_TOKEN_KEY } from '../../component/Axios/Axios';

// import Carousel from 'react-bootstrap/Carousel'
import 'bootstrap/dist/css/bootstrap.min.css';
// import axios from 'axios';

const useStyles = makeStyles((theme) => ({
    writeBox: {
        height: "auto",
        border: "2px lightgray solid",
        color: "gray",
        backgroundColor: "#00000",
        fontSize: "0.9rem",
        textAlign: "left",
        margin: "0.3rem auto",
    },
    writeContents: {
        height: "30vh",
        width: "98%",
        marginTop: "1rem",
        borderColor: "gray",
        borderRadius: "0.3rem",
    },
    boxFooter: {
        height: "1.7rem",
    },
    registerBtn: {
        cursor: "pointer",
        padding: "0.3rem",
        backgroundColor: "#C00000",
        color: "white",
        float: "right",
    },
}));

function WriteBox(props) {
    const {
        boardType,
        handleIsInitialize,
        handleWriteBoxShow,
    } = props;
    const classes = useStyles();
    const [title, setTitle] = useState("");
    const [contents, setContents] = useState("");
    const [checked, setChecked] = useState(false);

    const token = localStorage.getItem(SESSION_TOKEN_KEY);
    const tokenJson = JSON.parse(atob(token.split(".")[1]));

    const handleSetTitle = (e) => {
        setTitle(e.target.value);
    };
    const handleSetContents = (e) => {
        setContents(e.target.value);
    };
    const handleCheckBox = (event) => {
        setChecked(event.target.checked);
    };

    const [imgBase64, setImgBase64] = useState([]); // 파일 base64(미리보기)
    const [imgFile, setImgFile] = useState(0);	//파일	
    const handleChangeFile = (event) => {
        // console.log(event.target.files)
        setImgFile(event.target.files);
        setImgBase64([]);
        for (let i = 0; i < event.target.files.length; i++) {
            if (event.target.files[i]) {
                const reader = new FileReader();
                reader.readAsDataURL(event.target.files[i]); // 1)파일을 읽어 버퍼에 저장
                // 파일 상태 업데이트
                reader.onloadend = () => {
                    // 2)읽기가 완료되면 아래코드 실행
                    const base64 = reader.result;
                    if (base64) {
                        const base64Sub = base64.toString()
                        setImgBase64(imgBase64 => [...imgBase64, base64Sub]);
                        //  setImgBase64(newObj);
                        // 파일 base64 상태 업데이트
                        //  console.log(images)
                    }
                }
            }
        }
    }

    //글등록(파일포함)
    // const handleRegister = (e) => {
    //     const formData = new FormData();
    //     // Object.values(imgFile).forEach((file) => formData.append("file", file));
    //     for (let i = 0; i < imgFile.length; i++) {
    //         formData.append("file", imgFile[i])
    //     }

    //     let isAnonymous = '';
    //     if (checked) {
    //         isAnonymous = 'Y'
    //     } else {
    //         isAnonymous = 'N'
    //     }
    //     const data = {
    //         boardType: boardType,
    //         title: title,
    //         contents: contents,
    //         isAnonymous: isAnonymous,
    //     }

    //     // formData.append("data", new Blob([JSON.stringify(data)] , {type: "application/json"}))
    //     formData.append("data", JSON.stringify(data))

    //     if (boardType === '공지사항') {   //관리자 공지등록
    //         BoardAPI.registerBoardByAdmin(formData).then(response => {
    //             Message.success(response.message);
    //             handleIsInitialize(false);
    //         }).catch(error => {
    //             console.log(JSON.stringify(error));
    //             Message.error(error.message);
    //         })

    //     } else {    //일반사용자 글등록
    //         BoardAPI.registerBoard(formData).then(response => {
    //             Message.success(response.message);
    //             handleIsInitialize(false);
    //         }).catch(error => {
    //             console.log(JSON.stringify(error));
    //             Message.error(error.message);
    //         })
    //     }
    //     // console.log(Object.fromEntries(formData));
    // };

    //글등록(파일제외)
    const handleRegister = (e) => {
        if (!title || !contents) { //입력값체크
            alert("정확하게 입력하였는지 확인해주세요.");
        } else {
            let isAnonymous = '';
            if (checked) {
                isAnonymous = 'Y'
            } else {
                isAnonymous = 'N'
            }
            const data = {
                boardType: boardType,
                title: title,
                contents: contents,
                isAnonymous: isAnonymous,
            }
            if (boardType === '공지사항') {   //관리자 공지등록
                BoardAPI.registerBoardByAdmin(data).then(response => {
                    Message.success(response.message);
                    handleIsInitialize(false);
                    handleWriteBoxShow(false);
                }).catch(error => {
                    console.log(JSON.stringify(error));
                    Message.error(error.message);
                })
            } else {    //일반사용자 글등록
                BoardAPI.registerBoard(data).then(response => {
                    Message.success(response.message);
                    handleIsInitialize(false);
                    handleWriteBoxShow(false);
                }).catch(error => {
                    console.log(JSON.stringify(error));
                    Message.error(error.message);
                })
            }
        }
    };

    return (
        <>
            <Box p={1.8} className={classes.writeBox}>
                <div>
                    <TextField id="standard-basic" label="글 제목" variant="standard" sx={{ width: "100%" }}
                        value={title} onChange={(e) => handleSetTitle(e)} />
                </div>
                <div>
                    <textarea
                        // onKeyUp={checkLength}
                        style={{ padding: "1rem", width: "99%", height: "26vh", fontSize: "0.9rem", fontFamily: "-moz-initial", margin: "1rem 0.3rem", resize: "none", whiteSpace: "pre-line" }}
                        placeholder="에브리타임은 누구나 기분 좋게 참여할 수 있는 커뮤니티를 만들기 위해 커뮤니티 이용규칙을 제정하여 운영하고 있습니다. .... "
                        value={contents}
                        onChange={(e) => handleSetContents(e)}
                    />
                </div>

                {/* 파일첨부 시 미리보기 */}
                <div>
                    {/* <Carousel> */}
                    {imgBase64.map((item) => {
                        return (
                            // <Carousel.Item>
                            <img
                                src={item}
                                alt="첨부된 이미지"
                                style={{ width: "100px", height: "100px", margin: "0.2rem" }}
                            />
                            // </Carousel.Item>
                        )
                    })}
                    {/* </Carousel> */}
                    {/* <UploadFile /> */}
                </div>

                <hr style={{ marginBottom: "0.3rem" }} />

                <div className={classes.boxFooter}>
                    <div>
                        <input
                            type="file"
                            id="file"
                            style={{ display: 'none' }}
                            onChange={handleChangeFile}
                            multiple
                            accept="image/png, image/jpeg, image/jpg"
                        />
                        <label htmlFor="file"><InsertPhotoOutlinedIcon sx={{ cursor: 'pointer', fontSize: '2rem' }} /></label>
                    </div>
                    {
                        (tokenJson.account_authority === 'USER') &&
                        <FormControlLabel
                            control={<Checkbox color="default" size="small" />}
                            label="익명"
                            sx={{ margin: "-3.5rem auto auto 87%" }}
                            checked={checked}
                            onChange={handleCheckBox} />
                    }
                    <BorderColorIcon className={classes.registerBtn} onClick={handleRegister} sx={{ fontSize: '2.5rem', marginTop: '-2.1rem' }} />
                </div>
            </Box>
        </>
    )
}

export default WriteBox
```
# 코드
BoardList.jsx에 글을 작성하고 등록할 수 있도록 하는 WriteBox 컴포넌트를 추가하였고 WriteBox에서는 글 작성 후 작성버튼을 눌렀을 때, 
BoardAPI를 통해 게시판 타입, 제목, 내용, 익명 체크 여부 데이터들을 서버로 보낼 수 있도록 개발하였습니다. 

<br/>이 때, 관리자의 글 작성인지, 사용자의 글 작성인지 boardType으로 구분하여 각각의 api로 요청할 수 있도록 하였습니다.

<br/>
# BoardDetail.jsx
```javascript
import React, { useState, useEffect } from 'react';
import EditBox from './EditBox';
import CommentList from './Comment/CommentList';
import { Link, useNavigate, useLocation } from 'react-router-dom';

import { makeStyles, Typography } from "@material-ui/core";
import { Box } from '@mui/material/';
import AccountCircleIcon from '@mui/icons-material/AccountCircle';
import FavoriteOutlinedIcon from '@mui/icons-material/FavoriteOutlined';                //채워진좋아요
import FavoriteBorderOutlinedIcon from '@mui/icons-material/FavoriteBorderOutlined';    //좋아요
import TextsmsOutlinedIcon from '@mui/icons-material/TextsmsOutlined';                  //댓글
import VisibilityOutlinedIcon from '@mui/icons-material/VisibilityOutlined';            //조회수
import InsertPhotoOutlinedIcon from '@mui/icons-material/InsertPhotoOutlined';          //사진첨부

import { displayDateFormat } from "../CommentTool";
import * as BoardAPI from '../../api/Board';
import { Message } from '../../component/Message';
import { SESSION_TOKEN_KEY } from '../../component/Axios/Axios';

const useStyles = makeStyles((theme) => ({
    headLink: {
        textDecoration: 'none',
        cursor: 'pointer',
        color: 'black',
        "&:hover": {
            color: '#C00000',
        }
    },
    .
    .
    .
}));

function BoardDetail() {
    const location = useLocation();
    const postId = location.state.postId;
    const headTitle = location.state.headTitle;
    
    const navigate = useNavigate();
    const classes = useStyles();
    
    const token = localStorage.getItem(SESSION_TOKEN_KEY);
    const tokenJson = JSON.parse(atob(token.split(".")[1]));

    const [boardType, setBoardType] = useState('');
    const [boardTypeToLowerCase, setBoardTypeToLowerCase] = useState('');
    const [writer, setWriter] = useState('');
    const [writerLoginId, setWriterLoginId] = useState('');
    const [id, setId] = useState('');
    const [likeCount, setLikeCount] = useState('');
    const [likeState, setLikeState] = useState('');

    const [commentCount, setCommentCount] = useState('');
    const [views, setViews] = useState('');
    const [fileCount, setFileCount] = useState('');
    const [title, setTitle] = useState('');
    const [contents, setContents] = useState('');
    const [registrationDate, setRegistrationDate] = useState('');

    const [comment, setComment] = useState([]);
    const [isInitialize, setIsInitialize] = useState(false);

    const handleIsInitialize = (value) => {
        setIsInitialize(value);
    }

    const data = {
        postId: postId,
    }
    //게시글상세조회api
    const boardDetailSelect = async () => {
        BoardAPI.boardDetailSelect(data).then(response => {
            // file 처리필요
            // if (response.data.hasOwnProperty('file')) {
            // }
            const boardType = response.data.boardType;
            setBoardType(JSON.stringify(response.data.boardType).replaceAll("\"", ""));
            setBoardTypeToLowerCase(boardType.toLowerCase());
            setTitle(JSON.stringify(response.data.title).replaceAll("\"", ""));
            setContents((response.data.contents).replaceAll("\"", ""));
            setRegistrationDate(response.data.registrationDate);
            setWriter(JSON.stringify(response.data.writer).replaceAll("\"", ""));
            setWriterLoginId(JSON.stringify(response.data.writerLoginId).replaceAll("\"", ""));
            setId(JSON.stringify(response.data.id));
            setLikeCount(Number(response.data.likeCount));
            let isLikePost = JSON.stringify(response.data.isLikePost).replaceAll("\"", ""); //해당 게시글 좋아요했는지에 대한 상태값  
            if (isLikePost === "Y") {
                setLikeState(true);
            } else {
                setLikeState(false);
            }
            setCommentCount(JSON.stringify(response.data.commentCount));
            setViews(JSON.stringify(response.data.views));
            setFileCount(JSON.stringify(response.data.fileCount));
        })
    }

    //댓글상세조회api
      .
      .
      .
    }

    useEffect(() => {
        if (!isInitialize) {
            if (tokenJson.account_authority === "USER") {
                Promise.all([boardDetailSelect(), boardCommentSelect()]).then(() => {
                    handleIsInitialize(true);
                }).catch(error => { console.log(error) })
            }
        }
    });
    .
    .
    .
    const [displayEditBox, setDisplayEditBox] = useState(false);
    //게시글 수정 버튼
    const editPost = (value) => {
        if (tokenJson.sub === writerLoginId) {
            setDisplayEditBox(value);
        }
    }
    //게시글 삭제
    const deletePost = () => {
        if (tokenJson.sub === writerLoginId) {
            const data = {
                postId: id,
            }
            BoardAPI.deleteBoard(data).then(response => {
                Message.success(response.message);
                navigate('/' + boardTypeToLowerCase + 'board');
            }).catch(error => {
                console.log(JSON.stringify(error));
                Message.error(error.message);
            })
        } else {
            alert("삭제할 권한이 없습니다.");
        }
    }

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
```
# 코드
BoardDetail.jsx에서 useEffect를 통해 boardDetailSelect() 함수를 호출하여 서버로 게시글 상세를 조회하는 api를 요청하였고, 본인의 글일 경우, 삭제 버튼이 활성화 되는데 이 떄, 
삭제 버튼을 클릭 시 글이 삭제될 수 있도록 삭제 api 요청 후 재렌더링하여 삭제된 것을 볼 수 있도록 개발하였습니다.
<br/>
# EditBox.jsx
```javascript
//게시글 수정 박스
import React, { useState } from 'react'
import { makeStyles } from "@material-ui/core";
import { Box, TextField } from '@mui/material/';
import BorderColorIcon from '@mui/icons-material/BorderColor';
import { FormControlLabel, Checkbox } from '@mui/material';

import * as BoardAPI from '../../api/Board';
import { Message } from '../Message';
import { SESSION_TOKEN_KEY } from '../Axios/Axios';

const useStyles = makeStyles((theme) => ({
    writeBox: {
        height: "auto",
        border: "2px lightgray solid",
        color: "gray",
        backgroundColor: "#00000",
        fontSize: "0.9rem",
        textAlign: "left",
        margin: "0.3rem auto",
    },
    .
    .
    .
}));

function EditBox(props) {
    const {
        boardType,
        postId,
        writtenTitle,
        writtenContents,
        editPost,
        handleIsInitialize,
    } = props;

    const classes = useStyles();
    const [title, setTitle] = useState(writtenTitle);
    const [contents, setContents] = useState(writtenContents);
    const [checked, setChecked] = useState(false);

    const token = localStorage.getItem(SESSION_TOKEN_KEY);
    const tokenJson = JSON.parse(atob(token.split(".")[1]));

    const handleSetTitle = (e) => {
        setTitle(e.target.value);
    };
    const handleSetContents = (e) => {
        setContents(e.target.value);
    };
    const handleCheckBox = (event) => {
        setChecked(event.target.checked);
    };

    //글수정(파일제외)
    const handleUpdate = (e) => {
        if (!title || !contents) { //입력값체크
            alert("정확하게 입력하였는지 확인해주세요.");
        } else {
            let isAnonymous = '';
            if (checked) {
                isAnonymous = 'Y'
            } else {
                isAnonymous = 'N'
            }
            const data = {
                boardType: boardType,
                title: title,
                contents: contents,
                isAnonymous: isAnonymous,
            }
            if (boardType === 'NOTICE') {   //관리자 공지수정
                BoardAPI.updateBoardByAdmin(postId, data).then(response => {
                    Message.success(response.message);
                    editPost(false);
                    handleIsInitialize(false);
                }).catch(error => {
                    console.log(JSON.stringify(error));
                    Message.error(error.message);
                })
            } else {    //일반사용자 글수정
                BoardAPI.updateBoard(postId, data).then(response => {
                    Message.success(response.message);
                    editPost(false);
                    handleIsInitialize(false);
                }).catch(error => {
                    console.log(JSON.stringify(error));
                    Message.error(error.message);
                })
            }
        }
    };

    return (
        <div>
            <Box p={1.8} className={classes.writeBox}>
                <div>
                    <TextField id="standard-basic" label="글 제목" variant="standard" sx={{ width: "100%" }}
                        value={title} onChange={(e) => handleSetTitle(e)} />
                </div>
                <div>
                    <textarea
                        // onKeyUp={checkLength}
                        style={{ padding: "1rem", width: "100%", height: "26vh", fontSize: "0.9rem", fontFamily: "-moz-initial", marginTop: "1rem", resize: "none", whiteSpace: "pre-line" }}
                        value={contents}
                        onChange={(e) => handleSetContents(e)}
                    />
                </div>

                <hr style={{ marginBottom: "0.3rem" }} />

                <div className={classes.boxFooter}>
                    {
                        (tokenJson.account_authority === 'USER') &&
                        <FormControlLabel
                            control={<Checkbox color="default" size="small" />}
                            label="익명"
                            sx={{ margin: "auto auto auto 87%" }}
                            checked={checked}
                            onChange={handleCheckBox} />
                    }
                    <BorderColorIcon className={classes.registerBtn} onClick={handleUpdate} sx={{ fontSize: '2.5rem', marginTop: '-0.1rem' }} />
                </div>
            </Box>
            <button className={classes.listBtn} onClick={() => editPost(false)} >수정 취소</button>
        </div>
    )
}

export default EditBox
```
# 코드
BoardDtail.jsx에 본인의 글일 경우 삭제버튼과 함께 수정버튼도 함께 뜨게 되는데, 이 때 수정 버튼을 누르면 EditBox 컴포넌트가 노출되면서 글을 수정할 수 있도록 개발하였습니다.
그리고 게시글 등록과 마찬가지로 수정완료 아이콘(BorderColorIcon)의 버튼을 누르게 되면 BoardAPI를 통해 게시판 타입, 제목, 내용, 익명 체크 여부 데이터들을 서버로 보내어 재렌더링 후,
수정된 글을 볼 수 있도록 개발하였습니다.

<br/>
# 어려웠던 점
한 페이지에 들어가는 모든 내용을 한 파일에 담는 것이 아니라 어떻게 하면 컴포넌트로 잘 나눌 수 있을지, 어떻게 하면 효율적으로 '컴포넌트화' 하여 재사용성을 높일 수 있는지 고민하는 
것이 어려웠던 것 같고, 에브리타임 사이트의 카피사이트이기 때문에 최대한 UI가 같도록 개발해야했기 때문에 그 부분을 모두 생각하면서 개발하는 것이 어려웠던 것 같습니다.
<br/><br/>그리고 점점 프로젝트의 기능들이 추가되면서 코드들이 많아지고 복잡해지다 보니, 먼저 어느 기능부터 모듈화하고 공통화를 해야하는지 어려웠던 것 같습니다. 그래서 
**글 작성(등록) 박스**와 **글 수정 박스**를 컴포넌트화 하기 위해 먼저 따로 생성하였고 이 컴포넌트들을 각각 **게시글 목록, 게시글 상세** 파일에서 컴포넌트 역할을하도록 추가하고 나니, 
그 뒤에 부분들이 수월해졌던 것 같습니다.

<br/>
# 보완하고 싶은 부분
아무래도 끝내 성공하지 못한 파일첨부 기능이 될 것 같습니다.
<br/><br/>WriteBox.jsx에서 주석된 코드가 모두 파일 첨부 관련한 코드인데요, 등록 시, FormData에 이미지(파일)의 데이터들을 담아 서버로 전송하였으나 
‘Required request part 'data' is not present’ 와 같은 에러 메시지가 떠서 에러 메시지 해결을 찾아보다 콘솔을 확인해보니 FormData에 값이 null인 것을 발견하였습니다.<br/><br/>
파일첨부 관련한 여러 블로그와 문서를 찾아보았지만 현재 저의 코드에서는 별다른 이상을 찾지 못했고 이 오류를 해결하기 위해 2주의 시간을 들였지만 기간 내에 해결하지 못하였습니다. 
FormData 에 담을 이미지 데이터를 콘솔로 확인해보았을 땐, 해당 데이터는 정상적으로 출력되지만 FormData 안에 넣어 서버로 보내면 값이 null이 뜨게 됩니다. FormData 객체에 문제가 있거나
Axios 통신에서 어떠한 오류가 있는 것으로 유추해보아 이 부분을 더 찾아보고 해결해 나갈 예정입니다.

<br/>
# 마무리
게시글의 등록부터 수정, 삭제까지 모두 API 연결하여 개발해보면서 다양한 에러들과 많은 고민들을 겪었던 것 같습니다.
<br/><br/>예를 들어 게시글 상세 조회 api 요청을 통해 결과 데이터들을 받아오면서 이 데이터들이 배열 값으로 어떻게 저장되어 있는지, 
또 이 데이터들은 어떤 타입으로 값을 받아서 저장해야하는지 등 개발하는 과정에서 많은 시행착오가 있었고 가끔 방법을 몰라 헤맨적이 많았습니다.
<br/><br/>하지만 동시에 해결해나가기 위해 저만의 방법을 찾아보면서 그 문제들을 해결해냈고 끝에는 잘 마무리하였습니다.
<br/>결과적으로 해냈다는 성취감을 느낄 수 있었던 과 뿌듯한 시간이었습니다.


<br/> <a href="https://github.com/ram-yeon/everyday">→ [에브리데이] 프로젝트 GitHub 보러가기</a>
<h5>:page_with_curl: Acknowledgments</h5>
- <a href="https://mui.com/material-ui/material-icons/">Material Icons</a>



