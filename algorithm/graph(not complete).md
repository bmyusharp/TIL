# 그래프 & 백트래킹

- N * N개의 2차 배열에서 원소 3개를 고르는 (간단한) 방법
  - 1차원 배열로 만들어서 (0, 0), (0, 1), ... (1, 1), (1, 2), .... (N-1, N-1)까지 저장
  - 그 다음에 3개를 고른다.

- 실 세계 문제를 그래프로 추상화해서 해결하는 방법을 학습한다.
  - 그래프 탐색 방법인 BFS, DFS에 대해 학습한다.
  - 그래프 알고리즘에 활용되는 상호배타 집합(Disjoint-Sets)의 자료구조에 대해 학습한다.
  - 최소 신장 트리(Minimum Spanning Tree)를 이해하고 탐욕 기법을 이용해서 그래프에서 최소 신장 트리를 찾는 알고리즘을 학습한다.
  - 그래프의 두 정점 사이의 최단 경로(Shortest Path)를 찾는 방법을 학습한다.
- 그래프까지 끝났다면 알고리즘 좀 했다... 고 할 수 있음

## 그래프

- 그래프는 아이템(사물 또는 추상적 개념)들과 이들 사이의 연결 관계를 표현한다.
- 그래프는 정점(Vertex)들의 집합과 이들을 연결하는 간선(Edge)들의 집합으로 구성된 자료구조 *Tree*
  - |V|: 정점의 개수, |E|: 그래프에 포함된 간선의 개수
    - |V| : 집합의 크기(원소 수, 정점 수)
  - |V|개의 정점을 가지는 그래프는 최대 |V|(|V|-1) / 2 간선이 가능
  - 예) 5개 정점이 있는 그래프의 최대 간선 수는 10 ( = 5*4/2) 개이다.
- 선형 자료구조나 트리 자료구조로 표현하기 어려운 N : N 관계를 가지는 원소들을 표현하기에 용이하다.

### 그래프 유형

- 무향 그래프(Undirected Graph)
- 유향 그래프(Directed Graph)
- 가중치 그래프(Weighted Graph)
- 사이클 없는 방향 그래프(DAG, Directed Acyclic Graph)

![image-20220401091438756](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220401091438756.png)

- 완전 그래프
  - 정점들에 대해 가능한 모든 간선들을 가진 그래프

![image-20220401091503626](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220401091503626.png)

- 부분 그래프
  - 원래 그래프에서 일부의 정점이나 간선을 제외한 그래프
- 인접(Adacency)
  - 두 개의 정점에 간선이 존재(연결됨)하면 **서로 인접해 있다**고 한다.
  - 완전 그래프에 속한 임의의 두 정점들은 모두 인접해 있다.

### 그래프 경로

- 경로란 간선들을 순서대로 나열한 것
  - 간선들: (0, 2), (2, 4), (4, 6)
  - 정점들: 0 - 2 - 4 - 6
- 경로 중 한 정점을 최대한 한 번만 지나는 경로를 **단순경로**라 한다.
  - 0 - 2 - 4 - 6, 0 - 1 - 6
- 시작한 정점에서 끝나는 경로를 사이클(Cycle)이라고 한다.
  - 1 - 3 - 5 - 1

### 그래프 표현

- 간선의 정보를 저장하는 방식, 메모리나 성능을 고려해서 결정
- 인접 행렬 (Adjacent matrix)
  - |V| * |V| 크기의 2차원 배열을 이용해서 간선 정보를 저장
  - 배열의 배열(포인터 배열)
- 인접 리스트 (Adjacent List)
  - 각 정점마다 해당 정점으로 나가는 간선의 정보를 저장
- 간선의 배열
  - 간선(시작 정점, 끝 정점)을 배열에 연속적으로 저장

### 인접 행렬

- 두 정점을 연결하는 간선의 유무를 행렬로 표현
  - |V| * |V| 정방 행렬
  - 행 번호와 열 번호는 그래프의 정점에 대응
  - 두 정점이 인접되어 있으면 1, 그렇지 않으면 0으로 표현
  - 무향 그래프
    - i번째 행의 합 = i번째 열의 합 = Vi의 차수
  - 유향 그래프
    - 행 i의 합 = Vi의 진출 차수 (나가는 개 몇 개지?)
    - 열 i의 합 = Vi의 진입 차수 (들어오는 게 몇 개지?) *위상정렬에서 사용*
    - 보통은, 행이 출발하는 번호고, 열이 도착하는 번호다.

![image-20220401092110372](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220401092110372.png)

```python
"""
마지막 정점번호, 간선 수
6 8
0 1 0 2 0 5 0 6 5 3 4 3 5 4 6 4
"""
V, E = map(int, input().split()) # V 마지막정점 번호, E 간선 수
arr = list(map(int, input().split()))
adjM = [[0]*(V+1) for _ in range(V+1)] # 인접행렬
#adjL = [[] for _ in range(V+1)] #인접 리스트

for i in range(E): 
    n1, n2 = arr[i*2], arr[i*2+1]
    adjM[n1][n2] = 1 # 유향 그래프인 경우, 이 줄만 하야 됨
    adjM[n2][n1] = 1 # 무향 그래프인 경우, 이 줄까지 해야 됨
# 방향이 있는지, 없는지는 이 저장 순간에만 확인할 수 있다.
```

- 인접 행렬의 단점은?

![image-20220401093838002](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220401093838002.png)

- 오른쪽의 경우, 할당되는 메모리는 적으나 인접 관계를 보기 어려움...

### 인접 리스트

- 각 정점에 대한 인접 정점들을 순차적으로 표현
- 하나의 정점에 대한 인접 정점들을 각각 노드로 하는 연결 리스트로 저장

![image-20220401093959912](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220401093959912.png)

![image-20220401094048016](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220401094048016.png)

```python
"""
마지막 정점번호, 간선 수
6 8
0 1 0 2 0 5 0 6 5 3 4 3 5 4 6 4
"""
V, E = map(int, input().split()) # V 마지막정점 번호, E 간선 수
arr = list(map(int, input().split()))
#adjM = [[0]*(V+1) for _ in range(V+1)] # 인접행렬
adjL = [[] for _ in range(V+1)] # 인접 리스트

for i in range(E): 
    n1, n2 = arr[i*2], arr[i*2+1]
    adjL[n1].append(n2) # 유향 그래프인 경우, 이 줄만
    adjL[n2].append(n1) # 무향 그래프인 경우, 이 줄까지
```



### 문제 : 친구 관계

다음과 같이 친구 관계를 그래프로 표현하였다.

- A로부터 시작해서 한 명의 친구에게만 소식을 전달할 수 있다면 최대 몇 명의 친구가 소식을 전달받을 수 있을까?
- A로부터 시작해서 친구들에게 동시에 소식을 전달할 수 있다고 할 때, 가장 늦게 전달 받는 사람은 누구일까? (단 소식을 전달하는 속도는 모두 동일하다)

![image-20220401095109122](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220401095109122.png)



### 그래프 순회(탐색)

- 그래프 순회는 비선형구조인 그래프로 표현된 모든 자료(정점)를 빠짐없이 탐색하는 것을 의미한다.
- 두 가지 방법
  - 깊이 우선 탐색(DFS, Depth First Search)
  - 너비 우선 탐색(BFS, Breadth First Search)

### DFS (깊이우선탐색)

- 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 더 이상 갈 곳이 없게 되면, 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서 다른 방향의 정점으로 탐색을 계속 반복하여 결국 모든 정점을 방문하는 순회방법
- 가장 마지막에 만났던 갈림길에 정점으로 되돌아가서 **다시 깊이 우선 탐색을 반복해야** 하므로 후입선출 구조의 스택을 사용(반복)하거나, 재귀를 사용(리턴)해야 함.

![image-20220401100544703](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220401100544703.png)

#### DFS 알고리즘 - 재귀

```
DFS_Recursive(G, V)

	visited[ v ] <- true		// v 방문 설정
	
	FOR	each all w in adjacency( G, V )
		IF	visited[w]	!=	TRUE
			DFS_Recursive(G, w)
```

- 1에서 4로 가는 경로의 개수는?
  - 1-2-5-4, 1-3-2-5-4, 1-3-4의 경로들을 구해야 할 경우, visited를 허용하느냐, 안 하느냐를 조절해야 함
  - 그렇다고 visited를 빼버리면, 고립되어 무한 반복되는 경우도 생길 수 있으니... visited를 false로 한다던가, 다른 경로를 통해 가는 것을 허용하던가

#### DFS 알고리즘 - 반복

```
STACK s
visited[ ]
DFS(v)
	push( s, v )
	WHILE NOT isEmpty( s )
		V <- pop(s)
		IF NOT visited[v]
			visited( v )
			FOR each w in adjacency( v )
				IF NOT visited[w]
				push(s, w)
```

```python
def dfs1(i):
    visited[i] = 1
    print(i, end = ' ') # 방문한 순서를 출력해 보자
    for w in range(V+1): # i에 인접한 모든 노드에 대해
    if adjM[i][w] == 1 and visited[w] == 0: # 아직 방문하지 않은 곳이면
        dfs1(w)

visited = [0](V+1)
dfs1(0) # 0번부터 탐색
# 0 1 2 5 3 4 6
```

### BFS(너비 우선 탐색)

- 너비우선탐색은 탐색 시작점의 인접한 정점들을 먼저 모두 차례로 방문한 후에, 방문했던 정점을 시작점으로 하여 다시 인접한 정점들을 차례로 방문하는 방식
- 인접한 정점들에 대해 탐색을 한 후, 차례로 다시 너비우선탐색을 진행해야 하므로, 선입선출 형태의 자료구조인 큐를 활용함
- 큐의 기본 연산
  - 삽입: enQueue
  - 삭제: deQueue

#### BFS 알고리즘

- 입력 파라미터: 그래프 G와 탐색 시작점 v

```
BFS(G, v) // 그래프 G, 탐색 시작점 v
	큐 생성
	시작점 v를 큐에 삽입
	점 v를 방문한 것으로 표시
	WHILE 큐가 비어있지 않은 경우
		t <- 큐의 첫번째 원소 반환
		FOR t와 연결된 모든 선에 대해
			u <- t의 인접 정점
			u가 방문되지 않은 곳이면,
			u를 큐에 넣고, 방문한 것으로 표시
```



## 서로소 집합(Disjoint-sets)

- 서로소 또는 상호배타 집합들은 서로 중복 포함된 원소가 없는 집합들이다. 다시 말해 교집합이 없다.
- 집합에 속한 하나의 특정 멤버를 통해 각 집합들을 구분한다. 이를 대표자(representative)라 한다.

- 상호배타 집합을 표현하는 방법
  - 연결 리스트
  - 트리
- 상호배타 집합 연산
  - Make-Set( x )
  - Find-Set( x )
  - Union( x, y )

![image-20220401111735092](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220401111735092.png)

- 상호배타 집합 표현 - 연결 리스트
  - 
- 상호배타 집합 표현 - 트리
  - 하나의 집합(a disjoint set)을 하나의 트리로 표현한다.
  - 자식 노드가 부모 노드를 가리키며 루트 노드가 대표자가 된다.

부모 인덱스로 자식 번호 저장

**자식 인덱스로 부모 번호 저장** < (+ 루트노드는 자기자신을 저장, 대표자)

![image-20220401112406452](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220401112406452.png)

![image-20220401112415453](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220401112415453.png)
