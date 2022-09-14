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
<br/>
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
**기본 매개변수**
함수를 호출할 때, 인자값 없이 보냈을 때를 대비하여 파라미터에 디폴트값을 정해놓는다.
```js
//es5
var example = (a, b) => {
  console.log(a, b);
};

example("a");
//a undefined

//es6
const example = (a, b = "b") => {
  console.log(a, b);
};

example("a");
//a b
```
## 2) Template Literals 
템플릿 리터럴은 작은 따옴표('), 큰 따옴표(") 대신 백틱(`)으로 문자열을 감싸 표현하는 기능을 말한다. 이는 문자열을 중간에 추가할 때 '+'를 사용할 필요없이 백틱 사이에 ${문자열}을 넣어 추가할 수 있다.
```js
//es5
let age = 24;
let name = "정보람"
console.log(name + "의 나이는 " + age + "입니다.")

//es6
let age = 24;
let name = "정보람"
console.log(`${name}의 나이는 ${age}입니다.`)
```
## 3) Destructuring Assignment
**구조 분해 할당**
배열이나 Object 형태에서 속성을 해제하여 변수로 할당하는 방식을 의미한다. 따라서 어떤 것을 복사한 이후에 변수로 복사해준다는 의미인데, 이 과정에서 분해 혹은 파괴되지 않는다는 특징이 있다.
```js
// Array
const example = [ 1, 2, 3 ] 
const [x,y,z] = example;
console.log(x,y,z); // 1 2 3

// Object 
const me = {
	name: "boram",
	age: "24",
	hobby: "Watching movies"
}

const { name, age, hobby } = me;
console.log(name, age, hobby); //boram 24 Watching movies
```
## 4) Enhanced Object Literals
향상된 객체 리터럴은 기존 자바스크립트에서 사용하던 객체 정의 방식을 개선한 문법으로 자주 사용하던 문법들을 좀 더 간결하게 사용할 수 있도록 객체 정의 형식을 변경하였다.
```js
// es5
var boram = {
  // 속성: 값
  language: 'javascript',
  coding: function() {
    console.log('Hello World~');
  }
};

// es6
let language = 'javascript';
let boram = {
  // language: language,  //속성과 값이 같으면 아래와 같이 축약 가능
  language
  coding() {  //속성에 함수를 정의할 때 function 예약어 생략 가능
    console.log('Hello World~');
  }
};

console.log(josh); // {language: "javascript"}
josh.coding(); // Hello World~
```
## 5) Arrow Functions
**화살표 함수**
화살표 함수는 javascript에서 함수를 정의하는 function 키워드 없이 함수를 만들 수 있다.
```js
// es5
function func(name) {
  return `hello ${name}`;
}

// es6
// 함수 func는 화살표(=>) 우측의 표현식(expression)을 평가하고, 평가 결과 반환
const func = (name) => {
  return `hello ${name}`;
};

// return 키워드를 사용하지 않아도 됨
// 파라미터가 하나밖에 없다면 파라미터를 감싸는 괄호 생략 가능
// 괄호를 생략하면 코드 길이를 더 줄일 수 있음
const func = name => `hello ${name}`;
```
## 6) Promises
자바스크립트의 비동기 콜백 지옥을 해결해 줄 기법이 추가되었다.
```
> Promise는 new 로 생성하고 함수로 전달받는데 인수는 resolve와 reject이다.
> 어떤일이 완료된이후, 실행되는 함수를 콜백(callback) 함수라 하는데, 콜백 지옥은 함수 안에 함수 호출로 depth가 깊어지면서 콜백을 호출하는 것을 말한다. 
주로 이벤트 처리나 서버 통신과 같은 비동기적인 작업을 수행하기 위해 이런 형태가 자주 등장하는데, 
가독성이 떨어지면서 코드를 수정하기 어려운 단점이 있어, 이를 해결하기 위해 나온것이 Promise이다. 

> 프로미스 객체는 state와 result를 프로퍼티로 가진다.
- state: pending(대기)
- result: undefined

- state: fulfilled(이행됨)
- result: value

- state: rejected(거부됨)
- result: error
```
```js
// EX1) 콜백 지옥
const f1=(callback)=>{
	return new promise((res,rej)=>{
		setTimeout(()=>{
			res("1번완료")
		},1000)
	}
}
const f2=(callback)=>{
	return new promise((res,rej)=>{
		setTimeout(()=>{
			res("2번완료")
		},3000)
	}
}
const f3=(callback)=>{
	return new promise((res,rej)=>{
		setTimeout(()=>{
			res("3번완료")
		},2000)
	}
}

f1(function(){
	f2(function(){
    ...
		// 함수 안에 함수 호출로 depth가 깊어지면서 콜백을 호출 -> 콜백지옥
    ...
	}
}

// EX2) Promises chaining
f1()
.then((res)=>f2(res))
.then((res)=>f3(res))
.then((res)=>console.log(res))
.catch(console.log)
.finally(()=>{console.log("끝")})	// 모두 기다려서 합한 6초가 걸림

// EX3) 모든 시간 기다리지 않고 한꺼번에 동시처리하는 Promise.all
Promise.all([f1(),f2(),f3()]).then((res=>{	//제일 긴 3초가 걸림
	console.log(res)
})
// => f1 f2 f3중에 하나라도 에러이거나 누락되면 페이지를 보여줄 수 없도록 에러 처리할 때 사용
```

## 7) Block-Scoped Constructs Let and Const
## 8) Classes
클래스는 new를 통해 호출하면 실행되고 객체를 초기화하기 위한 값이 constructor에 정의되어 있다. constructor는 객체를 만들어주는 생성자 메소드이다.
```js
class User{
	constructor(name,age){
		this.name=name
		this.age=age
	}
	showName(){
		console.log(`${this.name} : ${this.age}`)
	}
}
const User = new User('정보람', 24);
User.showName(); // 크리스 : 24
```
## 9) Modules
모듈은 실현가능한 특정 프로그램의 그룹으로 ES6부터는 export, import로 모듈을 관리할 수 있고
export와 import로 재사용 가능한 구성요소를 작성할 수 있다. export로 모듈을 내보낼 수 있고, import로 원하는 모듈을 가져와 사용할 수 있다.
둘 이상의 모듈을 import하려는 경우 import { 임포트 하려는 모듈 } from '임포트하려는 모듈이 들어있는 파일의 경로'의 형식으로 사용할 수 있다.
```import{ a, b, c } from './modules'```

## 10) spread syntax
배열 혹은 Object 형태에서 기존에 있는 값을 그대로 유지해서 불러오는 방법으로 구조 분배 할당(destructuring)등에 다양하게 활용할 수 있다.
```js
// Array
const example = [1,2,3];
console.log(...example);	//1 2 3

// Object
const example1 = {
	name: 'boram',
  age: 24,
};

const example2 = {
  hobby: 'Watching movies',
} 

const user = {
	...example1,
	...example2,
}
```

<br/><br/>
# ES11(ES2019) 주된 특징
## 1) Optional chaining
```?.``` 연산자를 사용하면 지저분한 방어 로직이나 유틸리티 라이브러리 없이 안전하고 깔끔하게 속성값에 접근할 수 있다.
객체의 속성을 접근할 때 ```.``` 연산자 대신에 ```?.``` 연산자를 사용하면, 해당 객체가 nullish 즉, undefined나 null인 경우에 TypeError 대신에 undefined를 얻게 된다.
```js
let user = {name: {first: "boram", last: "Jeong"}}

> user?.name?.first
'boram'
> user?.address?.street
undefined
```
## 2) Nullish Coalescing Operator
Nullish coalescing operator(??)는 논리 연산자로
왼쪽 피연산자가 null이나 undefined일 때, 오른쪽 피연산자를 return한다.
반대의 경우면, 왼쪽 피연산자가 return된다.
```js
let a = null ?? 'hello';
let b = '' ?? true;
let c = false ?? true;
let d = 0 ?? 1;
let e = undefined ?? 'world'; 

console.log(a);	// 'hello'
console.log(b); // ''
console.log(c); // false
console.log(d); // 0
console.log(e); // 'world'
```

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
Promise.any <br/>
: 프로미스 중에 가장 먼저 첫 번째로 이행된(해결된) 프로미스가 생기면 단락되고 해당 객체 반환. 만약, Promise.any()는 약속이 이행되지 않으면 AggregateError로 거부.

Promise.race <br/> 
: 프로미스 중에 가장 먼저 완료된 결과값으로 이행/거부

즉, Promise 이터레이터를 인자로 받아 하나라도 성공한다면 전체를 완료한다. 만약, 모두 실패한다면 모든 실패 이유가 포함된 AggregateError가 발생한다.
ES6에서 등장한 Promise.race() 와 유사한 듯 보이나, Promise.race() 는 Promise 이터레이터 중 하나라도 성공 혹은 실패하면 전체를 완료한다는 차이점이 있다.

## 3) WeakRefs
WeakRef는 약한 참조(Weak References)를 의미한다. 
<br/><br/> **약한 참조 => 가비지컬렉터 대상(언제든지 객체를 없애고 메모리를 뻇어올 수 있다)**
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
num = num || 0; // num이 잘못된 값일 경우 0 할당
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
<br/><br/>이렇게 전체적인 ECMAScript의 버전별로 구조적인 부분을 정리해보니 확실히 더 이해가 잘 되었고 정리하다보니 놓치고 있던 문법들도 정확히 알 수 있었던 시간이 되어서 너무나 유익한 시간이었다.
<br/><br/>왜 ES5에서 ES6로 버전이 업데이트 되었는지, 왜 이 문법이 추가가 되었는지 버전마다 문법이 비교가 되면서 지금의 ES2021까지 어떻게 업데이트 되었는지 이해되는 시간이었다.


<br/>
<h5>:page_with_curl: Acknowledgments</h5>
- <a href="https://ko.wikipedia.org/wiki/ECMA%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8">ECMA스크립트 - 위키백과</a>
- <a href="https://www.youtube.com/watch?v=36HrZHzPeuY">[드림코딩] 유튜브 강의 중</a>
- <a href="https://www.youtube.com/watch?v=CvKSR94D0QE&list=PLZKTXPmaJk8JZ2NAC538UzhY_UNqMdZB4&index=19">[코딩앙마] 유튜브 강의 중</a>
- <a href="https://medium.com/sjk5766/ecma-script-es-%EC%A0%95%EB%A6%AC%EC%99%80-%EB%B2%84%EC%A0%84%EB%B3%84-%ED%8A%B9%EC%A7%95-77715f696dcb">[Jeongkuk Seo] 님의 블로그</a>
- <a href="https://abangpa1ace.tistory.com/231">[ttaeng_99's] 님의 블로그</a>



