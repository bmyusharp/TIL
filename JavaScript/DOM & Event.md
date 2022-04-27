[TOC]

목차

- DOM
- Event Listener

# 0. History of JavaScript

## 0.1. 핵심 인물

- 팀 버너스리
  - www, url, http, html 최초 설계자
  - 웹의 아버지
- 브랜던 아이크
  - javascript 최초 설계자
  - 모질라 재단 공동 설립자
  - 코드네임 피닉스 프로젝트 진행
    - 파이어폭스의 전신

## 0.2. JavaScript의 탄생

- 1994년 당시 넷스케이프 커뮤니케이션스사의 Netscape Navigator(NN) 브라우저가 전 세계 점유율 80% 이상 독점하며 브라우저의 표준 역할을 함
- 당시 넷스케이프에 재직 중이던 브랜던 아이크가 HTML을 동적으로 동작하기 위한 회사 내부 프로젝트를 진행 중 JS를 개발
- JavaScript 이름 변천사
  - Mocha -> LiveScript -> JavaScript (1995)
- 그러나 1995년 경쟁자 마이크로소프트에서 이를 채택하여 커스터마이징한 JScript를 만듦
- 이를 IE 1.0의 탑재 -> 1차 브라우저 전쟁의 시작

## 0.3. 제1차 브라우저 전쟁

- 넷스케이프 vs 마이크로소프트
- 빌 게이츠 주도하에 MS는 1997년 IE 4를 발표하면서 시장을 장악하기 시작
  - 당시 윈도우 OS의 시장 점유율은 90%
  - 글로벌 기업 MS의 공격적인 마케팅
- MS의 승리로 끝나며 2001년부터 IE의 점유율은 90%를 상회
- 1998년 넷스케이프에서 나온 브랜던 아이크 외 후계자들은 모질라 재단을 설립
  - 파이어폭스를 통해 IE에 대항하며 꾸준히 점유율을 올려 나감
- MS의 폭발적 성장, IE3에서 자체적은 JScript를 지원, 호환성 문제로 크로스 브라우징 등의 이슈 발생 (같은 기능인데, 언어는 두 가지(JScript와 JavaScript))
- 이후 넷스케이프 후계자들은 모질라 재단 기반의 파이어폭스를 개발

## 0.4. 제2차 브라우저 전쟁

- MS vs Google
- 2008년 Google의 Chrome 브라우저 발표
- 2011년 3년 만에 파이어폭스의 점유율을 돌파 후 2012년부터 전 세계 점유율 1위 등록
- 크롬의 승리 요인
  - 압도적인 속도
  - 강력한 개발자 도구 제공
  - 웹 표준

## 0.5. 파편화와 표준화

- 제1차 브라우저 전쟁 이후 수많은 브라우저에서 자체 자바스크립트 언어를 사용하게 됨
- 결국 서로 다른 자바스크립트가 만들어지면서 크로스 브라우징 이슈가 발생하여 웹 표준의 필요성이 제기
- 크로스 브라우징 (Cross Browsing)
  - W3C에서 채택된 표준 웹 기술을 채용하여 각각의 브라우저마다 다르게 구현되는 기술을 비슷하게 만들되, 어느 한쪽에 치우치지 않도록 웹 페이지를 제작하는 방법론 (동일성이 아닌 동등성)
  - 브라우저마다 렌더링에 사용하는 엔진이 다르기 때문

- 1996년부터 넷스케이프는 표준 제정의 필요성을 주장
  - ECMA 인터내셔널 (정보와 통신 시스템을 위한 국제적 표준화 기구) 에 표준 제정 요청
- 1997년 ECMAScript 1 (ES1) 탄생 ~~그시절 용산~~
- 제1차 브라우저 전쟁 이후 제기된 언어의 파편화를 해결하기 위해 각 브라우저 회사와 재단은 표준화에 더욱 적극적으로 힘을 모으기 시작

## 0.6. JavaScript ES6+

![image](https://www.sencha.com/wp-content/uploads/2016/10/ecmascript2015-senchacon-img2.png)

- 2015년 ES2015 (ES6) 탄생
  - Next-gen of JS
  - JavaScript의 고질적인 문제들을 해결
  - JavaScript의 다음 시대라고 불릴 정도로 많은 혁신과 변화를 맞이한 버전
  - 이때부터 버전 순서가 아닌 출시 연도를 붙히는 것이 공식 명칭이나 통상적으로  ES6 라 부름
  - 현재는 표준 대부분이 ES6+로 넘어옴



# 1. DOM

## 1.1. 브라우저에서 할 수 있는 일

- DOM 조작
  - 문서(HTML) 조작
- BOM 조작
  - navigator, screen, location, frames, history, XHR
- JavaScript Core (ECMAScript)
  - Data Structure(Object, Array), Conditional Expression, Iteration



## 1.2. DOM 조작

### 1.2.1. 개념

- Document는 문서 한 장(HTML)에 해당하고 이를 조작
- DOM 조작 순서
  1. 선택 (Select)
  2. 변경 (Manipulation)

### 1.2.2. DOM 관련 객체의 상속 구조

- EventTarget
  - Event Listener를 가질 수 있는 객체가 구현하는 DOM 인터페이스
- Node
  - 여러 가지 DOM 타입들이 상속하는 인터페이스
- Element
  - Document 안의 모든 객체가 상속하는 가장 범용적인 인터페이스
  - 부모인 Node와 그 부모인 EventTarget의 속성을 상속

- **Document**
  - 브라우저가 불러온 웹 페이지를 나타냄
  - DOM 트리의 진입점(entry point) 역할을 수행
- **HTMLElement**
  - 모든 종류의 HTML 요소
  - 부모 element의 속성 상속

### 1.2.3. 선택 관련 메서드

- **document.querySelector(selector)**
  - 제공한 선택자와 일치하는,  CSS selector를 만족시키는 첫 번째 element 선택 (없다면 null)
  - id 등 유일한 값을 선택할때 쓰는 게 좋음
- **querySelectorAll(selector)**
  - 제공한 선택자와 일치하는 여러 element를 선택
  - 매칭할 하나 이상의 셀렉터를 포함하는 유효한 CSS selector를 인자(문자열) 로 받음
  - 지정된 셀렉터에 일치하는 NodeList를 반환

- getElementById(id), getElementByTagName(name) ... 보다 위 두개를 사용하자!
  - id, class 그리고 tag 선택자 등을 모두 사용 가능하므로 더 구체적이고 유연하게 선택 가능하다.
  - getElementById(id)와 document.querySelector('#id')는 같은 걸 선택한다.



#### 1.2.3.1. 선택 메서드별 반환 타입

1. 단일 element
   - ~~getElementById()~~
   - **querySelector()**
2. HTMLCollection
   - ~~getElementsByTagName()~~
   - ~~getElementsByClassName()~~
3. Nodelist
   - **querySelectorAll()**



#### 1.2.3.2. HTMLCollection & NodeList

- 둘 다 배열과 같이 각 항목에 접근하기 위한 index를 제공 (유사 배열)
- **HTMLCollection**
  - name, id, index 속성으로 각 항목에 접근 가능
- **NodeList**
  - index로만 각 항목에 접근 가능
  - 단, HTMLCollection과 달리 배열에서 사용하는 forEach 메서드 및 다양한 메서드 사용 가능
- 둘 다 Live Collection으로 DOM의 변경사항을 실시간으로 반영하지만, **querySelectorAll() 에 의해 반환되는 NodeList는 Static Collection으로 실시간으로 반영되지 않음**



#### 1.2.3.3. Collection

- **Live Collection**
  - 문서가 바뀔 때 실시간으로 업데이트 됨
  - DOM의 변경사항을 실시간으로 collection에 반영
  - ex) HTMLCollection, NodeList
- **Static Collection (non-live)**
  - DOM이 변경되어도 collection 내용에는 영향을 주지 않음
  - querySelectorAll()의 반환 NodeList만 static collection



### 1.2.4. 변경 관련 메서드

- document**.createElement()**
  - 작성한 태그 명의 HTML 요소를 생성하여 반환

- Element**.append()**
  - 특정 부모 Node의 자식 NodeList 중 마지막 자식 다음에 Node 객체나 DOMString을 삽입
  - 여러 개의 Node 객체, DOMString을 추가 할 수 있음
  - 반환 값이 없음
- Node**.appendChild()**
  - 한 Node를 특정 부모 Node의 자식 NodeList 중 마지막 자식으로 삽입 (Node만 추가 가능)
  - 한번에 오직 하나의 Node만 추가할 수 있음
  - 만약 주어진 Node가 이미 문서에 존재하는 다른 Node를 참조한다면 새로운 위치로 이동

![image-20220427111032902](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220427111032902.png)

![image-20220427111535677](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220427111535677.png)



#### 1.2.4.1. ParentNode.append() vs Node.appendChild()

|      | ParentNode.append()                   | Node.appendChild()               |
| ---- | ------------------------------------- | -------------------------------- |
|      | DOMString 객체 추가 가능              | Node 객체만 허용                 |
|      | 반환 값 **없음**                      | **추가된 Node 객체 반환**        |
|      | **여러** Node 객체와 문자열 추가 가능 | **하나의** Node 객체만 추가 가능 |



### 1.2.5. 변경 관련 속성

- Node**.innerText**
  - Node 객체와 그 자손의 텍스트 컨텐츠 (DOMString) 를 표현 (해당 요소 내부의 raw text) (사람이 읽을 수 있는 요소만 남김)
  - 즉, 줄 바꿈을 인식하고 숨겨진 내용을 무시하는 등 최종적으로 스타일링이 적용된 모습으로 표현
- Element**.innerHTML**
  - 요소(element) 내에 포함된 HTML 마크업을 반환
  - [참고] XSS 공격에 취약하므로 사용 시 주의

![image-20220427121558666](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220427121558666.png)

#### 1.2.5.1. XSS (Cross-Site Scripting)

- 공격자가 입력요소를 사용하여 (\<input>) 웹 사이트 클라이언트 측 코드에 악성 스크립트를 삽입해 공격하는 방법
- 피해자(사용자) 의 브라우저가 악성 스크립트를 실행하며 공격자가 엑세스 제어를 우회하고 사용자를 가장할 수 있도록 함

![image-20220427121743649](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220427121743649.png)



### 1.2.6. 삭제 관련 메서드

- ChildNode**.remove()**
  - Node가 속한 트리에서 해당 Node를 제거
- Node**.removeChild()**
  - DOM에서 자식 Node를 제거하고 제거된 Node를 반환
  - Node는 인자로 들어가는 자식 Node의 부모 Node



### 1.2.7. 속성 관련 메서드

- Element**.setAttribute(name, value)**
  - 지정된 요소의 값을 설정
  - 속성이 이미 존재하면 값을 갱신, 존재하지 않으면 지정된 이름과 값으로 새 속성을 추가
- Element**.getAttribute(attributeName)**
  - 해당 요소의 지정된 값(문자열)을 반환
  - 인자(attributeName)는 값을 얻고자 하는 속성의 이름

![image-20220427124100356](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220427124100356.png)

![image-20220427124119525](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220427124119525.png)





## 1.3. DOM 조작 정리

**1. [선택한다.](#1.2.3. 선택 관련 메서드)** -> **2. [정리한다.](#1.2.4. 변경 관련 메서드)**

![image-20220427124336972](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220427124336972.png)







# 2. Event

