# 트리

## 트리란?

- 비선형 구조
- 원소들 간에 1:n 관계를 가지는 자료구조
- 원소들 간에 계층관계를 가지는 계층형 자료구조
- 상위 원소에서 하위 원소로 내려가면서 확장되는 트리(나무)모양의 구조

### 정의

- 한 개 이상의 노드로 이루어진 유한 집합이며 다음 조건을 만족한다.
  - 노드 중 최상위 노드를 루트(root)라 한다.
  - 나머지 노드들은 n(>=0)개의 분리 집합 T1, T2, ..., TN으로 분리될 수 있다.
- 이들 T1, T2, ... , TN은 각각 하나의 트리가 되며(재귀적 정의) 루트의 부 트리(subtree)라 한다.

### 용어정리

- 노드(node): 트리의 원소
- 간선(edge): 노드를 연결하는 선, 부모 노드와 자식 노드를 연결
- 루트 노드(root node): 트리의 시작 노드
- 형제 노드(sibling node): 같은 부모 노드의 자식 노드들
- 조상 노드: 간선을 따라 루트 노드까지 이르는 경로에 있는 모든 노드들
- 서브 트리(subtree): 부모 노드와 연결된 간선을 끊었을 때 생성되는 트리
- 자손 노드: 서브 트리에 있는 하위 레벨의 노드들
- 차수(degree)
  - 노드의 차수: 노드에 연결된 자식 노드의 수
  - 트리의 차수: 트리에 있는 노드의 차수 중에서 가장 큰 값
  - 단말 노드: 차수가 0인 노드. 자식 노드가 없는 노드

![image-20220316092413554](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220316092413554.png)

- 높이
  - 노드의 높이: 루트에서 노드에 이르는 간선의 수. 노드의 레벨
  - 트리의 높이: 트리에 있는 노드의 높이 중에서 가장 큰 값. 최대 레벨





## 이진트리

- 모든 노드들이 2개의 서브트리를 갖는 특별한 형태의 트리
- 각 노드가 자식 노드를 최대한 2개 까지만 가질 수 있는 트리
  - 왼쪽 자식 노드(left child node)
  - 오른쪽 자식 노드(right child node)
- 이진 트리의 예

![image-20220316092549302](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220316092549302.png)

- 레벨 i에서 노드의 최대 개수는 2^i 개

- 높이가 h인 이진 트리가 가질 수 있는 노드의 최소 개수는 (h+1)개가 되며, 최대 개수는 (2^(h+1) - 1)개가 된다.

  

- **포화 이진 트리(Full Binary Tree)**

  - 모든 레벨에 노드가 포화상태로 차 있는 이진 트리
  - 높이가 h일 때, 최대의 노드 개수인 (2^(h+1) - 1)의 노드를 가진 이진 트리
    - 높이 3일 때 (2^(3+1) - 1) = 15개의 노드
  - 루트를 1번으로 하여 2^(h+1) - 1까지 정해진 위치에 대한 노드 번호를 가짐

![image-20220316092852807](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220316092852807.png)



- **완전 이진 트리(Complete Binary Tree)**
  - 높이가 h이고 노드 수가 n개일 때 (단, 2^h <= n < 2^(h+1) - 1), 포화 이진 트리의 노드 번호 1번부터 n번까지 빈 자리가 없는 이진 트리
  - 예) 노드가 10개인 완전 이진 트리는?

![image-20220316093032754](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220316093032754.png)

(=왼쪽부터 채운 거)



- **편향 이진 트리(Skewed Binary Tree)**
  - 높이 h에 대한 최소 개수의 노드를 가지면서 한쪽 방향의 자식 노드만을 가진 이진 트리
    - 왼쪽/오른쪽 편향 이진 트리

![image-20220316093216826](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220316093216826.png)



### 이진트리 - 순회(traversal)

- 순회란 트리의 각 노드를 **중복**되지 않게 **전부 방문(visit)**하는 것을 말하는데, 트리는 비 선형 구조이기 때문에 선형구조에서와 같이 선후 연결 관계를 알 수 없다. **따라서 특별한 방법이 필요하다.** 
- 3가지의 기본적인 순회방법
  - 전위순회(preorder traversal) : VLR
    - 부모노드 방문 후, 자식노드를 좌, 우 순서로 방문한다.
  - 중위순회(inorder traversal): LVR
    - 왼쪽 자식노드, 부모노드, 오른쪽 자식노드 순으로 방문한다.
  - 후위순회(postorder traversal): LRV
    - 자식노드를 좌우 순서로 방문한 후, 부모 노드로 방문한다.

![image-20220316094005759](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220316094005759.png)



- **전위 순회(preorder traversal)**
  - 수행 방법
    1. 현재 노드 n을 방문하여 처리한다. -> V
    2. 현재 노드 n의 왼쪽 서브트리로 이동한다. -> L
    3. 현재 노드 n의 오른쪽 서브트리로 이동한다. -> R
  - 전위 순회 알고리즘

```python
def preorder_traverse(T): #전위순회
    if T: 
        # T is not None 
        # 마지막 자식이면 부모의 오른쪽 서브트리(자신의 형제트리로 이동함)
        visit(T) # print(T.item)
        preorder_traverse(T.left)
        preorder_traverse(T.right)
```



- **중위 순회(inorder traversal)**
  - 수행 방법
    1. 현재 노드 n의 왼쪽 서브트리로 이동한다. -> L
    2. 현재 노드 n을 방문하여 처리한다. -> V
    3. 현재 노드 n의 오른쪽 서브트리로 이동한다. -> R
  - 중위 순회 알고리즘

```python
def inorder_traverse(T): #중위순회
    if T:		# T is not True
        inorder_traverse(T.left)
        visit(T) # print(T.item)
        inorder_traverse(T.right)
```



- **후위 순회(postorder traversal)**
  - 수행 방법
    1. 현재 노드 n의 왼쪽 서브트리로 이동한다. -> L
    2. 현재 노드 n의 오른쪽 서브트리로 이동한다. -> R
    3. 현재 노드 n을 방문하여 처리한다. -> V
  - 후위 순회 알고리즘

```python
def postorder_traverse(T): #후위순회
    if T:		# T is not True
        postorder_traverse(T.left)
        postorder_traverse(T.right)
        visit(T) # print(T.item)
```

응용에 따라 여러 가지 방법으로 순회 중에 여러 작업이 가능하다.



- 이진 트리의 순회
  - 전위 순회는? A-B-D-H-I-E-J-C-F-K-G-L-M
  - 중위 순회는? H-D-I-B-J-E-A-F-K-C-L-G-M
  - 후위 순회는? H-I-D-J-E-B-K-F-L-M-G-C-A

![image-20220316102756026](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220316102756026.png)



- **배열을 이용한 이진 트리의 표현**
  - 이진 트리에 각 노드 번호를 다음과 같이 부여
  - 루트의 번호를 1로 함
  - 레벨 n에 있는 노드에 대하여 왼쪽부터 오른쪽으로 2^n부터 2^(n+1) - 1까지 번호를 차례로 부여
- 노드 번호의 성질
  - 노드 번호가 i인 노드의 부모 노드 번호? ㄴi/2ㅡㅣ
  - 노드 번호가 i인 노드의 왼쪽 자식 노드 번호? 2*i
  - 노드 번호가 i인 노드의 왼쪽 자식 노드 번호? 2*i + 1
  - 레벨 n의 노드 번호 시작 번호는? 2^n

![image-20220316103951297](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220316103951297.png)

- 노드 번호를 배열의 인덱스로 사용
- 높이가 h인 이진 트리를 위한 배열의 크기는?
  - 레벨 i의 최대 노드 수는? 2^i
  - 따라서 1 + 2 + 4 + 8 + ... + 2^i = 2^(h+1) - 1

![image-20220316104203228](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220316104203228.png)



- 편향 이진트리의 경우

![image-20220316104235566](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220316104235566.png)



### 이진 트리의 저장

> 예를 들어...
>
> 4 <- 간선의 개수 N
>
> 1 2 1 3 3 4 3 5 (1-2, 1-3, 3-4, 3-5) <- 부모 자식 순

참고: **어차피 정점(노드)의 개수와 간선의 개수는 항상 1만큼만 차이 난다.**

- 부모 번호를 인덱스로 자식 번호를 저장

![image-20220316104751345](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220316104751345.png)

-> 두 개의 1차원 배열 또는 2열의 2차원 배열로

참고: **루트가 무조건 1번이라는 건 틀리고, 부모보다 자식의 노드 번호가 항상 크다는 것도 틀리다.**



```python
def pre_order(V):
    if v: 	# 0번 정점이 없으므로... 0번은 자식이 없는 경우를 표시
		print(v)	# visit(v)
        pre_order(ch1[v])
        pre_order(ch2[v])
        
def in_order(v):
    if v:
        pre_order(ch1[v]).
        print(v)
        pre_order(ch2[v])

def postorder(v):
    if v:
        post_order(ch1[v])
        post_order(ch2[v])
        print(v)
        
E = int(input())	# edge 수
arr = list(map(int, input().split()))
V = E + 1			# 정점 수

# 부모번호를 인덱스로 자식번호 저장
ch1 = [0] * (V + 1)
ch2 = [0] * (V + 1)
for i in range(E):
    p, c = arr[i * 2], arr[i * 2 + 1]
    if ch1[p] == 0:
        ch1[p] = c
    else:
        ch2[p] = c

#pre_order(3)	# 3번부터 시작해서 3-4-5 탐색 후 종료
#in_order(1)	# 1번부터 시작해서 2-1-4-3-5 탐색 후 종료
#post_order(3)	# 3번부터 시작해서 4-5-3 탐색 후 종료
#post_order(1)	# 1번부터 시작해서 2-4-5-3-1 탐색 후 종료
```

