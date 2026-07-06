# day4- 순열(permutation) 알고리즘

## 1. 순열이란?
- 수학 시간에 배웠던 그 순열 맞습니다.

## 2. 순열을 구하는 방법
### 2-1. visited 방식
- 이미 사용한 원소를 체크하면서 결과 배열에 추가함
- 직관적임

### 2-2. swap 방식
- 배열 원소 위치를 바꿔가며 앞자리부터 고정
- 추가 배열 없이 구현 가능

## 3. Visited 방식
- 현재까지 선택한 원소를 path(배열)에 담고, 이미 선택한 원소는 visited 배열에 표시하여 다시 선택하지 않도록 하는 방식
```text
(ex) 배열 [1,2,3]
1 선택 -> visited[0] = True
2 선택 -> visited[1] = True
3 선택 -> visited[2] = True
[1,2,3] 경우 완성

...

다시 돌아가면서 다른 경우 찾음

```
### visited 방식 코드(python)
```python
# 이미 사용한 원소는 visited = true, 현재까지 뽑은 원소는 path에 담는다

def permutation_visited(arr, k): # arr에서 k개를 뽑아서 순열을 만든다
    # visited, path 초기화
    visited = [False] * len(arr) 
    path = []

    def backtrack(depth):
        # k개를 모두 뽑았으면 출력
        if depth == k:
            print(path)
            return
        
        for i in range(len(arr)):
            if not visited[i]:
                visited[i] = True # 방문 표시
                path.append(arr[i]) # 현재 숫자 추가

                backtrack(depth + 1) # 다음 자리 선택

                path.pop() # path 복구..
                visited[i] = False # 원상 복구
        
    backtrack(0) # depth 0부터 실행

arr = [1,2,3]
permutation_visited(arr, 3)
'''
출력:
[1,2,3]
[1,3,2]
[2,1,3]
[2,3,1]
[3,1,2]
[3,2,1]
'''
```



## 4. Swap 방식
- 배열의 원소를 직접 교환하면서 현재 depth 위치에 올 원소를 하나씩 고정하는 방법
```text
(ex) [1,2,3]에서 depth = 0인 경우
[1,2,3] -> 첫 번째 자리에 1 고정 -> depth = 1 진행 ~~
[2,1,3] -> 첫 번째 자리에 2 고정 -> depth = 1 진행 ~~
[3,2,1] -> 첫 번째 자리에 3 고정 -> depth = 1 진행 ~~
...
각 경우마다 다음 depth로 들어가서 뒤쪽 원소들의 순열을 만듦

```

### swap 방식 코드(python)
```python
# 현재 depth 위치에 올 숫자를 swap으로 바꿔가며 고정함
# 재귀가 끝나면 다시 swap하여 원래 상태로 되돌림

def permutation_swap(arr, k): # arr에서 k개를 뽑아서 순열을 만든다
    def backtrack(depth):
        # k개를 모두 정했으면 출력
        if depth == k:
            print(arr[:k])
            return

        for i in range(depth, len(arr)):
            arr[depth], arr[i] = arr[i], arr[depth] # 현재 위치에 i번째 숫자 고정

            backtrack(depth + 1) # 다음 자리 선택

            arr[depth], arr[i] = arr[i], arr[depth] # 원상복구
    
    backtrack(0) # depth 0부터 실행

arr = [1, 2, 3]
permutation_swap(arr, 3)

'''
출력:
[1, 2, 3]
[1, 3, 2]
[2, 1, 3]
[2, 3, 1]
[3, 2, 1]
[3, 1, 2]
'''

```

## 5. 시간/공간복잡도
- visited, swap 방식 둘 다 순열을 전부 만들어야 함 => 시간복잡도 같음
nPk 기준으로..
- n = 전체 원소 개수
- k = 뽑을 원소 개수
- nPk = n개 중 k개를 순서를 고려해서 뽑는 경우의 수

### visited 방식
순열 개수는 nPk개, 순열 하나를 출력하거나 복사할 때 k만큼 걸림
- 시간복잡도: O(k * nPk)
=> 전체를 다 뽑는 경우(k = n): O(n * n!)

- 공간복잡도: O(n + k)
<= visited 배열 크기 O(n), path 배열 크기 O(k), 재귀 호출 깊이 O(k)

### swap 방식
출력할 때 arr[:k]를 하면 앞에서 k개를 복사함
- 시간복잡도: O(k * nPk)
=> 전체를 다 뽑는 경우(k = n): O(n * n!)

- 공간복잡도: O(k)
<= 추가 배열 사용 x, 재귀 호출이 k 깊이까지 들어감

## 참고
- visited 방식 출력의 경우 단순 출력은 print(path)로 가능하지만 결과를 저장해야 할 때는 result.append(path[:]) 처럼 복사해서  저장해야 한다.

- visited 방식의 공간복잡도에서.. 보통 k <= n이므로 O(n)이라 표현하기도 함
- swap 방식의 공간복잡도에서.. k = n인 경우(전체 순열): O(n)

- 배열에 중복이 있는 경우 set을 이용하여 해결할 수 있다.
```python
def permutation_swap_unique(arr, k):
    def backtrack(depth):
        if depth == k:
            print(arr[:k])
            return

        used = set()  # 현재 depth에서 이미 고정한 값들

        for i in range(depth, len(arr)):
            if arr[i] in used: # 이번 depth에서 이미 사용했음(중복)
                continue

            used.add(arr[i])

            arr[depth], arr[i] = arr[i], arr[depth]

            backtrack(depth + 1)

            arr[depth], arr[i] = arr[i], arr[depth]

    backtrack(0)


arr = [1, 1, 2]
permutation_swap_unique(arr, 3)
```

- 재귀는 항상 이해하기 쉽지 않구려..