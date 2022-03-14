





- 이진 검색 알고리즘

```python
def binarySearch(a, N, key):
    start = 0
    end = N-1
    while start <= end :
        middle = (start + end) // 2
        if key == a[middle] : #검색 성공
            return true
       	elif key < a[middle] :
            end = middle - 1
        else :
            start = middle + 1
    return false #검색 실패
```

재귀 함수 이용 버전

```python
def binarySearch2(a, low, high, key) : 
    if low > high : #검색 실패
        return False
    else:
        middle = (low + high) // 2
        if key == a[middle] : #검색 성공
            return True
		elif key < a[middle] :
        	return binarySearch2(a, low, middle-1, key)
        elif a[middle] < key :
            return binarySearch2(a, middle+1, high, key)
```



### 선택 정렬(Selection Sort)

> O(n^2)

주어진 리스트 중에서 최소값을 찾는다. 그 값을 리스트의 맨 앞에 위치한 값과 교환한다. 맨 처음 위치를 제외한 나머지 리스트를 대상으로 위의 과정을 반복한다.

1. 주어진 리스트에서 최소값을 찾는다.
2. 리스트의 맨 앞에 위치한 값과 교환한다.
3. 2가 끝나고 난 뒤의 미정렬 리스트에서 최소값을 찾는다.
4. 2의 작업을 수행한다(미정렬된 리스트의 맨 앞에 위치한 값(1에서 찾은 최소값보다는 큰))과 교환한다.

```python
def selectionSort(a, N) :
    for i in range(N-1) :
        minIdx = i
        for j in range(i+1, N) :
            if a[minIdx] > a[j] :
                minIdx = j
        a[i], a[minIdx] = a[minIdx], a[i]
```

