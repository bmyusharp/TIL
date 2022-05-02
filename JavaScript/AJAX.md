[TOC]

이전: [DOM & Event (브라우저 전쟁의 역사, DOM 조작법과 Event)](./DOM%20&%20Event.md)

현재: AJAX (비동기, Promise, Axios)

다음: 없음

# 1. AJAX

## 1.1. AJAX란?

- Asynchronous JavaScript And XML (비동기식 JavaScript와 XML)
- 서버와 통신하기 위해 **XMLHttpRequest** 객체를 활용
- JSON, XML, HTML 그리고 일반 텍스트 형식 등을 포함한 다양한 포맷을 주고받을 수 있음
  - [참고] AJAX의 X가 XML을 의미하긴 하지만, 요즘은 더 가벼운 용량과 JavaScript의 일부라는 장점 때문에 JSON을 더 많이 사용함
  - XML = eXtended Markup Language

## 1.2. AJAX 특징

- 페이지 전체를 reload(새로고침) 를 하지 않고서도 수행되는 "**비동기성**"

  - 서버의 응답에 따라 전체 페이지가 아닌 일부분만을 업데이트 할 수 있음

- AJAX의 주요 두 가지 특징은 아래의 작업을 할 수 있게 해줌

  1. 페이지 새로 고침 없이 서버에 요청
  2. 서버로부터 데이터를 받고 작업을 수행

  -> HTS같은 실시간 주가반영이라던가...? 연관검색어 자동완성

![image-20220502102420476](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502102420476.png)

## 1.3. XMLHttpRequest 객체

- = XHR

- 서버와 상호작용하기 위해 사용되며 전체 페이지의 새로 고침 없이 데이터를 받아올 수 있음
- 사용자의 작업을 방해하지 않으면서 페이지 일부를 업데이트 할 수 있음
- 주로 AJAX 프로그래밍에 사용
- 이름과 달리 XML뿐만 아니라 모든 종류의 데이터를 받아올 수 있음
- 생성자
  - XMLHttpRequest()

### 1.3.1. XHR 예시

- console에 todo 데이터가 출력되지 않음
- 데이터 응답을 기다리지 않고 console.log() 를 먼저 실행했기 때문

```javascript
const request = new XMLHttpRequest()
const URL = 'htts://jsonplaceholder.typicode.com/todos/1/'

request.open('GET', URL)
request.send()

const todo = request.response
console.log('data: ${todo}')
```

### 1.3.2. JSON Placeholder

[사이트](https://jsonplaceholder.typicode.com/todos/1/)

- 더미 데이터 JSON 만들어놓은 사이트



# 2. Asynchronous JavaScript

## 2.1. 동기/비동기
### 2.1.1. 동기식

- 순차적, 직렬적 Task 수행
- 요청을 보낸 후 응답을 받아야만 다음 동작이 이루어짐 (blocking)
- 버튼 클릭 후 alert 메시지의 확인 버튼을 누를 때까지 문장이 만들어지지 않음
- 즉, alert 이후의 코드는 alert의 처리가 끝날 때까지 실행되지 않음
- 왜 이런 현상이 발생할까?

![image-20220502104351201](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502104351201.png)

![image-20220502104353868](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502104353868.png)

### 2.1.2. 비동기식

- 병렬적 Task 수행
- 요청을 보낸 후 응답을 기다리지 않고 다음 동작이 이루어짐 (non-blocking)
- 요청을 보내고 응답을 기다리지 않고 다음 코드가 실행됨
- 결과적으로 변수 todo에는 응답 데이터가 할당되지 않고 빈 문자열이 출력
- 그렇다면 JS는 왜 기다려주지 않는 방식으로 동작하는가?
  - "JavaScript는 single threaded"

![image-20220502104521560](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502104521560.png)

![image-20220502104524071](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502104524071.png)

- `request.send()`가 비동기식으로 되는 것..

### 2.1.3. 왜 비동기(Asynchronous) 를 사용하는가?

**"사용자 경험"**

- 매우 큰 데이터를 동반하는 앱이 있다고 가정
- 동기식 코드라면 데이터를 모두 불러온 뒤 앱이 실행됨
  - 즉, 데이터를 모두 불러올 때 까지는 앱이 모두 멈춘 것처럼 보임
  - 코드 실행을 차단하여 화면이 멈추고 응답하지 않는 것 같은 사용자 경험을 제공
- 비동기식 코드라면 데이터를 요청하고 응답 받는 동안, 앱 실행을 함께 진행함
  - 데이터를 불러오는 동안 지속적으로 응답하는 화면을 보여줌으로써 더욱 쾌적한 사용자 경험을 제공
- 때문에 많은 웹 API 기능은 현재 비동기 코드를 사용하여 실행됨

#### 2.1.3.1. [참고] Threads

- 프로그램이 작업을 완료하기 위해 사용할 수 있는 단일 프로세스
- 각 thread (스레드) 는 한 번에 하나의 작업만 수행할 수 있음
- 예시) Task A -> Task B -> Task C
  - 다음 작업을 시작하려면 반드시 앞의 작업이 완료되어야 함
  - 컴퓨터 CPU는 여러 코어를 가지고 있기 때문에 한 번에 여러 가지 일을 처리할 수 있음
- 스레드 = 일꾼 / 탭

## 2.2. Blocking vs. Non-Blocking

![image-20220502110738143](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502110738143.png)

- 파이썬은 하나가 끝날 때까지 넘어갈 수 없다

- "어떤 몇몇" 코드가 비동기 유발 (`request.send()` 같은 녀석들)

## 2.3. "JavaScript는 single threaded 이다."

- 컴퓨터가 여러 개의 CPU를 가지고 있어도 main thread라 불리는 단일 스레드에서만 작업 수행

- 즉, 이벤트를 처리하는 **Call Stack**이 하나인 언어라는 의미

- 이 문제를 해결하기 위해 JavaScript는

  1. Web API = 브라우저 내부의 고마운 식기세척기) 으로 보내서 처리하도록 하고,

  2. 처리된 이벤트들은 처리된 순서대로 **대기실(Task queue)**에 줄을 세워 놓고

  3. Call Stack이 비면 **담당자(Event Loop)**가 대기 줄에서 가장 오래된 (제일 앞의) 이벤트를 Call Stack 으로 보냄

### 2.3.1. Concurrency model

- Event loop를 기반으로 하는 **동시성 모델(Concurrency model)
  1. Call Stack
     - 요청이 들어올 때마다 해당 요청을 순차적으로 처리하는 Stack(LIFO) 형태의 자료 구조
  2. Web API (Browser API)
     - JavaScript 엔진이 아닌 브라우저 영역에서 제공하는 API
     - **setTimeout(), DOM events 그리고 AJAX로 데이터를 가져오는 시간이 소요되는 일들을 처리**
  3. Task Queue (Event Queue, Message Queue)
     - 비동기 처리된 callback 함수가 대기하는 Queue(FIFO) 형태의 자료 구조
     - main thread가 끝난 후 실행되어 후속 JavaScript 코드가 차단되는 것을 방지
  4. Event Loop
     - Call Stack이 비어 있는지 확인
     - 비어 있는 경우 Task Queue에서 실행 대기 중인 callback 함수가 있는지 확인
     - Task Queue에 대기 중인 callback 함수가 있다면 가장 앞에 있는 callback 함수를 Call Stack으로 push

### 2.3.2. Runtime

![image-20220502112104563](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502112104563.png)

![image-20220502112117018](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502112117018.png)

![image-20220502112130854](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502112130854.png)

![image-20220502112209225](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502112209225.png)

![image-20220502112226763](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502112226763.png)

![image-20220502112252094](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502112252094.png)

 Task Queue는 일종의 대기줄. 여기에 들어오는 건 콜백 함수(ex. ssafy()) 이고 setTimeout()이 아님

![image-20220502112341306](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502112341306.png)

![image-20220502112357489](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502112357489.png)

![image-20220502112411898](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502112411898.png)

### 2.3.3. Zero delays

setTimeout에 0을 넣어도, Web API - Task Queue를 거쳐서 Call Stack에 올라감

![image-20220502112722735](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502112722735.png)

setTimeout보다 매우 오래걸리는 작업이 다음 줄에 있더라도, 그 작업을 먼저 실행하고, setTimeout 콜백함수 실행 (이 콜백함수가 2순위가 되기 때문이다.)

- 실제로 0ms 후에 callback 함수가 시작된다는 의미가 아님
- 실행은 Task Queue에 대기 중인 작업 수에 따라 다르며 해당 예시에서는 callback 함수의 메시지가 처리되기 전에 'Hi'와 'Bye'가 먼저 출력됨
- 왜냐하면 delay (지연) 은 JavaScript가 요청을 처리하는 데 필요한 최소 시간이기 떄문 (보장된 시간이 아님)
- 기본적으로 `setTimeout` 함수에 특정 시간제한을 설정했더라도 대기 중인 메시지의 모든 코드가 완료된 때까지 대기해야 함

### 2.3.4. 순차적인 비동기 처리하기

- Web API로 들어오는 순서는 중요하지 않고, 어떤 이벤트가 **먼저** 처리되느냐가 중요 (즉, 실행 순서 불명확)
- 이를 해결하기 위해 순차적인 비동기 처리를 위한 2가지 작성 방식

1. Async callbacks
   - 백그라운드에서 실행을 시작할 함수를 호출할 때 인자로 지정된 함수
   - 예시) addEventListener() 의 두 번째 인자
2. promise-style
   - Modern Web APIs에서의 새로운 코드 스타일
   - XMLHttpRequest 객체를 사용하는 구조보다 조금 더 현대적인 버전



# 3. Callback Function

## 3.1. 콜백함수란?

- 다른 함수에 인자로 전달된 함수
- 외부 함수 내에서 호출되어 일종의 루틴 또는 작업을 완료함
- 동기식, 비동기식 모두 사용됨
  - 그러나 비동기 작업이 완료된 후 코드 실행을 계속하는 데 주로 사용됨
- 비동기 작업이 완료된 후 코드 실행을 계속하는 데 사용되는 경우를 비동기 콜백이라고 함 (asynchronous callback)

## 3.2. 일급 객체

> JavaScript의 함수는 "일급 객체 (First Class Object)다."

- 일급 객체 (일급 함수)
  - 다른 객체들에 적용할 수 있는 연산을 모두 지원하는 객체(함수)
- 일급 객체의 조건
  - 인자로 넘길 수 있어야 함
  - 함수의 반환 값으로 사용할 수 있어야 함
  - 변수에 할당할 수 있어야 함

사용 예시

```javascript
const btn = document.querySelector('button')

btn.addEventListener('click', function() {
    alert('Completed!') // function(){} 부분
})
```

```python
numbers = [1, 2, 3]

def add_one(number):
    return number + 1

print(map(add_one, numbers)) # add_one 부분
```

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index), # views.index 부분
]
```

## 3.3. Async callbacks

- 백그라운드에서 코드 실행을 시작할 함수를 호출할 때 인자로 지정된 함수

- 백그라운드 코드 실행이 끝나면 callback 함수를 호출하여 작업이 완료되었음을 알리거나, 다음 작업을 실행할 수 있음

  - 사용 예시) addEventListner() 의 두 번째 매개변수

- callback 함수를 다른 함수의 인수로 전달할 때, 함수의 참조를 인수로 전달할 뿐이지 즉시 실행되지 않고, 함수의 body에서 "called back"됨.

  정의된 함수는 때가 되면 callback 함수를 실행하는 역할을 함

### 3.3.1. Why use callback?

- callback 함수는 명시적인 호출이 아닌 특정 루틴 혹은 action에 의해 호출되는 함수
- Django의 경우 "요청이 들어오면", event의 경우 "특정 이벤트가 발생하면" 이라는 조건으로 함수를 호출할 수 있었던 건 'Callback function' 개념 때문에 가능
- 비동기 로직을 수행할 때 callback 함수는 필수
  - 명시적인 호출이 아니라 다른 함수의 매개변수로 전달하여 해당 함수 내에서 특정 시점에 호출

### 3.3.2. callback hell

- 순차적인 연쇄 비동기 작업을 처리하기 위해 "callback 함수를 호출하고, 그다음 callback 함수를 호출하고, 또 그 함수의 callback..." 의 패턴이 지속적으로 반복됨
- 즉, 여러 개의 연쇄비동기 작업을 할 때 마주하는 상황
- 이를 **callback Hell (콜백 지옥)** 혹은 pyramid of doom (파멸의 피라미드) 이라 함
- 위와 같은 상황이 벌어질 경우 디버깅 및 코드 가독성이 통제하기 어려움

#### 3.3.2.1. callback hell 해결하기

1. 코드의 깊이를 얕게 유지 (Keep your code shallow)
2. 모듈화 (Modularize)
3. 모든 단일 오류 처리 (Handle every single error)
4. **Promise 콜백 방식 사용 (Promise callbacks)**



# 4. Promise

## 4.1. Promise object

- 비동기 작업의 최종 완료 또는 실패를 나타내는 객체
  - 미래의 완료 또는 실패와 그 결과 값을 나타냄
  - 미래의 어떤 상황에 대한 약속
- 성공(이행)에 대한 약속
  - **`.then()`**
- 실패(거절)에 대한 약속
  - **`.catch()`**

```javascript
const myPromise = axios.get(URL)
// console.log(myPromise) // Promise Object

myPromise
  .then(response => {
    return response.data
})

// chaining
axios.get(URL)
  .then(response => {
    return response.data
  })
  .then(response => {
    return response.title
  })
  .catch(error => {
    console.log(error)
  })
  .finally(function () {
    console.log('나는 마지막에 무조건 시행!!!')
  })
```



## 4.2. Promise methods 

- **`.then(callback)`**
  - 이전 작업(promise) 이 성공했을 때(이행했을 때) 수행할 작업을 나타내는 callback 함수
  - 그리고 각 callback 함수는 이전 작업의 성공 결과를 인자로 전달받음
  - 따라서 성공했을 때의 코드를 callback 함수 안에 작성
- **`.catch()`**
  - .then이 하나라도 실패하면 (거부 되면) 동작 (동기식의 'try - except' 구문과 유사)
  - 이전 작업의 실패로 인해 생성된 error 객체는 catch 블록 안에서 사용할 수 있음

- 각각의 `.then()` 블록은 서로 다른 promise를 반환
  - 즉, `.then()` 을 여러 개 사용 (chaining) 하여 연쇄적인 작업을 수행할 수 있음
  - 결국 여러 비동기 작업을 차례대로 수행할 수 있다는 뜻
- `.then()` 과 `.catch()` 메서드는 모두 promise를 반환하기 때문에 chaining 가능
- 주의
  - **반환 값이 반드시 있어야 함**
  - 없다면 callback 함수가 이전의 promise 결과를 받을 수 없음

- **`.finally(callback)`**
  - Promise 객체를 반환
  - 결과와 상관없이 무조건 지정된 callback 함수가 실행
  - 어떠한 인자도 전달받지 않음
    - Promise가 성공인지 거절인지 판단 X
  - 무조건 실행되어야 하는 절에서 활용
    - `.then()` 과 `.catch()` 블록에서의 코드 중복을 방지

![image-20220502170912549](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220502170912549.png)

## 4.3. Promise가 보장하는 것

- Async callback 작성 스타일과 달리 Promise가 보장하는 특징

1. callback 함수는 JavaScript의 Event Loop 가 현재 실행 중인 Call Stack을 완료하기 이전에는 절대 호출되지 않음
   - Promise callback 함수는 Event Queue에 배치되는 엄격한 순서로 호출됨
2. 비동기 작업이 성공하거나 실패한 뒤에 `.then()` 메서드를 이용하여 추가한 경우에도 1번과 똑같이 동작
3. `.then()`을 여러 번 사용하여 여러 개의 callback 함수를 추가할 수 있음 (Chaining)
   - 각각의 callback은 주어진 순서대로 하나하나 실행하게 됨
   - Chaining은 Promise의 가장 뛰어난 장점



# 5. Axios

## 5.1. Axios

> Promise based HTTP client for the browser

- 브라우저를 위한 Promise 기반의 클라이언트
- 원래는 "XMR"이라는 브라우저 내장 객체를 활용해 AJAX 요청을 처리하는데, 이보다 편리한 AJAX 요청이 가능하다록 도움을 줌
  - 확장 가능한 인터페이스와 함께 패키지로 사용이 간편한 라이브러리를 제공

```javascript
axios.get('https://jsonplaceholder.typicode.com/todos/1/')
// Promise return
  .then(..)
  .catch(..)
```

## 5.2. XMLHttpRequest -> Axios 변경

- XMLHttpRequest

```html
<script>
  const xhr = new XMLHttpRequest()
  const URL = 'https://jsonplaceholder.typicode.com/todos/1/'

  xhr.open('GET', URL)
  xhr.send()

  xhr.onreadystatechange = function (event) {
    if (xhr.readyState === XMLHttpRequest.DONE) {
      const status = event.target.status
      if (status === 0 || (status >= 200 && status < 400)) {
        const res = event.target.response
        const data = JSON.parse(res)
        console.log(data.title)
      } else {
        console.error('Error!')
      }
    }
  }
</script>
```

- Axios

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>

  const URL = 'https://jsonplaceholder.typicode.com/todos/1/'

  axios.get(URL)
    .then(res => console.log(res.data.title))
    .catch(err => console.log('Error!'))
    
</script>
```



## 5.3. Axios 예시

```javascript
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>

  const URL = 'https://jsonplaceholder.typicode.com/todos/1/'

  axios.get(URL)
    .then(function (response) {
      console.log(response)
      return response.data
    })
    .then(function (data) {
      return data.title 
    })
    .then(function (title) {
      console.log(title)
    })
    .catch(function (error) {
      console.log(error)
    })
    .finally(function () {
      console.log('이건 무조건 실행됩니다.')
    })

</script>
```



# 6. 부록

## 6.1. async & await

비동기 코드를 작성하는 새로운 방법

- ECMAScript 2017 (ES8) 에서 등장

기존 Promise 시스템 위에 구축된 syntatic sugar

- Promise 구조의 then chaining을 제거
- 비동기 코드를 조금 더 동기 코드처럼 표현
- **Syntatic sugar**
  - 더 쉽게 읽고 표현할 수 있도록 설계된 프로그래밍 언어 내의 구문
  - 즉, 문법적 기능은 그대로 유지하되 사용자가 직관적으로 코드를 읽을 수 있게 만듦



## 6.2. Promise 에서 async & await 적용

.

```javascript
  const URL = 'https://dog.ceo/api'

  function fetchFirstDogImage() {
    axios.get(URL + '/breeds/list/all')
      .then(res => {
        const breed = Object.keys(res.data.message)[0]
        return axios.get(URL + '/breed/${breed}/images')
      })
      .then(res => {
        console.log(res)
      })
      .catch(err => {
        console.error(err.response)
      })
  }

  fetchFirstDogImage()
```

- async & await

```javascript
const URL = 'https://dog.ceo/api'

async function fetchFirstDogImage() {
  const res = await axios.get(URL + '/breeds/list/all/')
  const breed = Object.keys(res.data.message)[0]
  const images = await axios.get(URL + '/breed/${breed}/imagees')
  console.log(images)
}

fetchFirstDogImage()
  .catch(err => console.error(err.response))
```







# 같이 보기

이전: [DOM & Event (브라우저 전쟁의 역사, DOM 조작법과 Event)](./DOM%20&%20Event.md)

현재: AJAX (비동기, Promise, Axios)

다음: 없음
