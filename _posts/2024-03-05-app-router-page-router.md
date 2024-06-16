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
<br> 이러한 **그룹 폴더**만이 아닌 **패러렐 라우트('@' 를 붙인 폴더)** 폴더, **private 폴더('_' 를 붙인 폴더)**
 들은 라우팅에 관여를 하지 않는 폴더들로 이러한 폴더들이 추가됨에 따라 정리 측면에 편의성이 추가된 것 같다.
<br/><br/>
<img src="https://github.com/bo-ram-jeong/bo-ram-jeong.github.io/assets/84834172/90619244-0967-4e91-8e30-823de0ce39d6" width="200" height="300">
<br/>

### 2. 레이아웃 기능
레이아웃 기능으로 layout.tsx와 template.tsx가 있는데 주로 layout.tsx를 쓴다.
<br>둘의 차이는 layout.tsx은 재렌더링이 되지 않지만 template.tsx은 재렌더링이 가능하다는 것이다. 즉, 마운트가 된다는 것이다. 
<br>따라서 둘은 같이 함께 공존하지 못한다. 만약, 페이지를 넘나들때마다 레이아웃이 재렌더링을 하고 싶다면 template.tsx를 써야한다.

### 3. 데이터 캐시 
프론트에서 백엔드로 보낸 요청들을 얼마나 오래 캐싱시킬 수 있는지에 대한 Data Cache 기능이 지원된다.<br>
따라서, 데이터 캐시는 기본적으로 계속 유지가 되기 때문에 revalidate나 opting out을 잘 처리해줘야 하며 만약 지원되는 데이터 캐시를 사용하지 않으려면 cache: no-store 옵션을 주어야 한다.<br>
데이터 캐시는 서버의 반복적인 요청을 줄여주고 동일한 요청에 대해 네트워크 비용을 절감시킬 수 있는 장점이 있다. 

### 4. 서버 액션
클라이언트 컴포넌트 또는 서버 컴포넌트에서 직접 비동기 코드를 실행시킬 수 있는 Server Actions 코드를 작성할 수 있다.
예를 들어 아래와 같이 작성한 서버 코드는 브라우저에 노출이 되지 않기 때문에 키값이나 비밀값 이런 것들을 안전하게 사용을 할 수 있고 기존에는 브라우저에서 서버 거쳐서 데이터베이스로 접근을 해야 했다면 지금은 브라우저에서 바로 DB에 접근할 수 있어, 성능상 더 빨라진다는 이점이 있다.

```js
const submit = async (formData) =>{
  'use server'
  const response = await fetch({'api 주소',
    method:'post'
    body: formData,
    credentials: 'include',
  })
}

```

<br><br>
### 정리
이 외에도 app router의 장점으로는 **페이지별 권한 체크, 서버 컴포넌트 분리로 인한 최적화** 등의 장점들이 존재한다. <br>
[SNS-Project](https://github.com/bo-ram-jeong/next-react-project)를 만들어보면서 느꼈던 app router는 아직까지는 처음 적용해보다보니 서버 컴포넌트의 장점보다는 폴더구조의 복잡성이라던가 
서버 컴포넌트와 클라이언트 컴포넌트를 구분하여 생성하고 적용하는 부분에 있어 어려움을 더 크게 느껴 불편했던 부분이 있었던 것 같다.
하지만 확실히 서버 컴포넌트의 등장으로 서버측 렌더링의 성능상 이점을 높이는 장점은 확실한 듯하다.
이전에 클라이언트 자바스크립트 번들 사이즈에 영향을 주던 큰 의존성을 서버 컴포넌트를 통해 모두 서버에서 처리할 수 있기 때문에 클라이언드 JS 번들 사이즈는 감소하고 서버 컴포넌트를 사용하기 때문에 초기 페이지 로드 속도도 향상된다.
아직 내가 지금까지 해본 프로젝트에는 SSR 방식이 굳이 필요로 하지 않거나 CSR 방식으로 충분히 해결이 되었어서 추후에 서버 컴포넌트의 이점을 잘 활용할 수 있는 프로젝트에서 app router의 새로 추가된 기능들을 사용해며 효율성을 이끌어낼 수 있도록 개발해보고 싶다!



<br/><br/><br/><br/><br/>
참고 및 출처 : [[Next + React Query로 SNS 서비스 만들기] 인프런 강의 중,](https://www.inflearn.com/course/next-react-query-sns%EC%84%9C%EB%B9%84%EC%8A%A4/dashboard)





