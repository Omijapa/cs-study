# Permutation

Date: 2026년 7월 6일
Status: Not started

---

# 개념

<aside>
📜

순열, 중복순열, 조합, 중복조합의 개념은 익히 알고 있을 것이다.

중요한 건, 어떻게 구현할 것이지..!

- 서로 다른 n개 중에서 r개를 택해 순서를 고려하여 나열
- 서로 다른 n개 중에서 중복을 포함하여 r개를 택해 순서를 고려하여 나열
- 서로 다른 ‭$n$‬개 중에서 순서와 상관없이 $r$‬개를 선택하는 경우의 수
- 서로 다른 ‭$n$‬개 중에서 순서와 상관없이 중복을 포함하여 $r$‬개를 선택하는 경우의 수
</aside>

---

# 구현

### 순열

```python
import itertools

def permute(arr, n):
    result = []
    visited = [False] * len(arr)

    def dfs(curr):
        if len(curr) == n:
            result.append(curr[:])
            return

        for i in range(len(arr)):
            if not visited[i]:
                visited[i] = True
                curr.append(arr[i])

                dfs(curr)

                curr.pop()
                visited[i] = False

    dfs([])
    return result

def permute_with_replacement(arr, n):
    result = []

    def dfs(curr):
        if len(curr) == n:
            result.append(curr[:])
            return

        for i in range(len(arr)):
            curr.append(arr[i])
            dfs(curr)
            curr.pop()

    dfs([])
    return result

if __name__ == '__main__':
    test_case = [3, 9, 6]
    print("순열 직접 구현(2개): ", permute(test_case, 2))
    print("중복 순열 직접 구현(2개): ", permute_with_replacement(test_case, 2))
    print("순열 구현(2개): ", list(itertools.permutations(test_case, 2)))
    print("중복 순열 구현(2개): ", list(itertools.product(test_case, repeat = 2)))
```

### 조합

```python
import itertools

def combine(arr, n):
    result = []

    def dfs(curr, start):
        if len(curr) == n:
            result.append(curr[:])
            return

        for i in range(start, len(arr)):
            curr.append(arr[i])

            dfs(curr, i + 1)

            curr.pop()

    dfs([], 0)
    return result

def combine_with_replacement(arr, n):
    result = []

    def dfs(curr, start):
        if len(curr) == n:
            result.append(curr[:])
            return

        for i in range(start, len(arr)):
            curr.append(arr[i])

            dfs(curr, i)

            curr.pop()

    dfs([], 0)
    return result

if __name__ == '__main__':
    test_case = [3, 9, 6]
    print("조합 직접 구현(2개): ", combine(test_case, 2))
    print("중복 조합 직접 구현(2개): ", combine_with_replacement(test_case, 2))
    print("조합 구현(2개): ", list(itertools.combinations(test_case, 2)))
    print("중복 조합 구현(2개): ", list(itertools.combinations_with_replacement(test_case,2)))
```

---
# 예제

> 1부터 n까지의 수로 구성된 배열이 있다. 내부의 수들을 나열하는 방법을 사전 순으로 나열했을 때, k번째 방법을 구하시오.

**프로그래머스 줄 서는 방법 참조**
> 

```bash
보자마자 nPn을 구현 후, k번째를 반환한다면 시간 초과가 발생할 것이다. (완전 탐색이기 떄문)

첫 번째 자리부터 끝까지 k번째 나열이라면 어떤 수들이 들어가는지에 대한 규칙을 파악한 후 구현해야 한다.
```

```python
import math

def solution(n, k):
    answer = []
    numbers = list(range(1, n + 1))
    
    k -= 1
    
    while n > 0:
        fact = math.factorial(n - 1)
        idx = k // fact
        answer.append(numbers.pop(idx))
        k %= fact
        n -= 1
        
    return answer
```

---

# 시간복잡도

| 순열 | 평균: O(nPr)
최악: O(n!) |
| --- | --- |
| 중복순열 | O(2^n) |
| 조합 | 평균: O(nCr)
최악: O(2^n) |
| 중복조합 | O(n^r) |