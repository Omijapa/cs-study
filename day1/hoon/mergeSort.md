# Day 1-3 Merge Sort 합병 정렬

## 개념 (WooVictory ver)
- 빠른 정렬로 분류되며, Quick Sort와 함께 많이 언급되는 정렬 방식이다.
- Quick Sort와는 반대로 안정 정렬에 속한다.

## 로직 (Python)
```python

# 일반적인 방식(left, right, mid 인덱스를 사용한 정렬)
# 인덱스 기반 Merge Sort
def merge_sort(arr):
    temp = [0] * len(arr)

    def sort(left, right):
        # 원소가 1개가 될 때까지 나눔
        if left >= right:
            return
        
        mid = (left + right) // 2

        # mid 기준 왼쪽 부분 배열 정렬
        sort(left, mid)

        # mid 기준 오른쪽 부분 배열 정렬
        sort(mid + 1, right)

        # 정렬된 두 부분 병합
        merge(left, mid ,right)
    
    def merge(left, mid ,right):
        i = left # 왼쪽 부분 배열 시작 인덱스
        j = mid + 1 # 오른쪽 부분 배열 시작 인덱스
        k = left # temp 배열에 값을 넣을 인덱스

        # 왼쪽과 오른쪽 부분 배열을 비교하면서 작은 값을 temp에 넣음
        while i <= mid and j <= right:
            if arr[i] <= arr[j]:
                temp[k] = arr[i]
                i += 1
            else:
                temp[k] = arr[j]
                j += 1
            k += 1
        
        # 왼쪽 부분 배열에 값이 남아있는 경우
        while i <= mid:
            temp[k] = arr[i]
            i += 1
            k += 1

        # 오른쪽 부분 배열에 값이 남아있는 경우
        while j <= right:
            temp[k] = arr[j]
            j += 1
            k += 1
        
        # temp 배열에 정렬된 값을 원본 배열에 복사
        for idx in range(left, right + 1):
            arr[idx] = temp[idx]
    
    sort(0, len(arr) - 1)
    return arr

arr = [7, 6, 2, 4, 3, 9, 1]
merge_sort(arr)

print(arr)

# 단계별 결과
###
# 원소 1개 될 때까지 나눔
[7, 6, 2, 4, 3, 9, 1]
→ [7, 6, 2, 4] [3, 9, 1]
→ [7, 6] [2, 4] [3, 9] [1]
→ [7] [6] [2] [4] [3] [9] [1]

# 합병
[7] [6] → [6, 7]
[2] [4] → [2, 4]
[3] [9] → [3, 9]

[6, 7] [2, 4] → [2, 4, 6, 7]
[3, 9] [1] → [1, 3, 9]

[2, 4, 6, 7] [1, 3, 9] → [1, 2, 3, 4, 6, 7, 9]
###
```
- 반으로 나누는 과정: N → N/2 → N/4 → N/8 → ... → 1
=> 나누는 깊이: logN
- 각 단계의 병합 비용: O(N)
- 단계 수: O(logN)

시간복잡도(최선, 평균, 최악 모두): O(NlogN)
공간복잡도(temp라는 임시 배열 사용): O(N)

병합 정렬은 입력 데이터의 상태와 관계없이 배열을 계속 반으로 나누고, 각 단계에서 병합을 수행하므로 최선, 평균, 최악 모두 시간복잡도가 O(NlogN)이다.

## 장점
- 데이터의 분포에 영향을 덜 받는다. 즉, 입력 데이터가 무엇이든 간에 정렬되는 시간은 동일하다. -> O(N logN)
- 크기가 큰 레코드를 정렬한 경우, LinkedList를 사용한다면 병합 정렬은 퀵 정렬을 포함한 다른 어떤 정렬 방법보다 효율적이다.
- 안정 정렬에 속한다.

## 단점
- 배열 기반으로 병합 정렬을 구현하면 병합 과정에서 임시 배열이 필요하다.
    - 추가 메모리가 필요하므로 공간복잡도는 O(N)이다.
    - 기존 배열 안에서만 정렬하는 방식이 아니므로 제자리 정렬이 아니다.
- 레코드의 크기가 큰 경우, 데이터를 임시 배열로 복사하고 다시 원본 배열로 옮기는 비용이 커질 수 있다.

## 참고
