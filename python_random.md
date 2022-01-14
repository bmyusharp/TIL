# 파이썬 기초

## 랜덤까지 배웠습니다

오늘부터 본격적으로 파이썬을 시작했습니다.

조건문 if와 반복문 for, while을 배우고 랜덤 모듈이라는 것까지 배웠습니다.

우리가 사용해왔던 `print()` 등의 이미 있던 함수는 `내장함수`라고 하며, 이미 짜여진 코드를 기반으로 제공되는 함수입니다. `모듈`은 내장함수와 유사한데, `import`해와서 사용할 수 있습니다.

랜덤 기능을 간단히 설명해주시고, 1부터 45 중 로또 번호 6개를 뽑는 비복원추출 프로그램을 작성해보라 하셨습니다.

```python
import random #random이라는 모듈을 수입해옵니다.
numbers = []
for i in range(45):
    numbers.append(i+1) #0~44까지의 값에 각각 1을 더해 1~45까지를 리스트에 추가했습니다.
samples = []
samples.append(random.sample(numbers,6)) #random의 결과값을 samples 리스트에 추가
print(samples)
```

저는 비어있는 리스트를 먼저 선언하고, 그 리스트에 값들을 넣기 위해 '추가'한다는 생각을 했습니다.

그래서 리스트에 값들을 추가하는 방법을 몰랐기 때문에 구글에서 '리스트 원소 추가'를 검색했습니다.

![newbe_random-01](C:\Users\1004r\Desktop\TIL\python.assets\newbe_random-01.jpg)



그렇게 코드를 작성하고 보니 교수님께서 작성하신 코드는 조금 달랐습니다.

```python
import random
numbers = range(1,46) #numbers에 1~45까지의 수를 저장
lotto_number = random.sample(numbers, 6) #random의 결과값을 저장
print(lotto_number)
```

이때 저는 **파이썬은 변수에 넣는 초기값으로 자료형이 자동으로 설정된다**는 사실을 상기하게 되었습니다. 어쨌든 로또 번호가 나오고 있는 것을 보고 있자니 순서가 엉망입니다. 당연히 뽑은 순서로 정렬되어 나오는 것이기 때문에 이것도 이유가 있지만, 로또를 구매할 땐 오름차순으로 정렬되니까 그렇게 결과를 보고 싶은 것입니다.

그런 생각을 하고 있을 때 교수님이 코드를 추가로 입력하셨습니다.

```python
import random
numbers = range(1,46)
lotto_number = random.sample(numbers, 6)
print(sorted(lotto_number)) #sorted() 추가되었음
```

`sorted()`가 추가되면서 결과가 오름차순으로 출력되기 시작했습니다. sorted()도 내장함수인 것 같습니다.

그렇게 오늘의 수업은 마무리되었습니다.

그런데 갑자기 로또같은 비복원추출이 아닌 복원추출이 궁금해졌습니다.

복원추출의 경우 슬롯 머신이 있습니다. 돌아가는 무늬들이 모두 일치할 때 당첨되는 구조입니다.

비복원추출의 코드도 연습해보기 위해 random 모듈에 어떤 것들이 있나 찾아보았습니다.

단순히 세 번 choice하는 것으로는 마음에 들지 않아서 찾아보려고 했는데...

![newbe_random-02](C:\Users\1004r\Desktop\TIL\python.assets\newbe_random-02.jpg)



찾는 건 힘들어져서 일단 저만의 코드를 짜 보기로 했습니다.



## while 사용해서 인디언식 기우제식 슬롯머신 돌리기

방법은 '세 번의 추출, 출력하고 값이 같은지 확인!'으로 정했습니다.

```python
import random
slot = [1, 2, 3, 4, 5, 6, 7]

slot_machine = []
for i in range(3):
    slot_machine.append(random.choice(slot))

print(slot_machine)
```

![newbe_random-03](C:\Users\1004r\Desktop\TIL\python.assets\newbe_random-03.jpg)



여러 번 실행시켜보니 같은 값도 나오는 걸 확인했습니다.

슬롯머신 하면 떠오르는 777을 뽑을 때까지 몇 번 시도해야 했는지를 구하는 코드까지 만들어보겠습니다.

```python
import random
slot = [1, 2, 3, 4, 5, 6, 7]

count = 0
while(True):
    slot_machine = [] #반복할 때마다 리스트에 무한히 추가되면 안 되니까 반복할 때마다 리스트를 초기화해줍니다.
    for i in range(3):
        slot_machine.append(random.choice(slot))
    print(slot_machine)
    count = count + 1
    print(f'{count}회 째') #뽑을 때마다 몇 회만에 원하는 결과가 나올 지 볼 수 있게 카운트 했습니다.
    if (slot_machine[0] == slot_machine[1] == slot_machine[2]):
        break
```

while이 조건이 만족이라는 가정 하에 계속 반복되고, 일정 조건(`break`)이 발생하면 반복하지 않는다는 것을 이용해 세 개의 자릿수가 똑같으면 그만 뽑습니다.

결과는...

![newbe_random-04](C:\Users\1004r\Desktop\TIL\python.assets\newbe_random-04.jpg)



11회만에 뽑는 경우도 있고, 251회라는 수많은 시도 끝에 뽑힌 경우도 있습니다. 도박은 역시 해롭습니다.

이제 777을 뽑을 때까지 계속 시도하게 합니다.

```python
...
    if (slot_machine[0] == slot_machine[1] == slot_machine[2] == 7):
        break
```

![newbe_random-05](C:\Users\1004r\Desktop\TIL\python.assets\newbe_random-05.jpg)



첫 시도만에 금방 나오길래 이상함을 느꼈지만 몇 번의 시도를 더 해본 끝에

200회~400회 정도에서 뽑기도 하고 많으면 1700번까지도 시도해야 되기도 합니다.



모듈에서 새로운 함수를 찾아보려 했는데, 결국 찾지 못하고 노가다로 세 번 추출하게 했습니다.

아마 복원추출을 1억번 해야하는 경우에는 이 방법을 쓰기 힘들겠군요.
