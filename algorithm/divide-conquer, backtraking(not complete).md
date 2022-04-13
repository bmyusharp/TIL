# 분할 정복 기법

- 설계 전략
  - 분할(Divide): 해결할 문제를 여러 개의 작은 부분으로 나눈다.
  - 정복(Conquer): 나눈 작은 문제를 각각 해결한다.
  - 통합(Combine): (필요하다면) 해결된 해답을 모은다.

탑/다운 (가짜동전찾기)

반복 알고리즘 (거듭제곱)



## 병합 정렬 (Merge Sort)

여러 개의 정렬된 자료의 집합을 병합하여 한 개의 정렬된 집합으로 만드는 방식

탑-다운(top-down) 방식

시간 복잡도: O(n log n)



- 분할 단계: 전체 자료 집합에 대하여, 최소 크기의 부분집합(1개)이 될 때까지 분할 작업을 계속한다.

![image-20220330093227828](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220330093227828.png)

- 병합 단계: 더 이상 갈라지지 않으면 리턴, 2개의 부분집합을 정렬하면서 하나의 집합으로 병합(크기를 비교하면서)

  N개의 부분집합이 1개로 병합될 때까지 반복함

![image-20220330093653343](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220330093653343.png)



- 분할 단계

```
merge_sort(LIST m)
	IF length(m) == 1 : return m
	
	LIST left, right
	middle = length(m) / 2
	FOR x in m before middle
		add x to left
	FOR x in m after or equal middle
		add x to right
	
	left = merge_sort(left)
	right = merge_sort(right)
	
	RETURN merge(left, right)
```

- 병합 단계

```
merge(LIST left, LIST right)
	LIST result
	
	WHILE length(left) > 0 OR length(right) > 0
		IF length(left) > 0 AND length(right) > 0
			IF first(left) <= first(right)
				append popfirst(left) to result
			ELSE
				append popfirst(right) to result
		ELIF length(left) > 0
			append popfirst(left) to result
		ELIF length(right) > 0
			append popfirst(right) to result
	RETURN resul
```



**병합 정렬의 단점: 메모리를 많이 사용함**

실제로 값이 이동하는 것은 아니고, 인덱스를 이동시킴



## 퀵 정렬

- 주어진 배열을 두 개로 분할하고, 각각을 정렬한다.
  - 병합 정렬과 동일할까?
- 다른 점 1: 병합 정렬이 그냥 두 부분으로 나누는 반면, 퀵 정렬은 분할할 때, 기준 아이템(pivot item)을 중심으로, 이보다 작은 것은 왼편, 큰 것은 오른편에 위치시킨다.
- 다른 점 2: 각 부분 정렬이 끝난 후, 병합정렬은 "병합"이란 후처리 작업이 필요하나, 퀵 정렬은 필요로 하지 않는다.

```
quickSort(A[], l, r)
	if l < r #정렬할 구간이 실제로 존재할 때(정렬된 상태가 아닐 때)
		s = partition(a, l, r)
		quickSort(A[], l, s-1) #왼쪽 구간과 오른쪽구간에 대해 반복
		quickSort(A[], s+1, r)
```

"그냥 외우세요"



- Hoare-Partition 알고리즘

![image-20220330094820351](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220330094820351.png)

A[l] 맨 왼쪽 값을 기준값으로 함.. i와 j를 초기화(왼쪽 끝과 오른쪽 끝).. i와 j가 교차하기 전까지 반복 - 기준값(i)보다 큰 것을 찾음(해당자리 고정), 기준값( j)보다 작은 것을 찾음



1. 피봇 선택 (이 선택에 따라 성능이 좌우됨)

​		왼쪽 끝/오른쪽 끝/임의의 세개 값 중에 중간 값 등

2. i와 j가 양끝에서 다가오면서 P(pivot)과 비교함. -> 교환

![image-20220330100214619](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220330100214619.png)

![image-20220330100227966](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220330100227966.png)



- Lomuto partition 알고리즘

```
partition(A[], p, r)
	x = A[r] #맨 끝값
	i = p-1 
	
	FOR j in p <- r-1
		IF A[j] <= x
			i++, swap(A[i], A[j]) #swap부분은 자리를 바꾼다는 표현
				 #(실제로 재귀 구현은 느려짐..)
	
	swap(A[i+1], A[r])
	RETURN i+1
```

![image-20220330100831611](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220330100831611.png)

j와 i가 한 방향으로(둘다 오른쪽으로) 이동함. 그러나, j는 r까지

j가 가리키는 값이 피봇보다 같거나 작으면, i가 따라감.

피봇보다 크면, i는 멈춤. j는 계속 이동.

j가 피봇보다 작은 값을 발견하면, i+1과 바꿈. j가 끝까지 가면, 피봇을 i+1과 바꿈



## 이진 검색

- 자료의 가운데에 있는 항목의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 계속 진행하는 방법
  - 목적 키를 찾을 때까지 이진 검색을 순환적으로 반복 수행함으로써 검색 범위를 반으로 줄여가면서 보다 빠르게 검색을 수행함
- 이진 검색을 하기 위해서는 자료가 **정렬된 상태**여야 한다.

- 검색 과정
  - 자료의 중앙에 있는 원소를 고른다.
  - 중앙 원소의 값과 찾고자 하는 목표 값을 비교한다.
  - 목표 값이 원소의 값보다 작으면 자료의 왼쪽 반에 대해서 새로 검색을 수행하고, 크다면 자료의 오른쪽 반에 대해서 새로 검색을 수행한다.
  - 찾고자 하는 값을 찾을 때까지 위 과정을 반복한다.



- 반복 구조

![image-20220330103304653](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220330103304653.png)

첫줄 k <- **key**임

while low **`<=`** high

'<'는 안됩니다. 



- 재귀 구조

![image-20220330104307136](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220330104307136.png)



## 분할 정복의 활용

- 병합 정렬은 외부 정렬의 기본이 되는 정렬 알고리즘이다. 또한, 멀티코어 CPU나 다수의 프로세서에서 정렬 알고리즘을 병렬화하기 위해 병합 정렬 알고리즘이 활용된다.
- 퀵 정렬은 매우 큰 입력 데이터에 대해서 좋은 성능을 보이는 알고리즘이다.





# 백트래킹

- N-Queen 문제
- nxn 서양 장기판에서 배치한 Queen들이 서로 위협하지 않도록 n개의 Queen을 배치하는 문제
  - 어떤 두 Queen도 서로를 위협하지 않아야 한다.
  - Queen을 배치한 n개의 위치는?
- 여러 가지 선택지(옵션)들이 존재하는 상황에서 한가지를 선택한다.
- 선택이 이루어지면 새로운 선택지들의 집합이 생성된다.
- 이런 선택을 반복하면서 최종 상태에 도달한다.
  - 올바른 선택을 계속하면 목표 상태(goal stage)에 도달한다.
- 당첨 리프 노드 찾기
  - 루트에서 갈 수 있는 노드를 선택한다.
  - 꽝 노드까지 도달하면 최근의 선택으로 되돌아와서 다시 시작한다.
  - 더 이상의 선택지가 없다면 이전의 선택지로 돌아가서 다른 선택을 한다.
  - 루트까지 돌아갔을 경우 더 이상 선택지가 없다면 찾는 답이 없다.



### 백트래킹과 깊이 우선 탐색과의 차이

- 어떤 노드에서 출발하는 경로가 해결책으로 이어질 것 같지 않으면 더 이상 그 경로를 따라가지 않음으로써 시도의 횟수를 줄임. (**Prunning** 가지치기)
- 깊이 우선 탐색이 모든 경로를 추적하는데 비해 백트래킹은 불필요한 경로를 조기에 차단
- 깊이 우선 탐색을 가하기에는 경우의 수가 너무 많음. 즉, N! 가지의 경우의 수를 가진 문제에 대해 깊이 우선 탐색을 가하면 당연히 처리 불가능한 문제.
- 백트래킹 알고리즘을 적용하면 일반적으로 경우의 수가 줄어들지만 이 역시 최악의 경우에는 여전히 지수함수 시간(O(2^n) 등)을 요하므로 처리 불가능

### 백트래킹 개념

- 8-Queens 문제
  - 퀸 8개를 크기의 체스판 안에 서로를 공격할 수 없도록 배치하는 모든 경우를 구하는 문제
- 후보 해의 수: 64C8 = 64! / 8! * (64-8)! = 4,426,165,368
- 실제 해의 수는 이 중에서 92개 뿐
- 즉, 44억 개가 넘는 후보 해의 수 속에서 92개를 최대한 효율적으로 찾아내는 것이 관건

- 4-Queens 문제로 축소해서 생각해 보자
  - 같은 행에 위치할 수 없다.
  - 모든 경우의 수: 4 * 4 * 4 * 4 = 256

![image-20220330110905619](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220330110905619.png)

- 루트 노드에서 리프 노드까지의 경로는 해답후보(candidate solution)가 되는데, 깊이 우선 검색을 하여 그 해답후보 중에서 해답을 찾을 수 있다.
- 그러나 이 방법을 사용하면 해답이 될 가능성이 전혀 없는 노드의 후손 노드(descendant)들도 모두 검색해야 하므로 비효율적이다.

- 모든 후보를 검사? -> No!
- 백트래킹 기법
  - 어떤 노드의 유망성을 점검한 후에 유망(promising)하지 않다고 결정되면 그 노드의 부모로 되돌아가(backtracking) 다음 자식 노드로 감.
  - 어떤 노드를 방문하였을 때 그 노드를 포함한 경로가 해답이 될 수 없으면 그 노드는 유망하지 않다고 하며, 반대로 해답의 가능성이 있으면 유망하다고 한다.
  - 가지치기(pruning): 유망하지 않은 노드가 포함되는 경로는 더 이상 고려하지 않는다.

백트래킹을 이용한 알고리즘은 다음과 같은 절차로 진행된다.

1. 상태 공간 트리의 깊이 우선 검색을 실시한다.
2. 각 노드가 유망한지를 점검한다.
3. 만일 그 노드가 유망하지 않으면, 그 노드의 부모 노드로 돌아가서 검색을 계속한다.

일반 백트래킹 알고리즘

```
checknode(node v)
	IF promising(v)
		IF there is a solution at v
			write the solution
		ELSE
			FOR each child u of v
				checknode u
```

promising을 잘 짜는 것도 성능에 중요한 영향을 미침

순수한 깊이 우선 검색: **155노드** vs 백트래킹: **27노드**
