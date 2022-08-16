---
layout: single
title:  "[에브리데이] 게시판 댓글 / 대댓글 구현"

categories:
  - Team Project
tags:
  - comment
  - editor
  - 댓글
  - 대댓글
  - react

published: true
---

<br/>
안녕하세요.
<br/>오늘은 [에브리데이] 프로젝트에서 댓글, 대댓글을 어떻게 구현했는지 포스팅해보려고 합니다.
<br/><br/>먼저 <a href="https://velog.io/@wkahd01/%EB%8C%80%EB%8C%93%EA%B8%80-%EA%B8%B0%EB%8A%A5-%EB%A7%8C%EB%93%A4%EA%B8%B0-React">이곳</a> 블로그가 도움이 많이 되었으며,
일부 코드를 참조하여 저만의 댓글, 대댓글을 완성하게 되었습니다.
<br/><br/>댓글, 대댓글의 컴포넌트를 만들기 위해 저는 ```CommentList``` ```Comment``` ```ReplyList``` ```Reply``` 컴포넌트를 만들었고 
필요에 따라 <a href="https://velog.io/@wkahd01/%EB%8C%80%EB%8C%93%EA%B8%80-%EA%B8%B0%EB%8A%A5-%EB%A7%8C%EB%93%A4%EA%B8%B0-React">이곳</a> 블로그에서 가져온 
```Markdown``` ```CommentTool``` 컴포넌트를 사용하였습니다.

<br/>
# 구현 화면
<br/>

![image](https://user-images.githubusercontent.com/84834172/184888340-22388711-9271-4106-a6a0-88802bde929c.png)
![image](https://user-images.githubusercontent.com/84834172/184888392-b5b8684a-22f4-4a0b-8abf-ad2ef9567870.png)
![image](https://user-images.githubusercontent.com/84834172/184889140-cc9dad56-3aa0-4b08-b128-5a9b6f552e60.png)
![image](https://user-images.githubusercontent.com/84834172/184889186-54e4767f-960d-4062-944d-519ad0c47244.png)

<br/>
Comment 컴포넌트 자체에서는 댓글 안에 들어가는 작성자, 날짜, 내용 등에 대한 정보만 담게 하였고 댓글 전체가 들어가는 리스트(+댓글등록) 부분은 CommentList 컴포넌트에서 
관리하도록 개발하였습니다. 
<br/><br/>마찬가지로 대댓글 또한 Reply 컴포넌트 자체에서는 작성 관련 정보만 담았고 ReplyList 컴포넌트에서는 대댓글 리스트 전체 부분(+대댓글등록)을 담도록 개발하였습니다.
<br/>
따라서 서버에 댓글, 대댓글 등록 api 요청하는 부분은 CommentList.jsx, ReplyList.jsx 에 작성하였고, 댓글, 대댓글 삭제 api 요청은 Comment.jsx, Reply.jsx 파일에 작성하였습니다.
<br/><br/>
댓글과 대댓글을 등록할 때, 등록하는 부분은 Toast-UI Editor를 사용하였는데, 등록 api 요청을 할때, 해당 에디터로 작성한 내용을 보내기 위해 아래 코드와 같이 마크다운으로 변환하여,
데이터로 담아 보냈습니다.
```javascript
//마크다운 변환
  const editorInstance = editorRef.current.getInstance();
  const getContent = editorInstance.getMarkdown();
```
<br/><br/>
# CommentList.js
```javascript
import React, { useEffect, useState, useRef } from "react";
import Comment from './Comment';

import { makeStyles } from "@material-ui/core";
import { FormControlLabel, Checkbox, Paper } from '@mui/material';
import BorderColorIcon from '@mui/icons-material/BorderColor';

import "@toast-ui/editor/dist/toastui-editor.css";
import { Editor } from "@toast-ui/react-editor";

import * as BoardAPI from '../../../api/Board';
import { Message } from '../../Message';

const useStyles = makeStyles((theme) => ({
  commentInputBox: {
    border: "lightgray 1px solid",
    backgroundColor: "#F6F6F6",
    height: "2.5rem",
    padding: "0.5rem",
    width: "95%",
  },
  .
  .
  .
}));

const CommentList = (props) => {
  const {
    comment,
    postId,
    handleIsInitialize,
  } = props;
  
  const classes = useStyles();
  
  const [display, setDisplay] = useState(false);
  const editorRef = useRef();
  const [checked, setChecked] = useState(false);

  const handleCheckBox = (event) => {
    setChecked(event.target.checked);
  };

  //댓글등록
  const onSubmit = (e) => {
    //마크다운 변환
    const editorInstance = editorRef.current.getInstance();
    const getContent = editorInstance.getMarkdown();

    let isAnonymous = '';
    if (checked) {
      isAnonymous = 'Y'
    } else {
      isAnonymous = 'N'
    }
    const data = {
      postId: postId,
      contents: getContent,
      commentType: "COMMENT",
      isAnonymous: isAnonymous,
    };
    //댓글 등록(db)
    BoardAPI.registerComment(data).then(response => {
      Message.success(response.message);
      handleIsInitialize(false);
    }).catch(error => {
      console.log(JSON.stringify(error));
      Message.error(error.message);
    })
    setDisplay(!display);
  };

  return (
    <Paper sx={{ m: 0, p: 2 }}>
      {comment.map((comment, index) => (
        <Comment comment={comment} postId={postId} key={index} handleIsInitialize={handleIsInitialize} />
      ))}

      <div style={{ marginTop: '2rem', marginBottom: '-0.8rem' }}>
        <div
          onClick={() => { setDisplay(!display); }}
          className={classes.commentInputBox}>
          댓글을 입력하려면 클릭하세요.
        </div>
        <FormControlLabel control={<Checkbox color="default" size="small" />}
          label="익명" className={classes.checkAnonymous} sx={{ marginLeft: "86%" }} checked={checked} onChange={handleCheckBox} />
        <BorderColorIcon onClick={onSubmit} className={classes.registerBtn} sx={{ fontSize: "2.5rem" }} />
      </div>

      {display && (
        <>
          <Editor ref={editorRef} initialValue={" "} placeholder={"내용을 입력하세요."} />
        </>
      )}
    </Paper>
  );
};

export default CommentList;
```
<br/>
# Comment.jsx
```javascript
import React, { useState } from "react";
import ReplyList from "./ReplyList";

import AccountCircleIcon from '@mui/icons-material/AccountCircle';
import FavoriteOutlinedIcon from '@mui/icons-material/FavoriteOutlined';                //색채워진좋아요
import FavoriteBorderOutlinedIcon from '@mui/icons-material/FavoriteBorderOutlined';    //좋아요
import { Stack, Button, Divider, Box } from "@mui/material";
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
# ReplyList.jsx
```javascript
import React, { useState, useEffect, useRef } from "react";
import Reply from './Reply';

import { makeStyles } from "@material-ui/core";
import { FormControlLabel, Checkbox } from '@mui/material';
import { Stack, Button } from "@mui/material";

import { Editor } from "@toast-ui/react-editor";
import * as BoardAPI from '../../../api/Board';
import { Message } from '../../Message';

const useStyles = makeStyles((theme) => ({
  .
  .
}));

const ReplyList = (props) => {
  const [reply, setReply] = useState([]);
  const [checked, setChecked] = useState(false);
  // const [initCommentId] = useState(props.commentId);
  const [isInitialize, setIsInitialize] = useState(false);

  const handleCheckBox = (event) => {
    setChecked(event.target.checked);
  };
  const handleIsInitialize = (value) => {
    setIsInitialize(value);
  }

  const classes = useStyles();
  const [display, setDisplay] = useState(false);
  const editorRef = useRef();

  //대댓글등록
  const onSubmit = (e) => {
   
    //마크다운 변환
    const editorInstance = editorRef.current.getInstance();
    const getContent = editorInstance.getMarkdown();

    let isAnonymous = '';
    if (checked) {
      isAnonymous = 'Y'
    } else {
      isAnonymous = 'N'
    }
    const data = {
      postId: props.postId,
      contents: getContent,
      commentType: "REPLY",
      isAnonymous: isAnonymous,
      preId: props.commentId
    };
    //대댓글 등록(db)
    BoardAPI.registerComment(data).then(response => {
      Message.success(response.message);
      handleIsInitialize(false);
    }).catch(error => {
      console.log(JSON.stringify(error));
      Message.error(error.message);
    })
    setDisplay(!display);
  };

  const replyApiData = {
    postId: props.postId,
    commentId: props.commentId,
  }
  useEffect(() => {
    if (!isInitialize) {
      BoardAPI.boardReplySelect(replyApiData).then(response => {
        const replyItems = [];
        response.data.comment.forEach((v, i) => {
          const commentWriter = (JSON.stringify(v.writer).replaceAll("\"", ""));
          const commentContents = ((v.contents).replaceAll("\"", ""));
          const commentRegistrationDate = (v.registrationDate);
          const likeCount = (v.likeCount);
          const isLikeComment = (JSON.stringify(v.isLikeComment).replaceAll("\"", ""));     //해당 댓글 좋아요했는지에 대한 상태값
          const writerLoginId = (v.writerLoginId);
          replyItems.push({
            commentWriter: commentWriter, commentContents: commentContents, commentRegistrationDate: commentRegistrationDate,
            isLikeComment: isLikeComment === 'Y' ? true : false, likeCount: likeCount, writerLoginId: writerLoginId,
            commentId: v.id,
          });
        })
        setReply(replyItems);
        handleIsInitialize(true);
      }).catch(e => {
        setReply([]);
      })
    }
  });

  const clickToRegister = () => {
    setDisplay(!display);
  }

  return (
    <Stack sx={{ m: 1, ml: 5 }}>
      <Button sx={{ display: "flex", justifyContent: "flex-start", width: "8rem", fontSize: "0.8rem" }}
        onClick={() => clickToRegister()}>
        {!display && "+ 댓글 달기"}
        {display && "- 숨기기"}
      </Button>
      {display && (
        <>
          <Editor ref={editorRef} initialValue={" "} placeholder={"내용을 입력하세요."} />
          <div style={{ marginTop: "0.2rem" }} >
            <Button onClick={onSubmit} sx={{ ml: -1 }}>등록</Button>
            <FormControlLabel control={<Checkbox color="default" size="small" />}
              label="익명" className={classes.checkAnonymous} sx={{ marginLeft: "90%" }} checked={checked} onChange={handleCheckBox} />
          </div>
        </>
      )}

      {reply.map(reply => (
        <Reply key={reply} reply={reply} handleIsInitialize={handleIsInitialize} />
      ))}

    </Stack>
  );
};

export default ReplyList;
```
<br/>
# Reply.jsx
```javascript
import React, { useState } from "react";
import AccountCircleIcon from '@mui/icons-material/AccountCircle';
import FavoriteOutlinedIcon from '@mui/icons-material/FavoriteOutlined';                //채워진좋아요
import FavoriteBorderOutlinedIcon from '@mui/icons-material/FavoriteBorderOutlined';    //좋아요
import { Stack, Button, Divider } from "@mui/material";
import { Box } from '@mui/material/';
import Markdown from "../../Markdown";
import * as BoardAPI from '../../../api/Board';
import { Message } from '../../Message';
import { SESSION_TOKEN_KEY } from '../../Axios/Axios';
import {
  displayDateFormat,
  Item,
} from "../../CommentTool";

const Reply = (props) => {
  const {
    reply,
    handleIsInitialize,
  } = props;
  // const index = reply.commentId;
  const token = localStorage.getItem(SESSION_TOKEN_KEY);
  const tokenJson = JSON.parse(atob(token.split(".")[1]));

  const [isLikeComment, setIsLikeComment] = useState(reply.isLikeComment);
  const [likeCount, setLikeCount] = useState(reply.likeCount);

  //대댓글 삭제(db)
  const deleteComment = (commentId) => {
    BoardAPI.deleteComment(commentId).then(response => {
      Message.success(response.message);
      handleIsInitialize(false);
    }).catch(error => {
      console.log(JSON.stringify(error));
      Message.error(error.message);
    })
  };
 
  return (
    <>
      <Box sx={{ m: 2 }} >
        <Divider variant="middle" sx={{ mb: 2 }} />
        {/* writer 정보, 작성 시간 */}
        <Stack direction="row" spacing={2}>
          <AccountCircleIcon sx={{ color: 'gray', mt: 0.5, mr: -1 }} ></AccountCircleIcon>
          <Item style={{ color: 'black' }}>{reply.commentWriter}</Item>
          <Item sx={{ fontSize: '0.5rem' }}>{displayDateFormat(reply.commentRegistrationDate)}</Item>

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
          sx={{ padding: "0px 20px", color: reply.exist ?? "grey", ml: 1.6 }}
        >
          <Markdown comment={reply} />
        </Box>

        {/* comment 삭제 */}
        {tokenJson.sub === reply.writerLoginId && (
          <>
            <Button sx={{ ml: 1.8, fontSize: '0.8rem', color: 'gray' }}
              onClick={() => {
                deleteComment(reply.commentId);
              }}
            >
              삭제
            </Button>
          </>
        )}
        {/* 대댓글 컴포넌트 -> 주석처리(대댓글까지만 구현) */}
        {/* <ReplyList commentId={comment.commentId}  /> */}

      </Box>

    </>
  );
};

export default Reply;
```
<br/>
<br/>
# 어려웠던 점
Toast-UI Editor를 사용하는 것이 어려웠습니다. 
<br/>Toast-UI 에디터를 처음 사용해보았는데, 처음 사용하다보니 사용하는데 있어 서툴렀던 부분이 많은 것 같아 아쉬웠던 것 같습니다.
또한 에디터 자체에서 제공하는 기능들이 많아 이 부분들을 사용할 수 있을 때는 유용했지만, 반대로 예를 들어 파일 첨부 같은 경우, 파일을 댓글(대댓글)로 등록했을 때 처리해줘야 하는 
부분을 구현할 수 없어서 이 부분이 오히려 완성도를 떨어뜨렸던 것 같습니다.

<br/>
# 보완이 필요한 부분
따라서 보완을 한다면 Toast-UI Editor를 사용하여 글을 등록하는 부분을 보완하고 싶습니다. 만약 파일을 첨부하는 부분을 구현하지 못한다면 아예 파일을 첨부할 수 없도록 기능을 막거나
아니면 파일 첨부하는 부분을 더 공부한 후, 구현하여 구현의 완성도를 높이고 에디터로서의 기능을 유용하게 사용할 수 있도록 개발하고 싶습니다.

<br/>
# 마무리
기능적인 부분이나 UI적인 부분이 다소 미흡하고 아쉬운 부분이 많아 이 부분은 리팩토링을 통해 더 보완해나갈 예정입니다. 그래도 댓글과 대댓글 부분을 구현해보면서 컴포넌트 간의 관계적인 
부분과 구조적인 부분에 대해서 생각해볼 수 있었고 구현해봄으로써 이 부분에 대한 이해도가 높아졌던 것 같습니다.
<br/><br/> <a href="https://github.com/ram-yeon/everyday">→ [에브리데이] 프로젝트 GitHub 보러가기</a>
<h5>:page_with_curl: Acknowledgments</h5>
- <a href="https://velog.io/@wkahd01/%EB%8C%80%EB%8C%93%EA%B8%80-%EA%B8%B0%EB%8A%A5-%EB%A7%8C%EB%93%A4%EA%B8%B0-React">대댓글 기능 만들기</a>



