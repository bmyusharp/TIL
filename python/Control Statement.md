# 제어문

제어문은 기본적으로 위에서 아래로 실행되는 파이썬의 코드 방향을 바꿀 수 있는 문장입니다.

특정 상황이나 조건에 따라 선택적으로 실행(조건문)하거나 반복(반복문)하게 해 줄 수 있습니다.



## if 조건문

> 조건문은 참/거짓을 판별하여 코드의 실행 여부를 결정하는 문장입니다.

```python
if (조건):
    참일 경우 실행하는 문장
else:
    조건이 참이 아닐 경우 실행하는 문장
```

다음과 같이 `if` 이후에는 조건을 넣을 수 있습니다. 이것이 참일 경우 다음 줄에서 한 번 들여쓰고 실행할 문장을 적을 수 있습니다.

이후 조건이 참이 아닐 경우 실행하는 문장을 `else:` 다음 줄에 들여쓰고 나서 넣을 수 있습니다.

여러 가지의 조건을 따져야 할 경우, if문 속 if문을 활용하는 것도 가능하며, 조건이 거짓일 때 다른 조건을 확인하고 싶을 경우 `elif`를 사용하기도 합니다.

```python
if (조건1):
    조건1이 참일 경우 실행하는 문장
    if (조건2):
        조건1, 조건2가 모두 참일 경우 실행하는 문장
    else:
        조건1이 참이고 조건2가 거짓일 경우 실행하는 문장
elif (조건A):
    조건1이 거짓이고 조건A가 참일 경우 실행하는 문장
else:
    조건1, 조건A가 전부 거짓일 경우 실행하는 문장
```

### 조건 표현식

`if`를 활용하여 값을 정할 때 사용되는 문법입니다.

```python
#일정 점수 이상 여부에 따라 참/거짓을 대입하고 싶을 때
testpass = True if (score >= 60) else False
#과락이 한 과목이라도 없는지 확인하려 할 때
failure = False if (score_a >= 40 and score_b >= 40) else True
```



## 반복문 - for와 while

### for

> for i in range(10):

반복문은 특정 조건에 도달할 때까지 작업을 반복하기 위한 문법이며, 반복문의 종류로는 `for`와 `while`이 있습니다.

for는 특정 객체를 순회하며 내용을 반복 작업하며, 모두 순회하면 작업이 종료됩니다. 특정 개체에 들어갈 수 있는 것은 시퀀스(string, tuple, list, range) 포함입니다.

```python
for i in 'SSAFY':
    print(i, end = ' ') #실행결과: S S A F Y
for i in [2, 4, 6, 8]:
    print(i * i, end = ' ') #실행결과: 4 16 36 64
for i in range(10):
    print(i, end = '') #실행결과: 0123456789
```

for가 딕셔너리(Dictionary)를 순회할 때는 key값을 순회하며, 이때 출력할 경우 순회중인 key값을 출력하며, value를 출력하진 않습니다.

```python
dictionary = {'a': 64, 'b': 65, 'c': 66}
for i in dictionary:
    print(i, end = '')
#실행결과: abc
```

### while

> while (i >10):

while은 종료조건에 해당하는 코드가 존재하여야 하며, 종료조건을 만족하기 전까지 계속해서 작동하는 반복문입니다. 만약 종료조건이 없을 경우 무한 루프하게 되므로, 반드시 종료 조건이 필요합니다.

```python
i = 0
while (i < 10):
    print(i, end = '')
    i = i + 1 
#실행 결과: 0123456789
```

`break`는 while의 종료조건으로 사용됩니다.

```python
i = 0
while (True):
    print(i, end = '')
    i = i + 1
    if (i >= 10):
        break
#실행 결과: 0123456789
```

`while`과 `if`의 뒤에는 참/거짓을 판별하는 값이 나오는데, 이 때 불린 자료형 이외의 다른 것이 나올 경우 불린값으로 변합니다. 예를 들어, 문자열이나 리스트 등이 들어갈 경우 True로 변환됩니다. False나 0이 들어갈 경우 거짓으로 처리됩니다.

```python
#위의 while (True) 부분을 while ([1, 2, 3]) 이나 while ('unable')로 바꿔도 똑같이 작동합니다.
```



> 36의 약수를 출력하는 코드를 for와 while로 각각 표현하시오.

```python
for i in range(1, 36 + 1):
    if (36 % i == 0):
        print(i)
```

```python
i = 1
while True:
    if (36 % i == 0):
        print(i)
    i = i + 1
    if (i > 36):
        break
```



## 부록

### .append() 메서드

`.append()`메서드는 리스트의 제일 오른쪽에 값을 추가해주는 명령어입니다.

```python
#1부터 4까지 있는 리스트에 5부터 8까지 있는 리스트를 추가해주세요
o2f = [1, 2, 3, 4]
f2e = [5, 6, 7, 8]
for i in f2e:
    o2f.append(i)
print(o2f)
#실행결과: [1, 2, 3, 4, 5, 6, 7, 8]
```

### enumerate() 함수

`enumerate()`함수는 리스트의 내용물을 인덱스와 함께 튜플 형태로 묶어서 리스트로 반환하는 함수입니다.

```python
#리스트의 순서대로 출석 번호를 매겨주세요.
men = ['Yujin', 'Yuria', 'Ellena', 'Harin']
num_list = list(enumerate(men))
print(num_list) 
#실행결과: [(0, 'Yujin'), (1, 'Yuria'), (2, 'Ellena'), (3, 'Harin')]
```

### 함수? 메서드?

위에서 `.append()`는 메서드라 했고, `enumerate()`는 함수라고 했습니다.

.append()와 같은 메서드는 클래스 및 객체에 연관되어 있습니다. 그러므로, 연관된 클래스 및 객체 뒤에 .을 붙히고 사용합니다.

반면 함수는 그런 제약이 없이 독립적으로 사용될 수 있습니다.

### 주피터 노트북

주피터 노트북은 웹 브라우저 환경에서 파이썬 코드를 작성 및 실행할 수 있는 툴입니다.

바로바로 결과를 알 수 있기 때문에 repl.it과도 유사하고 할 수 있습니다.

1. 설치 방법

   cmd를 열거나, Bash 내에서 `pip install jupyter`를 입력합니다.

2. 실행 방법

   방금 주피터를 설치한 콘솔에서 `jupyter notebook`을 입력합니다.

자주 쓰이는 단축키

**a**: 현재 셀 위에 새로운 셀을 추가한다. (above)

**b**: 현재 셀 아래에 새로운 셀을 추가한다. (below)

**dd**: 현재 셀을 삭제한다. (delete) ~~정말 삭제하시겠습니까? ㅇㅇ~~

**ctrl + enter**: 현재 셀을 실행한다. 

마크다운(**m**) <-> 파이썬(**y**) 로 문서와 코드 변경
