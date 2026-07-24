## 병합 정렬 (Merge Sort)

탐색 범위가 정해진 배열을 절반씩 분할하는 과정을 반복한 후, 각 배열을 정렬하면서 하나의 배열로 병합하여 정렬하는 방법

- 장점
    - 시간 복잡도가 항상 O(nlogn)이고 Stable하다
    - 대용량 데이터 정렬에 적합하다
- 단점
    - 추가 메모리가 필요하며 구현이 비교적 복잡하다 (각 merge함수)

---

```c
mergeSort(left, right) {

    if (left >= right)
        return;

    mid = (left + right) / 2;

    mergeSort(left, mid);
    mergeSort(mid + 1, right);

    merge(left, mid, right);
}
```

시간 복잡도 : O(nlogn)