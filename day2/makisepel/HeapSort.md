## Heap Sort (힙 정렬)

(log2n에서 2는 밑)

탐색 범위가 정해진 배열을 최대 힙(Max Heap) 또는 최소 힙(Min Heap) 구조로 만든 후, 루트 노드와 마지막 원소를 교환하고 힙을 재구성하는 과정을 반복하여 정렬하는 방법

- 장점 :
    - 입력 데이터의 상태에 영향을 받지 않으며 추가 메모리가 거의 필요하지 않다 (In-place)
    - 항상 O(nlog2n)의 시간 복잡도를 보장한다
- 단점 :
    - 구현이 비교적 복잡하며 Stable하지 않다

1. 배열을 최대 힙(Max Heap)으로 구성한다.
2. 루트 노드(최댓값)와 마지막 원소를 교환한다.
3. 힙의 크기를 1 감소시킨 후, 루트부터 다시 힙을 재구성(Heapify)한다.
4. 힙의 크기가 1이 될 때까지 위 과정을 반복한다.

---

```c
heapSort() {
    for (i = n / 2 - 1; i >= 0; i--)
        heapify(n, i);

    for (i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(i, 0);
    }
}

heapify(size, root) {
    largest = root;

    left = root * 2 + 1;
    right = root * 2 + 2;

    if (left < size && arr[left] > arr[largest]) largest = left;

    if (right < size && arr[right] > arr[largest]) largest = right;

    if (largest != root) {
        swap(arr[root], arr[largest]);
        heapify(size, largest);
    }
}
```

### 시간 복잡도 : 항상 O(nlog2n)