# Binary Search

Date: 2026년 7월 8일
Status: Done

# 개념

<aside>
📜

**이미 정렬되어 있는 배열**에서 target을 찾는 탐색 알고리즘

탐색 범위를 매 단계마다 절반으로 자르면서 target을 찾아나가는데, 흔히 상대방 나이 맞출 때 업다운 게임 하는거랑 같은 원리이다.

1. 탐색 범위의 양 끝을 각각 left, right, 중간값을 mid로 정의한다.
2. target이 mid보다 크면 [mid + 1, right]를 새로운 탐색 범위로, target이 mid보다 작으면 [left, mid - 1]를 새로운 탐색 범위로 하여 1을 반복한다.

** 자바를 사용한다면, mid = (left + right) / 2 연산에서 오버플로우를 조심해야한다. 
(대충 배열 크기가 수십억이다? 비트연산자로 하는게 안전함) → int mid = (left + right) >>> 1; 

</aside>

---

# 구현

### Python (반복문 구현)

```python
def binary_search_loop(arr, target):
    left = 0
    right = len(arr) - 1

    while left <= right:
        mid = (left + right) // 2

        # 1. 찾고자 하는 값을 발견한 경우 인덱스 반환
        if arr[mid] == target:
            return mid
        # 2. 중간값이 타겟보다 작다면 오른쪽 영역 탐색 (left 이동)
        elif arr[mid] < target:
            left = mid + 1
        # 3. 중간값이 타겟보다 크다면 왼쪽 영역 탐색 (right 이동)
        else:
            right = mid - 1

    return -1  # 찾고자 하는 값이 배열에 없는 경우

# 테스트
test_arr = [1, 3, 5, 7, 9, 11, 13, 15]
print(binary_search_loo(test_arr, 7))  # 출력: 3 (인덱스 번호)
```

### Python (재귀 구현)

```python
def binary_search_recursion(arr, target, left, right):
    if left > right:
        return -1

    mid = (left + right) // 2

    # 1. 찾고자 하는 값을 발견한 경우 인덱스 반환
    if arr[mid] == target:
        return mid
    
    # 2. 중간값이 타겟보다 작다면 오른쪽 영역 탐색
    elif arr[mid] < target:
        return binary_search_recursion(arr, target, mid + 1, right)
    
    # 3. 중간값이 타겟보다 크다면 왼쪽 영역 탐
    else:
        return binary_search_recursion(arr, target, left, mid - 1)
test_arr = [1, 3, 5, 7, 9, 11, 13, 15]
target_num = 7

result_idx = binary_search_recursion(test_arr, target_num, 0, len(test_arr) - 1)
print(f"타겟의 인덱스 위치: {result_idx}")  # 출력: 3
```

### Python (binary search를 기반으로 하는 bisect 라이브러리 사용)

```python
from bisect import bisect_left, bisect_right

arr = [1, 3, 5, 7, 7, 7, 9, 11]

# 값이 7인 데이터의 시작 인덱스와 끝 인덱스 찾기
print(bisect_left(arr, 7))   # 출력: 3
print(bisect_right(arr, 7))  # 출력: 6 (7 다음 숫자가 들어갈 자리)

# 응용: 특정 범위 [left_value, right_value]에 속하는 원소의 개수 구하기
def count_by_range(arr, left_val, right_val):
    return bisect_right(arr, right_val) - bisect_left(arr, left_val)

print(count_by_range(arr, 7, 7)) # 배열에 포함된 '7'의 개수 출력: 3
```

### Java (반복문 구현)

```python
public class BinarySearchLoop {

    public static int binarySearch(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;

        while (left <= right) {
            // 중앙값 계산
            int mid = left + (right - left) / 2;

            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return -1;
    }

    public static void main(String[] args) {
        int[] testArr = {1, 3, 5, 7, 9, 11, 13, 15};
        int target = 11;

        int resultIdx = binarySearch(testArr, target);
        System.out.println("타겟의 인덱스 위치: " + resultIdx); // 출력: 5
    }
}
```

### Java (재귀 구현)

```python
public class BinarySearchRecursion {

    public static int binarySearch(int[] arr, int target, int left, int right) {
        if (left > right) {
            return -1;
        }

        int mid = left + (right - left) / 2;

        if (arr[mid] == target) {
            return mid;
        }
        else if (arr[mid] < target) {
            return binarySearch(arr, target, mid + 1, right);
        
        else {
            return binarySearch(arr, target, left, mid - 1);
        }
    }

    public static void main(String[] args) {
        int[] testArr = {1, 3, 5, 7, 9, 11, 13, 15};
        int target = 7;

        int resultIdx = binarySearch(testArr, target, 0, testArr.length - 1);
        
        System.out.println("타겟의 인덱스 위치: " + resultIdx); // 출력: 3
    }
}
```

### Java (라이브러리 사용)

```python
import java.util.Arrays;

public class BinarySearch {
    public static void main(String[] args) {
        int[] arr = {1, 3, 5, 7, 9, 11, 13, 15};

        // 1. 존재하는 값을 찾을 때
        int index = Arrays.binarySearch(arr, 7);
        System.out.println("7의 위치: " + index); // 출력: 3

        // 2. 존재하지 않는 값을 찾을 때 -> 기존 배열의 정렬을 유지한 채 target이 들어갈 위치 * (-1) - 1 을 반환
        int 매칭실패인덱스 = Arrays.binarySearch(arr, 6);
        System.out.println("6의 결과: " + 매칭실패인덱스); // 출력: -4 -> -3-1이므로 3번째 인덱스에 target이 들어갈 수 있다는 것을 뜻함.
        
    }
}
```

---

# 시간복잡도

반복문 구현: O(log n)

재귀 구현: O(log n)

공간복잡도 측면에서 반복문으로 구현하는게 좋당.