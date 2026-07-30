## 이진 탐색 (Binary Search)

이진 탐색은 **정렬된 배열(또는 리스트)에서 탐색 범위를 절반씩 줄여가며 원하는 값을 찾는 탐색 알고리즘**이다

- 장점
    - 탐색 범위를 절반씩 줄여 효율적으로 탐색하기 때문에 시간 복잡도가 O(logn)으로 매우 빠르다
- 단점
    - 반드시 **정렬된 데이터**에서만 사용할 수 있다

### 동작 과정

1. 배열의 가운데 원소(mid)를 선택한다
2. 찾는 값과 가운데 원소를 비교한다
3. 찾는 값이 더 작으면 왼쪽, 더 크면 오른쪽 구간을 탐색한다
4. 값을 찾거나 탐색 범위가 없어질 때까지 반복한다

---

```c
int BinarySearch(int arr[], int n, int target) {
    int left = 0;
    int right = n - 1;

    while (left <= right) {
        int mid = (left + right) / 2;
        
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }

    return -1;
}
```

### 시간 복잡도

최선 : O(1) 

→ 정렬되어 있는 경우

평균 / 최악 : O(log n)

→ 이유 : 탐색할 때마다 탐색 범위가 절반으로 줄어들기 때문

### 사용되는 곳?

- DB 검색
- Lower Bound / Upper Bound 문제
- Parametric Search