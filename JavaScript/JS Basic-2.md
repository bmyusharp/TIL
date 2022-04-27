[TOC]

이전: [JS Basic-1 (기초, 변수, 연산자, 데이터 타입, 조건문, 반복문)](./JS%20Basic-1.md))

현재: JS Basic-2 (함수, 문자열, 배열, 객체, this, lodash)

다음: 없음

# 6. 함수

## 6.1. 자바스크립트에서의 함수

- 참조 타입 중 하나로써 function 타입에 속함

- JavaScript에서 함수를 정의하는 방법은 주로 2가지로 구분

  - 함수 선언식 (function declaration)
  - 함수 표현식 (function expression)

- (참고) JavaScript의 함수는 **일급 객체* (First-class citizen)**에 해당

  - 일급 객체*: 다음의 조건들을 만족하는 객체를 의미함

    파이썬과 같다고 함... (데코레이터 中)

    - 변수에 할당 가능
    - 함수의 매개변수로 전달 가능
    - 함수의 반환 값으로 사용 가능



## 6.2. 함수 선언식

- 함수의 이름과 함께 정의하는 방식
- 3가지 부분으로 구성
  - 함수의 이름 (name)
  - 매개변수 (args)
  - 몸통 (중괄호 내부)

```javascript
function name(args) {
    // do something
}
function add(num1, num2) {
    return num1 + num2
}

add(1, 2)	// 3
```



## 6.3. 함수 표현식

- 함수를 표현식* 내에서 정의하는 방식
  - (참고) 표현식*: 어떤 하나의 값으로 결정되는 코드의 단위
- 함수의 이름을 생략하고 익명 함수* 로 정의 가능
  - 익명 함수* (annonymous function): 이름이 없는 함수
  - 익명 함수는 함수 표현식에서만 가능
- 3가지 부분으로 구성
  - 함수의 이름 **(생략 가능)**
  - 매개변수 (args)
  - 몸통 (중괄호 내부)

```javascript
const name = function (Args) {
    // do something
}

const add = function (num1, num2) {
    return num1 + num2
}

add(1, 2)
```

그냥 함수를 정의하는 것은 var로 변수를 정의하는 것과 같다.

그래서, const로 함수 재정의를 방지하고 사용하기도 한다.



## 6.4. 기본 인자 (default arguments)

- 인자 작성 시 '=' 문자 뒤 기본 인자 선언 가능

```javascript
const greeting = function (name = 'Anonymous') {
    return 'Hi ${name}'
}

greeting() // Hi Anonymous
```



## 6.5. 매개변수와 인자의 개수 불일치 허용

- 매개변수보다 인자의 개수가 많을 경우

  ```javascript
  const noArgs = function () {
      return 0
  }
  
  noArgs(1, 2, 3)	// 0
  
  const toArgs = function (arg1, arg2) {
      return [arg1, arg2]
  }
  
  twoArgs(1, 2, 3)	// [1, 2]
  ```

  뒤 값을 안 씀

- 매개변수보다 인자의 개수가 적을 경우

  ```javascript
  const threeArgs = function (arg1, arg2, arg3) {
      return [arg1, arg2, arg3]
  }
  
  threeArgs()			// [undifined, undifined, undifined]
  threeArgs(1)		// [1, undifined, undifined]
  threeArgs(1, 2)		// [1, 2, undifined]
  ```

  값에 undifined를 넣음



## 6.6. Rest operator

- **rest operator(...)** 를 사용하면 함수가 정해지지 않은 수의 매개변수를 배열로 받음 (= 파이썬의 *args와 유사)
  - 만약 rest operator로 처리한 매개변수에 인자가 넘어오지 않을 경우에는, 빈 배열로 처리

```javascript
const restOpr = function (arg1, arg2, ...restArgs) {
    return [arg1, arg2, restArgs]
}

restArgs(1, 2, 3, 4, 5)		// [1, 2, [3, 4, 5]]
restArgs(1, 2)				// [1, 2, []]
```



## 6.7. Spread operator

- **spread operator(...)**를 사용하면배열 인자를 전개하여 전달 가능

```javascript
const spreadOpr = function (arg1, arg2, arg3) {
    return arg1 + arg2 + arg3
}

const numbers = [1, 2, 3]
spreadOpr(...numbers)		// 6
```





## 6.8. 선언식 vs 표현식

- 함수 선언식과 표현식 비교 정리

![image-20220426102725420](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220426102725420.png)



- 함수의 타입
  - 선언식 함수와 표현식 함수 모두 타입은 function으로 동일

```javascript
// 함수 표현식
const add = function (args) { }

// 함수 선언식
function sub(args) { }

console.log(typeof add)	// function
console.log(typeof sub)	// function
```

- 호이스팅 (hoisting) - 함수 선언식
  - 함수 선언식으로 선언한 함수는 var로 정의한 변수처럼 hoisting 발생
  - 함수 호출 이후에 선언 해도 동작

```javascript
add(2, 7)	// 9

function add (num1, num2) {
    return num1 + num2
}
```

- 호이스팅 (hoisting) - 함수 표현식
  - 반면 **함수 표현식**으로 선언한 함수는 **함수 정의 전에 호출 시 에러 발생**
  - 함수 표현식으로 정의된 함수는 **변수로 평가되어 변수의 scope 규칙을 따름**

```javascript
sub(7, 2)	// Uncaught RefferenceError: Cannot access 'sub' before initialization

const sub = function (num1, num2) {
    return num1 - num2
}
```

- (참고) 함수 표현식을 var 키워드로 작성한 경우, 변수가 선언 전 **undefined 로 초기화 되어 다른 에러가 발생**

```javascript
console.log(sub)	// undefined
sub(7, 2)			// Uncaught TypeError: sub is not a function

var sub = function (num1, num2) {
    return num1 - num2
}
```





## 6.9. Arrow Function

- 함수를 비교적 간결하게 정의할 수 있는 문법
- **function 키워드 생략 가능**
- 함수의 **매개변수가 단 하나 뿐이라면, '( )' 도 생략 가능**
- 함수 **몸통이 표현식 하나라면 '{ }'과 return도 생략 가능**
- 기존 function 키워드 사용 방식과의 **차이점은 후반부 this 키워드를 학습하고 다시 설명하겠음.**
  - 정말 많이 쓴다고 알아두자.

```javascript
const arrow1 = function (name) {
    return 'hello, ${name}'
}

// 1. function 키워드 삭제
const arrow2 = (name) => { return 'hello, ${name}' }

// 2. 매개변수가 1개일 경우에만 ( ) 생략 가능
const arrow3 = name => {return 'hello, ${name} ' }

// 3. 함수 바디가 return을 포함한 표현식 1개일 경우에 { } & return 삭제 가능
const arrow4 = name => 'hello, ${name}'
```





# 7. 문자열 (String)

## 7.1. 문자열 관련 주요 메서드 목록

- (참고) **추가적인 문자열 관련 메서드 정보**는 아래 링크에서 참고
  - [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript), [ECMA262](https://www.ecma-international.org/publications-and-standards/standards/ecma-262/)

| 메서드   | 설명                                          | 비고                                         |
| -------- | --------------------------------------------- | -------------------------------------------- |
| includes | 특정 문자열의 **존재여부를 참/거짓으로 반환** |                                              |
| split    | 문자열을 토큰 기준으로 **나눈 배열 반환**     | 인자가 없으면 기존 문자열을 배열에 담아 반환 |
| replace  | 해당 문자열을 **대상 문자열로 교체하여 반환** | replaceAll                                   |
| trim     | 문자열의 **좌우 공백 제거하여 반환**          | trimStart, trimEnd                           |

- **string.includes(value)**
  - 문자열에 value가 존재하는지 판별 후 참 또는 거짓 반환

```javascript
const str = 'a santa at nasa'

str.includes('santa')	// true
str.includes('asan')	// false
```

- **string.split(value)**
  - value가 없을 경우, 기존 문자열을 배열에 담아 반환
  - value가 빈 문자열일 경우 각 문자로 나눈 배열을 반환
  - value가 기타 문자열일 경우, 해당 문자열로 나눈 배열을 반환

```javascript
const str = 'a cup'

str.split()			// ['a cup']
str.split('')		// ['a', ' ', 'c', 'u', 'p']
str.split(' ')		// ['a', 'cup']
```

- **string.replace(from, to)**
  - 문자열에 from 값이 존재할 경우, 1개만 to 값으로 교체하여 반환

- **string.replaceAll(from, to)**
  - 문자열에 from 값이 존재할 경우, 모두 to 값으로 교체하여 반환

```javascript
const str = 'a b c d'

str.replace(' ', '-')		// 'a-b c d'
str.replaceAll(' ', '-')	// 'a-b-c-d'
```

- **string.trim()**
  - 문자열 시작과 끝의 모든 공백문자(스페이스, 탭, 엔터 등) 를 제거한 문자열 반환
- **string.trimStart()**
  - 문자열 시작의 공백문자를 제거한 문자열 반환
- **string.trimEnd()**
  - 문자열 끝의 공백문자를 제거한 문자열 반환

```javascript
const str = '    hello     '

str.trim()		// 'hello'
str.trimStart()	// 'hello     '
str.trimEnd()	// '     hello'
```





# 8. 배열 (Arrays)

## 8.1. 배열의 정의와 특징

- 키와 속성들을 담고 있는 참조 타입의 **객체(object)**
- 순서를 보장하는 특징이 있음
- 주로 대괄호를 이용하여 생성하고, 0을 포함한 양의 정수 인덱스로 특정 값에 접근 가능
- 배열의 길이는 array.length 형태로 접근 가능
  - (참고) 배열의 마지막 원소는 array.length - 1 로 접근 (파이썬처럼 [-1]로 접근할 수는 없음)

```javascript
const numbers = [1, 2, 3, 4, 5]

console.log(numbers[0])			// 1
console.log(numbers[-1])		// undefined
console.log(numbers.length)		// 5
```



## 8.2. 배열 관련 주요 메서드 - 1

- (참고) 추가적인 배열 관련 메서드 정보는 아래 링크에서 참고
  - [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript), [ECMA262](https://www.ecma-international.org/publications-and-standards/standards/ecma-262/#sec-properties-of-the-array-constructor)

| 메서드              | 설명                                                 | 비고                     |
| ------------------- | ---------------------------------------------------- | ------------------------ |
| **reverse**         | **원본 배열**의 요소들의 순서를 반대로 정렬          |                          |
| **push & pop**      | 배열의 **가장 뒤에** 요소를 **추가 또는 제거**       |                          |
| **unshift & shift** | 배열의 **가장 앞에** 요소를 **추가 또는 제거**       |                          |
| **includes**        | 배열에 특정 값이 존재하는지 판별 후 **참/거짓 반환** |                          |
| **indexOf**         | 배열에 특정 값이 존재하는지 판별 후 **인덱스 반환**  | 요소가 없을 경우 -1 반환 |
| **join**            | 배열의 **모든 요소를 구분자를 이용하여 연결**        | 구분자 생략 시 쉼표 기준 |

- **array.reverse()**
  - 원본 배열의 요소들의 순서를 반대로 정렬

```javascript
const numbers = [1, 2, 3, 4, 5]
numbers.reverse()
console.log(numbers)	// [5, 4, 3, 2, 1]
```

- **array.push()**
  - 배열의 가장 뒤에 요소 추가
- **array.pop()**
  - 배열의 마지막 요소 제거

```javascript
const numbers = [1, 2, 3, 4, 5]

numbers.push(100)
console.log(numbers)	// [1, 2, 3, 4, 5, 100]

numbers.pop()
console.log(numbers)	// [1, 2, 3, 4, 5]
```

- **array.unshift()**
  - 배열의 가장 앞에 요소 추가
- **array.shift()**
  - 배열의 첫번째 요소 제거

```javascript
const numbers = [1, 2, 3, 4, 5]

numbers.unshift(100)
console.log(numbers)	// [100, 1, 2, 3, 4, 5]

numbers.shift()
console.log(numbers)	// [1, 2, 3, 4, 5]
```

- **array.includes(value)**
  - 배열에 특정 값이 존재하는지 판별 후 참 또는 거짓 반환

```javascript
const numbers = [1, 2, 3, 4, 5]

console.log(numbers.include(1))		// true

console.log(numbers.include(100))	// false
```

- **array.indexOf(value)**
  - 배열에 특정 값이 존재하는지 확인 후 가장 첫 번째로 찾은 요소의 인덱스 반환
  - **만약 해당 값이 없을 경우 -1 반환** (false가 아님)

```javascript
const numbers = [1, 2, 3, 4, 5]
let result

result = numbers.indexOf(3)
console.log(result)			// 2

result = numbers.indexOf(100)
console.log(result)			// -1
```

- **array.join([separator])**
  - 배열의 모든 요소를 연결하여 반환
  - separator(구분자)는 선택적으로 지정 가능하며, 생략 시 쉼표를 기본 값으로 사용

```javascript
const numbers = [1, 2, 3, 4, 5]
let result

result = numbers.join()
console.log(result)				// 1, 2, 3, 4, 5

result = numbers.join('')
console.log(result)				// 12345

result = numbers.join(' ')
console.log(result)				// 1 2 3 4 5

result = numbers.join('-')
console.log(result)				// 1-2-3-4-5
```



### 8.2.1. Spread operator

- **spread operator(...)**를 사용하면 배열 내부에서 배열 전개 가능.
- ES5 까지는 Array.concat() 메서드를 사용.
- 얕은 복사에 활용 가능.
- **중요한 개념입니다 !**

```javascript
const array = [1, 2, 3]
const newArray = [0, ...array, 4]

console.log(newArray)	// [0, 1, 2, 3, 4]
```



## 8.3. 배열 관련 주요 메서드 - 2

- 배열을 순회하며 특정 로직을 수행하는 메서드
- 메서드 호출 시 인자로 **callback 함수***를 받는 것이 특징
  - **callback 함수***: 어떤 함수의 내부에서 실행될 목적으로 인자로 넘겨받는 함수를 말함

| 메서드      | 설명                                                         | 비고             |
| ----------- | ------------------------------------------------------------ | ---------------- |
| **forEach** | 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행               | **반환 값 없음** |
| **map**     | **콜백 함수의 반환 값**을 요소로 하는 **새로운 배열 반환**   |                  |
| **filter**  | **콜백 함수의 반환 값이 참인 요소들만** 모아서 **새로운 배열을 반환** |                  |
| **reduce**  | **콜백 함수의 반환 값들을 하나의 값(acc)에 누적 후 반환**    |                  |
| **find**    | 콜백 함수의 **반환 값이 참이면 해당 요소를 반환**            |                  |
| **some**    | 배열의 **요소 중 하나라도 판별 함수를 통과**하면 참을 반환   |                  |
| **every**   | 배열의 **모든 요소가 판별 함수를 통과**하면 참을 반환        |                  |

- (참고) Django로 보는 callback 함수 예시

```python
# urls.py
urlpatterns = [
    path('index/', views.index, name='index')
]
# views.py
def index(request):	# 이 index 가 urls.py의 'views.index'에서 콜백
    # ... 생략
    return render(request, 'articles/index.html', context)
```



### 8.3.1. forEach

- **array.forEach(callback(element[, index[,array]])))**
  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
  - 콜백 함수는 3가지 매개변수로 구성
    - element: 배열의 요소
    - index: 배열 요소의 인덱스
    - array: 배열 자체
  - **반환 값(return) 이 없는 메서드**

```javascript
array.forEach((element, index, array) => {
    // do something
})
```

```javascript
const fruits = ['딸기', '수박', '사과', '체리']

fruits.forEach((fruit, index) => {
    console.log(fruit, index)
    // 딸기 0
    // 수박 1
    // 사과 2
    // 체리 3
})
```

### 8.3.2. map

- **array.map(callback(element[, index[, array]]))**
  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
  - 콜백 **함수의 반환 값을 요소로 하는** 새로운 배열 반환
  - 기존 배열 전체를 다른 형태로 바꿀 때 유용

```javascript
array.map((element, index, array) => {
    // do something
})
```

```javascript
const numbers = [1, 2, 3, 4, 5]

const doubleNums = numbers.map((num) => {
    return num * 2
})

console.log(doubleNums)	// [2, 4, 6, 8, 10]
```

### 8.3.3. filter

- **array.filter(callback(element[, index[, array]]))**
  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
  - 콜백 **함수의 반환 값이 참인 요소들만** 모아서 새로운 배열을 반환
  - 기존 배열의 요소들을 필터링할 때 유용

```javascript
array.filter((element, index, array) => {
    // do something
})
```

```javascript
const numbers = [1, 2, 3, 4, 5]

const oddNums = numbers.filter((num, index) => {
    return num % 2
})

console.log(oddNums)	// 1, 3, 5
```

### 8.3.4. reduce

- **array.reduce(callback(acc, element, [index[, array]])[, initialValue])**

```javascript
array.reduce(acc, element, index, array) => {
    //do something
}, initialValue)
```

- 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
- 콜백 함수의 반환 값들을 **하나의 값(acc) 에 누적 후 반환**
- reduce 메서드의 주요 매개변수
  - acc
    - 이전 callback 함수의 반환 값이 누적되는 변수
  - initialValue(optional)
    - 최초 callback 함수 호출 시 acc에 할당되는 값, default 값은 배열의 첫번째 값
- (참고) 빈 배열의 경우 initialValue를 제공하지 않으면 에러 발생

#### 8.3.4.1. reduce 예시 & 동작 방식

```javascript
const numbers = [1, 2, 3]

const result = numbers.reduce((acc, num) => {
    return acc + num
}, 0)

console.log(result)	// 6
```

1. acc: 0, num: 1, return 0 + 1
2. acc: 1, num: 2, return 1 + 2
3. acc: 3, num: 3, return 3 + 3

### 8.3.5. find

- array.find(callback(element[, index[, array]]))
  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
  - 콜백 함수의 **반환 값이 참이면, 조건을 만족하는 첫번째 요소를 반환**
  - 찾는 값이 배열에 없으면 undefined 반환

```javascript
array.find((element, index, array)) {
    // do something
}
```

```javascript
const avengers = [
    { name: 'Tony Stark', age: 45 },
    { name: 'Steve Rogers', age: 32 },
    { name: 'Thor', age: 40},
]

const result = avengers.find((avenger) => {
    return avenger.name === 'Tony Stark'
})

console.log(result)	// { name: 'Tony Stark', age: 45 }
```

### 8.3.6. some

- array.some(callback(element[, index[, array]]))
  - 배열의 **요소 중 하나라도** 주어진 판별 함수를 통과하면 참을 반환
  - 모든 요소가 통과하지 못하면 거짓 반환
  - (참고) 빈 배열은 항상 거짓 반환

```javascript
array.some((element, index, array) => {
    // do something
})
```

```javascript
const numbers = [1, 3, 5, 7, 9]

const hasEvenNumber = numbers.some((num) => {
    return num % 2 === 0
})
console.log(hasEvenNumber)	// false

const hasOddNumber = numbers.some((num) => {
    return num % 2
})
console.log(hasOddNumber)	// true
```

### 8.3.7. every

- array.every(callback(element[, index[, array]]))
  - 배열의 **모든 요소가** 주어진 판별 함수를 통과하면 참을 반환
  - 하나의 요소라도 통과하지 못하면 거짓 반환
  - (참고) 빈 배열은 항상 참 반환

```javascript
array.every((element, index, array) => {
    // do something
})
```

```javascript
const numbers = [2, 4, 6, 8, 10]

const isEveryNumberEven = numbers.every((num) => {
    return num % 2 === 0
})
console.log(isEveryNumberEven)	// true

const isEveryNumberOdd = numbers.every((num) => {
    return num % 2
})
console.log(isEveryNumberOdd)	// false
```

- (참고) 배열 순회 방법 비교

```javascript
const chars = ['A', 'B', 'C', 'D']

// for loop
for (let idx = 0; idx < chars.length; idx++) {
    console.log(idx, chars[idx])
}

// for ... of
for (const char of chars) {
    console.log(char)
}

// forEach
chars.forEach((char, idx) => {
    console.log(idx, char)
})

chars.forEach(char => {
    console.log(char)
})
```

| 방식           | 특징                                                         | 비고      |
| -------------- | ------------------------------------------------------------ | --------- |
| **for loop**   | 1. 모든 브라우저 환경에서 지원 2. 인덱스를 활용하여 배열의 요소에 접근 3. break, continue 사용 가능 |           |
| **for ... of** | 1. 일부 오래된 브라우저 환경에서 지원 X 2. 인덱스 없이 배열의 요소에 바로 접근 가능 3. break, continue 사용 가능 |           |
| **forEach**    | 1. 대부분의 브라우저 환경에서 지원 2. break, continue 사용 불가능 | 권장 방식 |





# 9. 객체 (Objects)

## 9.1. 객체의 정의와 특징

- 객체는 속성(property) 의 집합이며, 중괄호 내부에 key와 value의 쌍으로 표현
- key는 문자열 타입*만 가능
  - (참고) key 이름에 띄어쓰기 등의 구분자가 있으면 따옴표로 묶어서 표현
- value는 모든 타입(함수 포함) 가능
- 객체 요소 접근은 점 또는 대괄호로 가능
  - (참고) key 이름에 띄어쓰기 같은 구분자가 있으면 대괄호 접근만 가능

```javascript
const me = {
    name: 'jack',
    phoneNumber: '01012345678',
    'samsung products': {
    	buds: 'Galaxy Buds pro',
    	galaxy: 'Galaxy s20',
	},
}

console.log(me.name)
console.log(me.phoneNumber)
console.log(me['samsung products'])
console.log(me['samsung products'].buds)
```



## 9.2. 객체와 메서드

- 메서드는 객체의 속성이 참조하는 함수
- **객체.메서드명() 으로 호출 가능**
- 메서드 내부에서는 this 키워드가 **객체**를 의미함.
  - fullName은 메서드가 아니기 때문에 정상출력되지 않음 (NaN)
  - getFullName은 메서드이기 때문에 해당 객체의 firstName과 lastName을 정상적으로 이어서 반환

```javascript
const me = {
    firstName: 'John',
    lastName: 'Doe',
    
    fullName: this.firstName + this.lastName,
    
    getFullName: function() {
        return this.firstName + this.lastName
    }
}
```



## 9.3. 객체 관련 ES6 문법 익히기

- ES6에 새로 도입된 문법들로 객체 생성 및 조작에 유용하게 사용 가능
  - 속성명 축약
  - 메서드명 축약
  - 계산된 속성명 사용하기
  - 구조 분해 할당*
    - (참고) 구조 분해 할당은 배열도 가능함
  - 객체 전개 구문 (Spread Operator)

### 9.3.1. 속성명 축약 (shorthand)

- 객체를 정의할 때 **key와 할당하는 변수의 이름이 같으면** 예시와 같이 축약 가능

![image-20220426204054492](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220426204054492.png)

### 9.3.2. 메서드명 축약 (shorthand)

- 메서드 선언 시 **function 키워드 생략 가능**

![image-20220426204220580](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220426204220580.png)

### 9.3.3. 계산된 속성 (computed property name)

- 객체를 정의할 때 key의 이름을 표현식을 이용하여 동적으로 생성 가능

![image-20220426204331237](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220426204331237.png)

### 9.3.4. 구조 분해 할당 (destructing assignment)

- 배열 또는 객체를 분해하여 속성을 변수에 쉽게 할당할 수 있는 문법

![image-20220426205715125](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220426205715125.png)

- 이름이 같아야 함

### 9.3.5. Spread operator (중요!)

- **spread operator(...)**를 사용하면 객체 내부에서 객체 전개 가능.
- ES5 까지는 Object.assign() 메서드를 사용.
- 얕은 복사에 활용 가능.

```javascript
const obj = {b: 2, c: 3, d: 4}
const newObj = {a: 1, ...obj, e: 5}

console.log(newObj)	// {a: 1, b: 2, c: 3, d: 4, e: 5}
```



## 9.4. JSON (JavaScript Object Notation)

- key-value 쌍의 형태로 데이터를 표기하는 언어 독립적 표준 포맷
- 자바스크립트의 객체와 유사하게 생겼으나 실제로는 문자열 타입
  - 따라서 JS의 객체로써 조작하기 위해서는 구문 분석(parsing, 파싱) 이 필수
- 자바스크립트에서는 JSON을 조작하기 위한 두 가지 내장 메서드를 제공
  - **JSON.parse()**
    - JSON => 자바스크립트 객체
  - **JSON.stringfy()**
    - 자바스크립트 객체 => JSON

![image-20220426210226693](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220426210226693.png)



## (참고) 배열은 객체다

- 키와 속성들을 담고 있는 참조 타입의 **객체(object)**
- 배열은 인덱스를 키로 갖으며 length 프로퍼티를 갖는 특수한 객체

![image-20220426210410349](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220426210410349.png)





# 10. this

## 10.1. this ?= window ?= object

- JS의 `this`는 실행 문맥 (execution context)에 따라 다른 대상을 가리킨다.
- class 내부의 생성자 (constructor) 함수
  - `this`는 생성되는 객체를 가리킴 (파이썬의 self)
- 메서드 (객체.메서드명() 으로 호출 가능한 함수)
  - `this`는 해당 메서드가 소속된 객체를 가리킴
- 위의 두 가지 경우를 제외하면 모두 최상위 객체 (window) 를 가리킴 (액션스크립트로 치면 root)



## 10.2. function 키워드와 화살표 함수 차이

```javascript
const obj = {
    PI: 3.14,
    radiuses: [1, 2, 3, 4, 5],
    printArea: function() {
        this.radiuses.forEach(function (r) {
            console.log(this.PI * r * r)
        }. bind(this))
    },
}
```

```javascript
const obj = {
    PI: 3.14,
    radiuses: [1, 2, 3, 4, 5],
    printArea: function() {
        this.radiuses.forEach((r) => {
            console.log(this.PI * r * r)
        })
    },
}
```

- **this.radiuses**는 메서드(객체.메서드명()으로 호출 가능) 소속이기 때문에 정상적으로 접근 가능
- forEach의 콜백함수의 경우 메서드가 아님 (객체.메서드명()으로 호출 불가능)
- 때문에 콜백함수 내부의 this는 window가 되어 `this.PI`는 정상적으로 접근 불가능
- 이 콜백함수 내부에서 `this.PI`에 접근하기 위해서 `함수객체.bind(this)` 메서드를 사용
- 이 번거로운 bind 과정을 없앤 것이 **화살표 함수**

### 10.2.1. 요약

- 함수 내부에 this 키워드가 존재할 경우
  - 화살표 함수와 function 키워드로 선언한 함수가 다르게 동작
- 함수 내부에 this 키워드가 존재하지 않을 경우
  - 완전히 동일하게 동작





# 11. lodash

> A modern JavaScript utility **library**

- 모듈성, 성능 및 추가 기능을 제공하는 JavaScript 유틸리티 라이브러리
- array, object 등 자료구조를 다룰 때 사용하는 유용하고 간편한 유틸리티 함수들을 제공
- 함수 예시
  - reverse, sortBy, range, random, cloneDeep ...



## 11.1. 사용 예시

```html
<body>
  <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
  <script>
    // 위의 CDN import를 통해 _ (언더스코어) 식별자 사용가능
    _.sample([1, 2, 3, 4])  // 3 (random 1 element)
    _.sampleSize([1, 2, 3, 4], 2) // [2, 3] (random 2 element)

    _.reverse([1, 2, 3, 4]) // [4, 3, 2, 1]
    
    _.range(5)  // [0, 1, 2, 4]
    _.range(1, 5) // [1, 2, 3, 4]
    _.range(1, 5, 2)  // [1, 3]

  </script>
</body>
```

```html
  <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
  
  <script>
    const original = { a: { b: 1 } }
    const ref = original
    const copy = _.cloneDeep(original)

    console.log(original.a.b, ref.a.b, copy.a.b)  // 1, 1, 1
    ref.a.b = 10
    console.log(original.a.b, ref.a.b, copy.a.b)  // 10, 10, 1
    copy.a.b = 100
    console.log(original.a.b, ref.a.b, copy.a.b)  // 10, 10, 100
  </script>
```

**lodash** 를 사용하지 않을 경우, 깊은 복사는 직접 함수를 만들어서 구현해야 함 (내장된 깊은복사 관련 함수 없음)

# 같이 보기

이전: [JS Basic-1 (기초, 변수, 연산자, 데이터 타입, 조건문, 반복문)](./JS%20Basic-1.md))

현재: JS Basic-2 (함수, 문자열, 배열, 객체, this, lodash)

다음: 없음