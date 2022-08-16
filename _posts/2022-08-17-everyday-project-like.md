---
layout: single
title:  "[에브리데이] 좋아요 기능 구현"

categories:
  - Team Project
tags:
  - 좋아요 버튼
  - react

published: true
---

<br/>
안녕하세요.
<br/>오늘은 [에브리데이] 프로젝트에서 구현했던 좋아요 기능을 포스팅해보려고 합니다.
<br/><br/>저는 게시글과 댓글 그리고 대댓글에 좋아요 아이콘을 생성하여 클릭 시, 좋아요를 할 수 있도록 기능을 구현하였고, 서버에게도 해당 데이터를 전달할 수 있도록 API를 연동하였습니다.
<br/><br/>
**개발 환경**
<br/>
**Front-End** 
<br/>
React
<br/>
**Back-End**
<br/>
Spring Boot

<br/>
# 구현 화면
<br/>
![everyday_like_20220817](https://user-images.githubusercontent.com/84834172/184922670-f7fb3733-c61e-49a2-880f-65323ad79084.gif)

<br/>

아래의 코드는 댓글의 좋아요 기능을 구현한 것입니다.
<br/><br/>isLikeComment, likeCount의 변수는 이전에 게시글 상세 조회 api에서 서버로부터 가져온 값으로, isLikeComment는 사용자가 댓글을 좋아요했는지에 대한 상태값이며(Y 또는 N)
likeCount는 해당 댓글의 총 좋아요 개수입니다.
<br/><br/>먼저 서버로부터 받은 isLikeComment, likeCount 변수를 아래와 같이 useState로 다시 선언하고 저장하였습니다.
```javascript 
const [isLikeComment, setIsLikeComment] = useState(comment.isLikeComment);  //comment.isLikeComment의 comment는 게시글 상세 조회 api에서 받은 댓글정보를 담은 배열
const [likeCount, setLikeCount] = useState(comment.likeCount); 
```
<br/>
그리고 좋아요 아이콘을 클릭 시, isLikeComment가 N이면 likeCount가 +1하도록, isLikeComment가 Y이면 likeCount가 -1하도록 분기처리를 하였습니다. 또한 isLikeComment의 상태값
에 따른 api 요청코드도 작성하였습니다.
<br/>따라서, isLikeComment의 값이 좋아요 아이콘을 누를때마다 Y→N→Y→N 으로 바뀌게 되어 좋아요했다가 좋아요를 취소하는 것을 반복하는 것도 가능해집니다.
<br/>
# Comment.js
  
```javascript
import React, { useState } from "react";
import ReplyList from "./ReplyList";

import { Stack, Button, Divider, Box } from "@mui/material";
import AccountCircleIcon from '@mui/icons-material/AccountCircle';
import FavoriteOutlinedIcon from '@mui/icons-material/FavoriteOutlined';                //색이채워진좋아요
import FavoriteBorderOutlinedIcon from '@mui/icons-material/FavoriteBorderOutlined';    //좋아요

import "@toast-ui/editor/dist/toastui-editor.css";
import Markdown from "../../Markdown";

import * as BoardAPI from '../../../api/Board';
import { Message } from '../../Message';
import { SESSION_TOKEN_KEY } from '../../Axios/Axios';
import {
  displayDateFormat,
  Item,
} from "../../CommentTool";

const Comment = (props) => {
  const {
    comment,
    postId,
    handleIsInitialize,
  } = props;
  const index = comment.commentId;
  
  const token = localStorage.getItem(SESSION_TOKEN_KEY);
  const tokenJson = JSON.parse(atob(token.split(".")[1]));
  
  const [isLikeComment, setIsLikeComment] = useState(comment.isLikeComment);
  const [likeCount, setLikeCount] = useState(comment.likeCount);

  //댓글 삭제(db)
  const deleteComment = (commentId) => {
    BoardAPI.deleteComment(commentId).then(response => {
      Message.success(response.message);
      handleIsInitialize(false);
    }).catch(error => {
      console.log(JSON.stringify(error));
      Message.error(error.message);
    })
  };
  
  //좋아요 api
  const clickLike = () => {
    setIsLikeComment(!isLikeComment);
    const data = {
      targetType: "COMMENT",
      targetId: comment.commentId
    };
    if (!isLikeComment) {
      setLikeCount(Number(likeCount) + 1);
      BoardAPI.like(data).then(response => {
        Message.success(response.message);
      }).catch(error => {
        console.log(JSON.stringify(error));
        Message.error(error.message);
      })
    } else {
      setLikeCount(Number(likeCount) - 1);
      BoardAPI.likeCancel(data).then(response => {
        Message.success(response.message);
      }).catch(error => {
        console.log(JSON.stringify(error));
        Message.error(error.message);
      })
    }
  }

  return (
    <>
      <Box sx={{ mt: 2, mb: 2 }} key={index}>
        {/* writer 정보, 작성 시간 */}
        <Stack direction="row" spacing={2}>
          <AccountCircleIcon sx={{ color: 'gray', mt: 0.5, mr: -1 }} ></AccountCircleIcon>
          <Item style={{ color: 'black' }}>{comment.commentWriter}</Item>
          <Item sx={{ fontSize: '0.5rem' }}>{displayDateFormat(comment.commentRegistrationDate)}</Item>

          {
            (!isLikeComment) ?
              <FavoriteBorderOutlinedIcon sx={{ fontSize: '1rem', color: '#C00000', cursor: 'pointer' }} onClick={() => clickLike()} />
              :
              <FavoriteOutlinedIcon sx={{ fontSize: '1rem', color: '#C00000', cursor: 'pointer' }} onClick={() => clickLike()} />
          }
          <span style={{ marginLeft: "0.2rem", marginTop: "0.2rem", fontSize: "0.7rem", color: '#C00000', cursor: 'pointer' }}
            onClick={() => clickLike()}>
            {likeCount}
          </span>
        </Stack>

        {/* comment 글 내용 */}
        <Box
          key={index}
          sx={{ padding: "0px 20px", color: comment.exist || "grey", ml: 1.6, }}
        >
          <Markdown comment={comment} />
        </Box>

        {/* comment 삭제 */}
        {tokenJson.sub === comment.writerLoginId && (
          <>
            <Button sx={{ ml: 1.8, fontSize:'0.8rem', color:'gray' }}
              onClick={() => {
                deleteComment(comment.commentId);
              }}
            >
              삭제
            </Button>
          </>
        )}

        {/* 대댓글 컴포넌트 */}
        <ReplyList commentId={comment.commentId} postId={postId} />

        <Divider variant="middle" />
      </Box>

    </>
  );
};

export default Comment;
```
<br/>
<br/>

# 마무리

좋아요 기능을 구현하는 다양한 방법이 있지만,<br/>
저는 좋아요를 클릭 시, 화면단에서 먼저 색상이 바뀌고 숫자가 바로 바뀔 수 있도록 개발하였습니다. 그 동시에 API요청 코드도 함께 작성하여 API 통신 후 DB에 반영된 값은 다시 렌더링
했을때 볼 수 있도록 하였습니다. API를 연동하여 좋아요 기능을 구현해봄으로써 state에 대한 이해를 더 높일 수 있었고 전보다 state를 사용하는 것이 더욱 수월해짐을 느꼈던 것 같습니다.


<br/><br/> <a href="https://github.com/ram-yeon/everyday">→ [에브리데이] 프로젝트 GitHub 보러가기</a>
<h5>:page_with_curl: Acknowledgments</h5>
- <a href="https://velog.io/@estell/React-%EC%A2%8B%EC%95%84%EC%9A%94-%EB%B2%84%ED%8A%BC-%EA%B8%B0%EB%8A%A5-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0-omCilck-%EB%B2%84%ED%8A%BC-%EB%88%84%EB%A5%B4%EB%A9%B4-1-%EC%A6%9D%EA%B0%80">좋아요 버튼 기능 만들기</a>



