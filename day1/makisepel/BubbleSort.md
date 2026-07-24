## 버블 정렬 (Bubble Sort)

탐색 범위가 정해진 배열에서, 인접한 두 원소를 비교 및 정렬하여 순차적으로 진행해 정렬하는 방법

- 장점
    - 구현이 매우 간단하고 Stable하다
    - 배열이 정렬되어 있을 수록 시간 복잡도가 낮다
- 단점
    - 비교와 교환 횟수가 많아 매우 비효율적이다
    - 큰 데이터에서는 사용하기 어렵다

---

```c
for (i = 0; i < n - 1; i++) {

    swapped = false;

    for (j = 0; j < n - i - 1; j++) {

        if (arr[j] > arr[j + 1]) {
            swap(arr[j], arr[j + 1]);
            swapped = true;
        }
    }

    if (swapped == false)
        break;
}
```

시간 복잡도 : O(n^2) → 정렬되어 있을 경우 O(n)