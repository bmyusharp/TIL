# 큐

인접한 칸의 인접한 칸 ... -> BFS(너비 우선 탐색)



## 토마토 (익은 토마토의 옆칸이 익음)

``` python
def bfs(N, M):
    visited = [[0] * M for _ in range(N)] #visited 생성
    q = [] #큐 생성
    #시작점 인큐(토마토 위치)
    for i in range(N):
        for j in range(M):
            if tomato[i][j] == 1:
                q.append(i, j)
                #시작점 방문(인큐)표시
                visited[i][j] = 1
    #큐가 비어 있지 않으면...
    while q:
        i, j = q.pop(0) #익은 토마토
        # visit(i, j)
        for di, dj in [[0, 1], [1, 0], [0, -1], [-1, 0]]:
            ni, nj = i + di, j + dj
            if 0 <= ni < N and 0 <= dj < M and tomato[ni][nj] == 0 and visited[ni][nj]==0:
                q.append((ni, nj))
                visited[ni][nj] = visited[i][j] + 1
    # 익지않은 토마토가 있으면 -1 리턴(비어있는 칸이 있는데도, 익지 않으면)
    # 모두 익은 경우 visited 최대값 리턴
    maxV = 0
    for i in range(N):
        for j in range(M):
            if tomato[i][j] == 0:
                if visited[i][j] == 0:
                	return -1 #안 익은 경우
                elif maxV < visited[i][j]:
                    maxV = visited[i][j] #익은 경우 날짜 비교
	return maxV - 1 #최대값 출력(처음 토마토가 1부터 시작하므로)
            
    
    
M, N = map(int, input().split())
tomata = [list(map(int, input().split())) for _ in range(N)]
```



## 단지번호붙이기

