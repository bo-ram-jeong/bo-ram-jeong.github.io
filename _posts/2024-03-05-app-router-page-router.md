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

<br/><br/>
회사에서는 next14에서 새로 추가된 app router에 대해 아직 실무에 적용하기엔 안정화가 필요하다고 판단되어 page router를 사용하였다.
<br/>
page router가 익숙해질때쯤 app router를 사용해보고 싶은 생각이 들었고 따로 프로젝트를 만들어보며 공부하게 되었다.
<br/>
따라서, 이번 블로그에서는 App Router VS Page Router 에 대해 비교해보며 둘의 장단점은 무엇인지 차이는 무엇인지 앞으로 App Router를 사용함으로써 어떻게 더 활용해볼 수 있는지 알아보았다.

<br/><br/>
먼저 App Router가 가지고 있는 장은 크게 6가지이다. 
<br/>
#### 각종 폴더 유형 추가로 편한 디렉토리 라우팅 #### 레이아웃 기능 #### 페이지별 권한 체크
#### 서버 컴포넌트 분리로 인한 최적화 ###### 데이터 캐시 #### 서버 액션







참고 및 출처 : [Next + React Query로 SNS 서비스 만들기] 인프런 강의 중,
(https://www.inflearn.com/course/next-react-query-sns%EC%84%9C%EB%B9%84%EC%8A%A4/dashboard)





