# 최댓값 구하기, 단어 뒤집기

## 구글링 vs 배운 것 활용하기

> len, max, min 쓴 사람은 배운 거로 다시 하십시오.

![image-20220118105112231](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220118105112231.png)

맨 마지막이 저였습니다. 구글링해서 나온 기능들을 사용하면서도 한 번도 써본 적 없는 함수들이다 보니 아리송했었습니다. 그런데 배웠던 기능들을 사용하라는 말씀을 듣고 나니 오히려 막혔던 문제까지 잘 풀리는 것이었습니다. 생각해보면 당연합니다. 배웠던 것들로 충분히 풀 수 있는 문제가 나왔을 테니까요. 먼 길을 돌아온 느낌이었습니다.

### 갯수 구하기

```python
# 주어진 리스트의 요소는 학생 이름으로 구성되어 있다. 학생들의 수를 출력하시오.
students = ['김철수', '이영희', '조민지']
# 아래에 코드를 작성하시오.

# 구글링해서 푼 방법
print(len(student))

# for if로 푼 방법
cnt = 0
for i in students:
    cnt = cnt + 1
print(cnt)
```

여기까지만 해도 `len()`을 사용하는 것이 더 쉬워 보입니다. 해본 적은 없지만 실제로 직무에 들어가면 윗 방법을 사용할 겁니다. 그런데 문제는 밑으로 갈 수록 발생합니다.

### 최댓값 구하기

```python
# 주어진 리스트의 요소 중에서 최댓값을 출력하시오.
numbers = [7, 10, 22, 4, 3, 17]
# 아래에 코드를 작성하시오.

# 구글링해서 푼 방법
numbers.sort()
M = numbers[len(numbers) - 1]
print(M)

# for if로 푼 방법
M = numbers[0]
#M = -99999999999
for i in numbers:
    if (M < i):
        M = i
print(M)
```

`sort()`메소드를 사용해 원소들을 오름차순으로 정렬한 뒤, 가장 오른쪽 값을 출력하였습니다. 이것도 나쁘지 않죠.

for / if를 사용한 방법은 좀 다릅니다. 초기값을 원소 첫번째로 설정한 뒤, 원소들과 하나하나 비교해서 더 큰 값이면 바꿨습니다.

원소 초기값도 **0으로 할까?** 고민한 뒤에 **-999999999**로 하자 라고 마음먹고 코딩한 뒤, 다른 분들의 코드를 보고 아, **항목 첫 값으로 설정하는 게 가장 맞겠구나**라는 걸 알게 되었습니다.

계속해서 보시죠.

### 'a'가 싫어

```python
# 입력으로 짧은 영단어 word가 주어질 때, 해당 단어에서 'a'를 모두 제거한 결과를 출력하시오.
word = input()
# 아래에 코드를 작성하시오.

# 구글링해서 푼 방법
noa = 'a'
for x in range(len(word)):
    noa = word.replace(word[x],"")
print(noa)

# for if로 푼 방법
noa = '' # 빈 문자열 생성
for i in word:
    if (i != 'a'):
        noa = noa + i # a가 없는 문자만 문자열에 추가
print(noa)
```

이쯤부터 구글링해서 푸는 방법이 이해가 되지 않기 시작합니다. 코드 길이도 비슷해졌습니다.

replace라는 메소드를 사용해보려 했는데, 잘 작동하지 않고 복사 붙혀넣기도 근본적으로 문제를 푸는 게 아니었기 때문이죠.

이 문제로 끙끙대던 중에 배운걸 활용하라는 교수님 말씀을 듣고 for if로 바꿔봤더니 아주 쉽게 풀렸습니다. 문제에 손 대기 전에 하는 생각이 얼마나 중요한지를 알 수 있었던 것 같습니다.

### 단어 뒤집기

```python
# 입력으로 짧은 영어단어 word가 주어질 때, 해당 단어를 역순으로 뒤집은 결과를 출력하시오.
word = input()
# 아래에 코드를 작성하시오.

drow = ''
for i in word:
    drow = i + drow
print(drow)
```

마지막 문제는 결국 for if만 사용해서 풀었습니다. 아주 빠르고 명쾌하게 풀렸기 때문이죠.