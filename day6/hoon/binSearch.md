# day 6-1. Binary Search (이분 탐색)

## 1. 개념
- 이미 정렬되어 있는 자료 구조에서 특정 값을 빠르게 찾기 위한 탐색 알고리즘
- 탐색 범위를 절반씩 줄여가며 원하는 값을 찾음 -> 선형 탐색보다 빠름
- 자료가 반드시 정렬되어 있어야 이진 탐색 가능

## 2. 알고리즘 흐름
1. 배열(arr) 정렬
2. 탐색 범위의 시작 인덱스를 left, 끝 인덱스를 right로 둠
3. left와 right의 중간인 mid를 구함
4. arr[mid]와 찾고자하는 값(target)을 비교
5. target == arr[mid]이면 값을 찾은 것이므로 탐색 종료
6. target < arr[mid]이면 찾는 값은 왼쪽에 있으므로 right = mid - 1로 변경(범위 절반으로 줄임)
7. target > arr[mid]이면 찾는 값은 오른쪽에 있으므로 left = mid + 1로 변경(범위 절반으로 줄임)
8. left가 right보다 커질 때까지 5-7 반복
9. 끝까지 찾지 못하면 -1을 반환, 종료

## 3. 코드(python)
```python
# 직접 구현
def binary_search(arr, target):
    arr.sort()

    left = 0
    right = len(arr) - 1

    while left <= right:
        mid= (left + right) // 2

        if arr[mid] == target:
            print(f"Find Target: {target}, Value : {arr[mid]}")
            return arr[mid]
        
        if target < arr[mid]:
            right = mid - 1
        
        else:
            left = mid + 1

    return -1

if __name__ == "__main__":
    arr = [2, 13, 6, 5, 12, 15, 23, 17, 19, 10]

    print(binary_search(arr, 17))

'''
결과
Find Target : 17, Value : 17
17
'''
```
```python
# 파이썬 bisect 모듈 사용
import bisect

def binary_search(arr, target):
    arr.sort()

    index = bisect.bisect_left(arr, target) # 파이썬은 신이야..

    if index < len(arr) and arr[index] == target:
        print(f"Find Target : {target}, Value : {arr[index]}")
        return arr[index]

    return -1


if __name__ == "__main__":
    arr = [2, 13, 6, 5, 12, 15, 23, 17, 19, 10]

    print(binary_search(arr, 17))

'''
결과
Find Target : 17, Value : 17
17
'''
```


## 4. 시간복잡도 / 공간복잡도
### 4.1. 시간복잡도
데이터 개수가 N개일 때, 탐색 범위는 
```text
 N -> N/2 -> N/4 -> N/8 ...
```
이분 탐색만 했을 경우의 시간복잡도: O(logN)

- 탐색 전 sort()를 사용하여 배열을 정렬했하게 될 경우(arr = sorted(arr))
정렬: O(NlogN)
탐색: O(logN)
-> 전체 시간복잡도: O(NlogN)

### 4.2. 공간복잡도
- 반복문 방식의 이진 탐색
- left, right, mid 정도의 변수만 사용
공간복잡도: O(1)