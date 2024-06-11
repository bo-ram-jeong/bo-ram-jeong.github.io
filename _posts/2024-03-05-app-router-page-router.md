---
layout: single
title:  "[Next] App Router vs Page Router"

categories:
  - Next
tags:
  - Next
  - React
  - Typescript

published: true
---

<br/><br/>
취업하고 오랜만에 블로그를 작성한다..<br/>
업무 외 자기계발하는 시간이 현저히 줄어들어 앞으로라도 꾸준히 작성하고자 다시 이렇게 블로그를 작성하게 되었다.<br/><br/>
회사에서 1년동안 vue2를 사용하다 vue2의 버전 지원이 공식적으로 종료됨에 따라 기존 회사 솔루션 서비스를 vue에서 react로 마이그레이션 할 수 있는 기회가 주어졌고 다함께 각자 스터디를 병행하며 마이그레이션을 진행하게 되었다.<br/><br/>
기술 스택은 ```Next/React/TypeScript/React-Query/Recoil/Styled-Component```를 사용하기로 하였다. 
<br/>아무래도 처음 접하는 기술이 많다보니 러닝커브가 생각보다 더 들었고 따로 더 깊이 있는 공부와 원리에 대한 이해도가 필요하다고 느껴 기술 블로그를 함께 작성하며 정리해보려고 한다.

<br/>
회사에서는 next14에서 새로 추가된 app router에 대해 아직 실무에 적용하기엔 안정화가 필요하다고 판단되어 page router를 사용하였다.
<br/>
page router가 익숙해질때쯤 app router를 사용해보고 싶은 생각이 들었고 따로 프로젝트를 만들어보며 공부하게 되었다.
<br/><br/>
따라서, 이번 블로그에서는 App Router VS Page Router 에 대해 비교해보며 새로 나온 App Router의 장단점은 무엇인지 앞으로 App Router를 사용함으로써 어떻게 더 활용해볼 수 있는지 알아보았다.

<br/>
## App Router vs Page Router 
<u>가장 먼저 보이는 차이점 => 폴더 구조</u>
<br/>
```
1) pages 폴더 -> app 폴더 로 변경(폴더명 변경)
2) _app.tsx + _document.tsx -> layout.tsx 
3) index.tsx -> page.tsx 로 변경(파일명 변경)
```

<br/>
## App Router 장점
### 1. 각종 폴더 유형 추가로 편한 디렉토리 라우팅 
예를 들어, 로그인 전과 후의 화면이 달라질 경우, 각각의 layout.tsx를 만들어 줄 수 있는데 <br/>
이때, 괄호로 감싸준 폴더명을 만들어줌으로써 라우팅엔 관여를 하지 않도록 하고 해당 폴더만의 layout.tsx를 각각 만들어 줄 수 있다.
<br/><br/>
<img src="https://github.com/bo-ram-jeong/bo-ram-jeong.github.io/assets/84834172/90619244-0967-4e91-8e30-823de0ce39d6" width="200" height="300">
<br/>

### 2. 레이아웃 기능
레이아웃 기능으로 layout.tsx와 template.tsx가 있는데 주로 layout.tsx를 쓴다.
<br/>둘의 차이는 layout.tsx은 재렌더링이 되지 않지만 template.tsx은 재렌더링이 가능하다는 것이다. 즉, 마운트가 된다는 것이다. 
<br/>따라서 둘은 같이 함께 공존하지 못한다. 만약, 페이지를 넘나들때마다 레이아웃이 재렌더링을 하고 싶다면 template.tsx를 써야한다.

### 3. 페이지별 권한 체크
### 4. 서버 컴포넌트 분리로 인한 최적화 
### 5. 데이터 캐시 
프론트에서 백엔드로 보낸 요청들을 얼마나 오래 캐싱시킬 수 있는지에 대한 Data Cache 기능이 지원된다.
### 6. 서버 액션
클라이언트 컴포넌트 또는 서버 컴포넌트에서 직접 비동기 코드를 실행시킬 수 있는 Server Actions가 추가되었다. 






<br/><br/><br/><br/><br/>
참고 및 출처 : [[Next + React Query로 SNS 서비스 만들기] 인프런 강의 중,](https://www.inflearn.com/course/next-react-query-sns%EC%84%9C%EB%B9%84%EC%8A%A4/dashboard)





