---
layout: single
title:  "[JavaScript] ECMAScript 버전별 특징"

categories:
  - JavaScript
tags:
  - javascript
  - es5
  - es6
  - es11

published: true
---

<br/>
<br/>전부터 JavaScript를 공부하면서 정리하고 싶었던 ES5와 ES6의 차이 그리고 비교적 최근에 나온 ES11, ES12 문법까지 버전별 특징을 정리해보려고 한다.
<br/><br/>
# ECMAScript란?
위에서 언급한 ES는 ECMAScript로, 자바스크립트를 표준화하기 위해 만들어졌으며 Ecma International이 ECMA-262 기술 규격에 따라 정의하고 있는 표준화된 스크립트 프로그래밍 언어를 말한다. 
<br/><br/>
# ES1 / ES2 / ES3 / ES4
- ES1(1997.6)
<br/> : 초판
- ES2(1998.6) 
<br/> : ISO/IEC 16262 국제 표준과 완전히 동일한 규격을 적용하기 위해 변경되었다.
- ES3(1999.12)
<br/> : 우리가 흔히 말하는 자바스크립트로 강력한 정규 표현식, 향상된 문자열 처리, 새로운 제어문 , try/catch 예외 처리, 엄격한 오류 정의, 수치형 출력의 포매팅 등의 내용으로 업데이트 되었다.
- ES4 
<br/> : 4번째 판은 언어에 얽힌 정치적 차이로 인해 버려졌으며 이 판을 작업 가운데 일부는 5번째 판을 이루는 기본이 되고 다른 일부는 ECMA스크립트의 기본을 이루고 있다.
<br/>
- ES5(2009.12)
<br/> : ES4 버전에서 10년만인 2009년 12월에 발표됐으며, 더 철저한 오류 검사를 제공하고 오류 경향이 있는 구조를 피하는 하부집합인 
"strict mode"를 추가한다. 3번째 판의 규격에 있던 수많은 애매한 부분을 명확히 한다.
<br/><br/>
1) 배열에 forEach, map, filter, reduce, some, every와 같은 메소드 지원<br/>
2) Object에 대한 getter / setter 지원<br/>
3) 자바스크립트 strict 모드 지원 <br/>
4) JSON 지원

<br/><br/>

# ES6(ES2015)
2015년 6월에 나왔으며, 6판에는 클래스와 모듈 같은 복잡한 응용 프로그램을 작성하기 위한 새로운 문법이 추가되었다. 
하지만 이러한 문법의 의미는 5판의 strict mode와 같은 방법으로 정의된다.
<br/>
**(추가된 문법과 자세한 특징은 아래 별도 정리)**

<br/>
# ES7(ES2016) / ES8(ES2017) / ES9(ES2018) / ES10(ES2019)
- ES7(2016.6)
<br/> 
1) Exponentiation oprator <br/>
2) Array.prototype.includes <br/>

- ES8(2017.6)
<br/> 
1) 함수 표현식의 인자에서 trailing commas 허용  <br/>
2) Object values/entries  <br/>
3) Object.getOwnPropertyDescriptors <br/>
4) Async functions <br/><br/>
```
async / await 는 ES6에서 callback hell을 해결하기 위해 Promise가 도입된 것처럼 
async / await도 Promise처럼 callback 을 해결할 뿐만 아니라 더 직관적이고 단순한 코드를 만들 수 있다.
```

- ES9(2018.6) 
<br/>
1) Promise.finally <br/>
2) Async iteration <br/>
3) object rest/spread property <br/>
4) 정규표현식 <br/>

- ES10(2019.6) 
<br/> 
1) Object.fromEntries <br/>
2) trimStart() 와 trimEnd() <br/>
3) flat, flatMap<br/>
4) Symbol.description <br/>
5) optional catch <br/>

<br/><br/>

---

<br/>

# ES6(ES2015) 주된 특징
## 1) Default Parameters 
## 2) Template Literals 
## 3) Multi-line Strings
## 4) Destructuring Assignment
## 5) Enhanced Object Literals
## 6) Arrow Functions
## 7) Promises
## 8) Block-Scoped Constructs Let and Const
## 9) Classes
## 10) Modules
## 11) String Method
## 12) Shorthand property names
## 13) spread syntax
## 14) ternary operator

<br/><br/>
# ES11(ES2019) 주된 특징
## 1) Optional chaining
## 2) Nullish Coalescing Operator

<br/><br/>
# ES12(ES2021) 주된 특징
## 1) String.prototype.replaceAll()
String.prototype.replaceAll 메소드는 정규식에 g옵션으로 전역으로 적용하지 않고 문자열의 지정한 모든 문자열을 특정 문자열의 값으로 변경할 수 있다.
```js
const string="Hello!"

// before
console.log(string.replace(/l/,"#")) 	//He#lo
console.log(string.replace(/l/g,"#")) 	//He##o 

// after
console.log(string.replaceAll("l","#")) 	//He##o	
```

## 2) Promise.any()
Promise.any<br/> 
: 프로미스 중에 가장 먼저 첫 번째로 이행된(해결된) 프로미스가 생기면 단락되고 해당 객체 반환한다. Promise.any()는 약속이 이행되지 않으면 AggregateError로 거부한다.
<br/><br/>  
Promise.race<br/> 
: 프로미스 중에 가장 먼저 완료된 결과값으로 이행/거부
<br/><br/> 
**Promise.any()는 약속이 먼저 거부되더라도 이행할 첫 번째 약속으로 해결하기 때문에, 이 부분이 첫 번째 약속으로 해결하거나 거부하는 Promise.race() 와 대조됨**

## 3) WeakRefs
WeakRef는 약한 참조(Weak References)를 의미한다. 
<br/> **약한 참조 => 가비지컬렉터 대상(언제든지 객체를 없애고 메모리를 뻇어올 수 있다)**
<br/> **가비지 컬렉터 => 사용하지 않는 객체를 메모리에서 자동으로 해제해줌(참조가 걸려있으면 메모리에서 제거되지 않음)**
<br/><br/>
약한 참조의 주요 용도는 캐시 또는 대형 개체에 대한 매핑을 구현하는 것이며 이러한 기능은 드물게 사용되는 캐시 또는 매핑을 저장하는데 오랜 시간 동안 메모리를 유지하고 싶지 않은 경우 사용한다.
<br/><br/>
```js
// EX1)
// deref()는 참조에 접근하기 위해 사용
let user = {name:"mike",age:30};
const weakUser = new WeakRef(user);
user = null;
const timer = setInterval(()=>{
	const wUser = weakUser.deref();
	if(wUser){
		console.log(wUser.name);
	} else{
		console.log("제거되었습니다");
		clearInterval(timer);
	}
},1000)
// 결과: 일곱번 마이크 이름이 찍히고 "제거되었습니다" 찍힘 ==> weakref는 특정객체를 일정시간 만큼만 캐시하도록 사용하기도 함

// EX2)
class MyCache{
	constructor(){
		this.cache={};
	}
	add(key,obj){
		this.cache[key]=new WeakRef(obj)	//weakref가 아닌 obj를 넣어줄 경우 강한 참조가됨
	}
	get(key){	//add로 넣어준 객체를 다시 읽을때 사용
		let cachedRef=this.cache[key].deref();
		if(cachedRef){	//지워졌을 수 있기 때문에 if문으로 있는지 없는지 항상 확인 필요. 
			return cachedRef
		}else{
			return false
		}
	}	
}
// 만약 WeakRef(obj)가 아닌 obj를 넣어주게되면 전달해 준 객체가 사라진다해도, 가비지가 된다하더라도 GC 대상으로 인식하지 못하기 때문에 문제발생. 따라서 WeakRef가 그런 문제를 방지함

```

## 4) Logical assignment operators
**논리 할당 연산자**
<br/>다음과 같이 기존의 논리 연산자들을 축약할 수 있다.
```js

// before
num = num || 0; // name이 잘못된 값일 경우 할당
name = name && `Hello ${name}`; // name이 올바른 값일 경우 할당
name = name ?? "Mike"; // name이 null이나 undefined일 경우 Mike 할당

// after
num ||= 0;
name &&= `Hello ${name}`;
name ??= "Mike"; //Nullish coalescing operator(null 병합 연산자)

```

## 5) Numeric separators
**숫자구분자**
<br/>숫자 구분 기호는 _ 를 사용해 숫자를 시각적으로 더 읽기 쉽게 만들어준다.
```js
let billion = 1_000_000_000 // 10억(,구분자는 인식못함 _로 구분해야함)
```      

<br/><br/>
# 마무리
주로 실무에서 많이 사용한다는 ES6문법과 ES11문법에서 일부만 또는 프로젝트에서 코딩하면서 필요한 문법만 그때 그때 이해하고 사용하다보니 머릿속에서 정리가 깔끔히 되지 않는 느낌이었다.
<br/>이렇게 전체적인 ECMAScript의 버전별로 구조적인 부분을 정리해보니 확실히 더 이해가 잘 되었고 정리하다보니 놓치고 있던 문법들도 정확히 알 수 있었던 시간이 되어서 너무나 유익한 시간이었다.
<br/>왜 ES5에서 ES6로 버전이 업데이트 되었는지, 왜 이 문법이 추가가 되었는지 버전마다 문법이 비교가 되면서 지금의 ES2021까지 어떻게 업데이트 되었는지 이해되는 시간이었다.


<br/>
<h5>:page_with_curl: Acknowledgments</h5>
- <a href="https://ko.wikipedia.org/wiki/ECMA%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8">ECMA스크립트 - 위키백과</a>
- <a href="https://www.youtube.com/watch?v=36HrZHzPeuY">[드림코딩] 유튜브 강의 중</a>
- <a href="https://www.youtube.com/watch?v=CvKSR94D0QE&list=PLZKTXPmaJk8JZ2NAC538UzhY_UNqMdZB4&index=19">[코딩앙마] 유튜브 강의 중</a>
- <a href="https://medium.com/sjk5766/ecma-script-es-%EC%A0%95%EB%A6%AC%EC%99%80-%EB%B2%84%EC%A0%84%EB%B3%84-%ED%8A%B9%EC%A7%95-77715f696dcb">[Jeongkuk Seo] 님의 블로그</a>
- <a href="https://abangpa1ace.tistory.com/231">[ttaeng_99's] 님의 블로그</a>



