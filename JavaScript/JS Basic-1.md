[TOC]

목차

1. Intro
2. 변수
3. 데이터 타입
4. 연산자
5. 조건/반복

# 1. Intro

## 1.1. 동작 방식

`장고 등 MTV` 서버 -> response(HTML CSS) -> 브라우저 `크롬` `파이어폭스` `오페라` `사파리` `IE`...



## 1.2. 브라우저 (browser)

- URL로 웹(WWW)을 탐색하며 서버와 통신하고, HTML 문서나 파일을 출력하는 GUI 기반의 소프트웨어
- 인터넷의 컨텐츠를 검색 및 열람하도록 함
- "웹 브라우저" 라고도 함
- 주요 브라우저
  - 구글 크롬, 모질라 파이어폭스, 마이크로소프트 엣지, 오페라, 사파리



## 1.3. JS의 필요성

- 프라우저 화면을 '동적'으로 만들기 위함
- 브라우저를 조작할 수 있는 **`유일한 언어`**



## 1.4. 가장 인기 있는 언어 (2021)

![image-20220425133935745](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220425133935745.png)





## 1.5. Browser

### 1.5.1. 브라우저에서 할 수 있는 일

- DOM (Document Object Model) 조작
  - 문서(HTML) 조작
- BOM (Browser Object Model) 조작
  - navigator, screen, location, frames, history, XHR
- JavaScript Core (ECMAScript)
  - Data Structure (Object, Array), Conditional Expression, Iteration




### 1.5.2. DOM 이란?

>  문서를 프로그램으로 조작할 수 있다.

- HTML, XML 과 같은 문서를 다루기 위한 프로그래밍 인터페이스
- 문서를 구조화하고, 구조화된 구성 요소를 하나의 객체로 취급하여 다루는 논리적 트리 모델
- 문서가 객체 (object) 로 구조화되어 있으며 key로 접근 가능
- 단순한 속성 접근, 메서드 활용뿐만 아니라 프로그래밍 언어적 특성을 활용한 조작 가능
- 주요 객체
  - window : DOM을 표현하는 창 (브라우저 탭). 최상위 객체 (작성 시 생략 가능)
  - document : 페이지 컨텐츠의 Entry Point 역할을 하며, \<head>, \<body> 등과 같은 수많은 다른 요소들을 포함
  - navigator, location, history, screen


![image-20220425134152378](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220425134152378.png)

#### 1.5.2.1. DOM - 해석

- 파싱 (Parsing)
  - 구문 분석, 해석
  - 브라우저가 문자열을 해석하여 DOM Tree로 만드는 과정

#### 1.5.2.2. DOM - 조작

- 콘솔을 열고 타이틀 조작

```javascript
document.title = 'Hi java'
"hi java"
-> 탭 이름이 hi java로 바뀜
```



### 1.5.3. BOM 이란?

- Browser Object Model
- 자바스크립트가 브라우저와 소통하기 위한 모델
- 브라우저의 창이나 프레임을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단
  - 버튼, URL 입력창, 타이틀 바 등 브라우저 윈도우 및 웹 페이지 일부분을 제어 가능
- window 객체는 모든 브라우저로부터 지원받으며 브라우저의 창 (window)를 지칭

#### 1.5.3.1. BOM - 조작

![image-20220425142537574](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220425142537574.png)



### 1.5.4. JavaScript Core

- 브라우저 (BOM & DOM) 을 조작하기 위한 명령어 약속 (언어)

![image-20220425142628436](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220425142628436.png)



### 1.5.5. 정리

![image-20220425142705599](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220425142705599.png)

브라우저 (BOM) 과 그 내부의 문서 (DOM) 를 조작하기 위해 ECMAScript (JS) 를 학습



## 1.6. ECMA

- ECMA (ECMA International)
  - 정보 통신에 대한 **표준을 제정하는 비영리 표준화 기구**
- ECMAScript는 ECMA에서 **ECMA-262*** 규격에 따라 정의한 언어
  - ECMA-262*: 범용적인 목적의 프로그래밍 언어에 대한 명세
- **ECMAScript6는 ECMA에서 제안하는 6번째 표준 명세를 말함**
  - (참고) ECMAScript6의 발표 연도에 따라 ECMAScript2015 라고도 불림



## 1.7. 세미콜론 (semicolon)

- 자바스크립트는 **세미콜론을 선택적으로 사용 가능**
- 세미콜론이 없으면 **ASI***에 의해 자동으로 세미콜론이 삽입됨
  - ASI*: 자동 세미콜론 삽입 규칙 (Automatic Semicolon Insertion)
- 일단은, 세미콜론 사용하지 않고 진행하기로 하였음



## 1.8. 코딩 스타일 가이드

- 코딩 스타일의 핵심은 **합의된 원칙과 일관성**
  - 절대적인 하나의 정답은 없으며, 상황에 맞게 원칙을 정하고 일관성 있게 사용하는 것이 중요
- 코딩 스타일은 **코드의 품질에 직결되는 중요한 요소**
  - 코드의 가독성, 유지보수 또는 팀원과의 커뮤니케이션 등 **개발 과정 전체에 영향을 끼침**
- (참고) 다양한 자바스크립트 코딩 스타일 가이드
  - Airbnb Javascript Style Guide <- 일단은 이걸로!
  - Google Javascript Style Guide
  - standardjs







# 2. 변수와 식별자

## 2.1. 식별자 정의와 특징

- 식별자(identifier)는 변수를 구분할 수 있는 변수명을 말함
- 식별자는 반드시 문자, 달러($) 또는 밑줄(_)로 시작
- 대소문자를 구분하며, 클래스명 외에는 모두 소문자로 시작
- 예약어* 사용 불가능
  - 예약어 예시: for, if, function 등



## 2.1.1. 식별자 작성 스타일

- **카멜 케이스**(camelCase, lower-camel-case)
  - 변수, 객체, 함수에 사용
- **파스칼 케이스**(PascalCase, upper-camel-case)
  - 클래스, 생성자에 사용
- **대문자 스네이크 케이스**(SNAKE_CASE)
  - **상수(constants)*에 사용**
    - 상수의 정의: 개발자의 의도와 상관없이 **변경될 가능성이 없는 값을 의미**

```javascript
// 카멜 케이스: 두 번째 단어의 첫 글자부터 대문자
// 변수
let dog
let variableName

// 객체
const userInfo = { name: 'Tom', age: 20 }

// 함수
function add() {}
function getName() {}


// 파스칼 케이스: 모든 단어의 첫 번째 글자를 대문자로 작성
// 클래스
class User {
    constructor(options) {
        this.name = options.name
    }
}

// 생성자 함수
function User(options) {
    this.name = options.name
}


// 대문자 스네이크 케이스: 모든 단어 대문자 작성 & 단어 사이에 언더스코어 삽입
// 값이 바뀌지 않을 상수
const API_KEY = 'my-key'
const PI = Math.PI

// 재할당이 일어나지 않는 변수
const numbers = [1, 2, 3]
```



## 2.2. 변수 선언 키워드 (let, const)

**let**

- **재할당 할 예정인** 변수 선언 시 사용
- 변수 **재선언 불가능**
- 블록 스코프*

**const**

- **재할당 할 예정이 없는** 변수 선언 시 사용
- 변수 **재선언 불가능**
- 블록 스코프*



시험해보기 (브라우저 조작 언어이므로, F12 누르고 Console 탭에서 바로 확인 가능)

![image-20220425140656160](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220425140656160.png)



### 2.2.1. 선언, 할당, 초기화

- 선언 (Declaration)
  - **변수를 생성**하는 행위 또는 시점
- 할당 (Assignment)
  - **선언된 변수에 값을 저장**하는 행위 또는 시점
- 초기화 (Initialization)
  - **선언된 변수에 처음으로 값을 저장**하는 행위 또는 시점

```javascript
// let (재할당 가능)
let number = 10		// 1. 선언 및 초기값 할당
number = 10			// 2. 재할당

console.log(number)	// 10
```

```javascript
// const (재할당 불가능)
const number = 10	// 1. 선언 및 초기값 할당
number = 10			// 2. 재할당 불가능

=> Uncaught TypeError: Assignment to constant variable.
```

```javascript
// let, const 모두 재선언 불가능
let number = 10
let number = 50 => Uncaught SyntaxError: Identifier 'number' has already been declared
const number = 10
const number = 50 => Uncaught SyntaxError: Identifier 'number' has already been declared
```



### 2.2.2. 블록 스코프* (block scope)

- if, for 함수 등의 **중괄호 내부**를 가리킴
- 블록 스코프를 가지는 변수는 **블록 바깥에서 접근 불가능**



### 2.2.3. var (구버전 자바스크립트)

- var로 선언한 변수는 재선언 및 재할당 모두 가능
- ES6 이전에 변수를 선언할 때 사용되던 키워드
- 호이스팅* 되는 특성으로 인해 예기치 못한 문제 발생 가능
  - 따라서 ES6 이후부터는 var 대신 const와 let을 사용하는 것을 권장
- 함수 스코프*

**함수 스코프* (function scope)**

- **함수의 중괄호 내부**를 가리킴
- 함수 스코프를 가지는 변수는 **함수 바깥에서 접근 불가능**

**호이스팅* (hoisting)**

- 변수를 **선언 이전에 참조할 수 있는 현상**

- **변서 선언 이전에 위치에서 접근시 (Error가 아니라) undefined 를 반환**

  (파이썬이라면) 에러가 나야 정상인데, 일단 undifined로 말하고, 에러를 내진 않음



**let, const, var 비교**

| 키워드 | 재선언 | 재할당 | 스코프      | 비고         |
| ------ | ------ | ------ | ----------- | ------------ |
| let    | X      | O      | 블록 스코프 | ES6부터 도입 |
| const  | X      | X      | 블록 스코프 | ES6부터 도입 |
| var    | O      | O      | 함수 스코프 | 사용X        |





# 3. 데이터 타입

## 3.1. 데이터 타입 종류

- 자바스크립트의 모든 값은 **특정한 데이터 타입을 가진다.**
- 크게 **원시 타입* (Primitive type)**과 **참조 타입* (Reference type)**으로 분류됨

진짜 값이 담기는지, 특정 좌표를 가리키는지 (참조하는지) 의 차이라고 할 수 있다.

그림 (Data type 트리)

![image-20220425143239674](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220425143239674.png)

Symbol은 잘 사용하지 않는다. 

### 3.1.1. (참고) 원시 타입과 참조 타입 비교

- **원시 타입 (Primitive type)**
  - **객체(object)가 아닌** 기본 타입
  - 변수에 **해당 타입의 값이 담김**
  - 다른 변수에 복사할 때 **실제 값이 복사됨**
- **참조 타입 (Reference type)**
  - **객체(object) 타입**의 자료형
  - 변수에 해당 **객체의 참조 값이 담김**
  - 다른 변수에 복사할 때 **참조 값이 복사됨**

#### 3.1.1.1. 원시 타입 - 숫자 (Number) 타입

- **정수, 실수 구분 없는 하나의 숫자 타입**
- **부동소수점 형식**을 따름
- (참고) NaN (Not-A-Number)
  - 계산 불가능한 경우 반환되는 값
    - ex) 'Angel' / 1004 => NaN

```javascript
const a = 2.998e8		// 거듭제곱
const b = Infinity		// 양의 무한대
const c = -Infinity		// 음의 무한대
const d = NaN			// 산술 연산 불가
```



#### 3.1.1.2. 원시 타입 - 문자열 (String) 타입

- **텍스트 데이터**를 나타내는 타입
- 16비트 유니코드 문자의 집합
- **작은따옴표** 또는 큰따옴표 모두 가능
- **템플릿 리터럴 (Template Literal)**
  - **ES6부터 지원**
  - **따옴표 대신 backtick(``)**으로 표현
  - **${ expression }** 형태로 표현식 삽입 가능

```javascript
const firstName = 'Brandan'
const lastName = 'Eich'
const fullName = '${firstName} ${lastName}' // fullName === 'Brandan Eich'
```



#### 3.1.1.3. 원시 타입 - undifined

- 변수의 **값이 없음**을 나타내는 데이터 타입
- 변수 선언 이후 **직접 값을 할당하지 않으면, 자동으로 undifined가 할당됨**

#### 3.1.1.4. 원시 타입 - null

- 변수의 **값이 없음**을 **의도적으로 표현**할 때 사용하는 데이터 타입
- (참고) null 타입과 **typeof 연산자***
  - **typeof 연산자*: 자료형 평가를 위한 연산자**
  - null 타입은 ECMA 명세의 원시 타입의 정의에 따라 원시 타입에 속하지만, **typeof 연산자의 결과는 객체(object)로 표현됨**

undifined = 개발자의 의도로 할당하지 않은 게 아님 (진짜 모름). 자바스크립트가 자동으로 할당

type은 undifined로 나옴

null = 개발자가 의도함 (없어야 한다고 함) 

type은 object (객체)로 나옴



#### 3.1.1.5. 원시 타입 - Boolean 타입

- **논리적 참 또는 거짓**을 나타내는 타입
- **true** 또는 **false로 표현**
- **조건문 또는 반복문***에서 유용하게 사용
  - (참고) 조건문 또는 반복문에서 **boolean이 아닌 데이터 타입**은 자동 형변환 규칙에 따라 true 또는 false로 변환됨
- 자동 형변환 정리

| 데이터 타입 | 거짓       | 참               |
| ----------- | ---------- | ---------------- |
| Undefined   | 항상 거짓  |                  |
| Null        | 항상 거짓  |                  |
| Number      | 0, -0, NaN | 나머지 모든 경우 |
| String      | 빈 문자열  | 나머지 모든 경우 |
| Object      |            | 항상 참          |

**조건문 또는 반복문**에서 **표현식의 결과가 참/거짓으로 판별되는 경우** 발생



### 3.1.2. 참조 타입

- 함수 (Functions)
- 배열 (Arrays)
- 객체 (Objects)





# 4. 연산자

## 4.1. 할당 연산자

- 오른쪽에 있는 피연산자의 평가 결과를 왼쪽 피연산자에 할당하는 연산자
- 다양한 연산에 대한 단축 연산자 지원

`+=`, `-=`, `*=`, `/=` -> 파이썬과 같음

`++`, `--` -> Increment 및 Decrement 연산자

- ++: 피연산자의 값을 1 증가시키는 연산자, 즉 += 1 을 줄인 것
- --: 피연산자의 값을 1 감소시키는 연산자, 즉 -= 1 을 줄인 것
- Airbnb Style Guide 에서는 +=나 -= 같이 분명한 표현으로 적을 것을 권장하고 있음



## 4.2. 비교 연산자

- 피연산자들 (숫자, 문자, Boolean 등) 을 비교하고 결과값을 boolean으로 반환하는 연산자
- 문자열은 유니코드 값을 사용하며 표준 사전 순서를 기반으로 비교
  - 알파벳의 경우 후순위가 더 크고, 소문자가 대문자보다 더 크다.



## 4.3. 동등 비교 연산자 (==)

- 두 피연산자가 같은 값으로 평가되는지 비교 후 boolean 값을 반환

- 비교할 때 **암묵적 타입 변환**을 통해 타입을 일치시킨 후 같은 값인지 비교

- 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 바라보는지 판별

- **예상치 못한 결과가 발생할 수 있으므로 특별한 경우를 제외하고 사용하지 않음**

그래서, 뭘 쓰면 되는데? -> `===`



## 4.4. 일치 비교 연산자 (===)

- 두 피연산자가 같은 값으로 평가되는지 비교 후 boolean 값을 반환
- **엄격한 비교*** 가 이뤄지며 암묵적 타입 변환이 발생하지 않음
  - **엄격한 비교***: 두 비교 대상의 **타입과 값 모두** 같은지 비교하는 방식

- 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 바라보는지 판별



## 4.5. 논리 연산자

- 세 가지 논리 연산자로 구성

  - 파이썬의 and 대신 **`&&`**

  - 파이썬의 or 대신 **`||`**

  - not 대신 **`!`** (!true = false, !!true = true)

- 단축 평가 지원 (&&면 앞이 참이어도 뒤 값까지 보고, ||면 앞이 참이면 뒤는 안 봄)



## 4.6. 삼항 연산자

> 항이 3개가 필요하다는 뜻

- 세 개의 피연산자를 사용하여 조건에 따라 **값**을 반환하는 연산자

- 가장 왼쪽의 조건식이 참이면 콜론(:) 앞의 값을 사용하고 그렇지 않으면 콜론 뒤의 값을 사용
- 삼항 연산자의 결과 값이기 때문에 변수에 할당 가능
- (참고) **한 줄에 표기하는 것을 권장**

```javascript
console.log(true ? 1 : 2)	// 1
console.log(false ? 1 : 2)	// 2

const result = Math.PI > 4 ? 'Yes' : 'No'
console.log(result)			// No
```





# 5. 조건문

## 5.1. 조건문의 종류와 특징

- if
  - 조건 표현식의 결과값을 **Boolean 타입으로 변환 후 참/거짓을 판단**
- switch
  - 조건 표현식의 결과값이 **어느 값(case)에 해당하는지 판별**
  - 주로 특정 변수의 값에 따라 조건을 분기할 때 활용
    - 조건이 많아질 경우 if문보다 가독성이 나을 수 있음



## 5.2. if, else if, else

- 조건은 **소괄호(condition)** 안에 작성
- 실행할 코드는 **중괄호{}** 안에 작성
- [블록 스코프](# 2.2.2. 블록 스코프* (block scope)) 생성

```javascript
const nation = 'Korea'

if (nation === 'Korea') {
    console.log('안녕하세요!')
} else if (nation === 'France') {
    console.log('Bonjour!')
} else {
    console.log('Hello!')
}
```

파이썬과 많이 차이나는 부분 (헷갈릴 여지가 있는 부분)

1. 조건을 소괄호 안에 넣기
2. 중괄호로 블록 여닫기
3. else부터 if 안쪽에 적기 (같은 들여쓰기가 아님)



## 5.3. switch

- **표현식 (expression) 의 결과값**을 이용한 조건문

- **표현식의 결과값과 case문의 오른쪽 값**을 비교
- break 및 default문은 [선택적] 으로 사용 가능
- **break문이 없는 경우 break문을 만나거나 default문을 실행할 때까지 다음 조건문 실행**
- [블록 스코프](# 2.2.2. 블록 스코프* (block scope)) 생성
- 예시 - break가 있는 경우

```javascript
const nation = 'Korea'

switch(nation) {
    case 'Korea': {
        console.log('안녕하세요!')	// 안녕하세요! 만 출력
        break
    }
    case 'France': {
        console.log('Bonjour!')
        break
    }
    default: {
        console.log('Hello!')
    }
}
```

- 예시 - **break가 없는 경우**

```javascript
const nation = 'Korea'

switch(nation) {
    case 'Korea': {
        console.log('안녕하세요!')
    }
    case 'France': {
        console.log('Bonjour!')
    }
    default: {
        console.log('Hello!')
    }
}	// 안녕하세요! Bonjour! Hello! 모두 출력
```



### 5.3.1. if vs. switch

![image-20220425181535903](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220425181535903.png)





# 6. 반복문

## 6.1. 반복문의 종류와 특징

- while

- for
  - 파이썬의 for와는 다르게 불편할 수도 있음
- for ... in
  - 주로 **객체(object)의 속성들을 순회할 때 사용
  - 배열도 순회 가능하지만 인덱스 순으로 순회한다는 보장이 없으므로 **권장하지 않음** 
- for ... of
  - **반복 가능한 (iterable) 객체를 순회**하며 **값을 꺼낼 때 사용**
    - 반복 가능한 객체의 종류: Array, Map, Set, String 등

이 두 개는 반복문을 편하게 해줌... (파이썬 for 처럼)



## 6.2. while

- 조건문이 참인 동안 반복 시행
- 조건은 소괄호 안에 작성, 실행할 코드는 중괄호 안에 작성
- 블록 스코프 생성



## 6.3. for

- 세미콜론(;)으로 구분되는 세 부분으로 구성 - 여기서는 가변적으로 붙히는 게 아닌, 무조건 써야 한다.

1. initialization
   - 최초 반복문 진입 시 1회만 실행되는 부분
2. condition
   - 매 반복 시행 전 평가되는 부분
3. expression
   - 매 반복 시행 이후 평가되는 부분

- 블록 스코프 생성

```javascript
for (initialization; condition; expression) {
    // do something
}
// 예시
for (let i = 0; i < 10; i++) {
    console.log(i)	// 0부터 9까지 출력
}
```



## 6.4. for ... in

- 객체(object)의 속성(key)들을 순회할 때 사용
- 배열도 순회 가능하지만 **권장하지 않음**
- 실행할 코드는 중괄호 안에 작성
- 블록 스코프 생성

```javascript
// object -> key-value로 이루어진 자료구조 (객체에서 마저...) 파이썬의 딕셔너리와 유사
const capitals = {
    korea: 'seoul'
    france: 'paris'
    USA: 'washington D.C.'
}

for (let capital in capitals) {
    console.log(capital) // korea, france, USA
}
```



## 6.5. for ... of

- **반복 가능한(iterable) 객체를 순회하며 값을 꺼낼 때 사용**
- 실행할 코드는 중괄호 안에 작성
- 블록 스코프 생성

```javascript
const fruits = ['딸기', '바나나', '메론']

for (let fruit of fruits) {
    fruit = fruit + '!'
    console.log(fruit)	// '딸기!' '바나나!' '메론!'
}

for (const fruit of fruits) {
    console.log(fruit)	// fruit 재할당 불가, (fruit + '!') 하면 위와 같은 출력 가능
}
```

![image-20220425200648795](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220425200648795.png)

(주의) for in의 객체는 Class의 Instance가 아닙니다. **자바스크립트에서의 객체**는 **파이썬의 딕셔너리** 같은 것입니다!



교수님의 한마디: for of를 자주 쓰게 될 것

알고리즘 문제를 푸는 게 아닌 이상 웬만한 변수는 const를 하는 게 낫고, 필요하다면 let을 사용하자.. (쓸 일이 손에 꼽을 정도)



## 6.6. 조건문과 반복문 정리

| 키워드     | 종류   | 연간 키워드              | 스코프      |
| ---------- | ------ | ------------------------ | ----------- |
| if         | 조건문 |                          | 블록 스코프 |
| switch     | 조건문 | case, **break**, default | 블록 스코프 |
| while      | 반복문 | break, continue          | 블록 스코프 |
| for        | 반복문 | break, continue          | 블록 스코프 |
| for ... in | 반복문 | 객체 순회                | 블록 스코프 |
| for ... of | 반복문 | 배열 등 Iterable 순회    | 블록 스코프 |

