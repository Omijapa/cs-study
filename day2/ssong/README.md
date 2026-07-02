# Day 2 - 퀵 정렬, 힙 정렬

# 퀵 정렬

퀵 정렬은 이름 그대로 매우 빠른 정렬 알고리즘이다. 평균 시간 복잡도는 $O(n \log(n))$으로 병합 정렬과 가지만, 실제 환경에서는 더 우수한 성능을 보이는 경우가 많다. 이는 퀵 정렬의 분할 과정이 대부분의 컴퓨터 아키텍처에서 효율적으로 동작하도록 설계되어 있으며, 최악의 경우인 $O(n^2)$이 발생할 확률도 낮기 때문이다.

또한 퀵 정렬은 제자리 정렬을 사용해 분할 과정에서 처음부터 끝까지 순차 탐색하므로 연속된 메모리 영역에 대한 접근이 많아 참조 지역성(Locality)이 높다. 따라서 CPU 캐시율이 높아져 실제 실행 시간이 단축된다.

반면 병합 정렬은 병합 과정에서 추가 배열을 사용해야 하므로 메모리 사용량이 증가하며, 원본 배열과 임시 배열 사이의 복사가 반복되어 캐시 효율이 상대적으로 낮다.

퀵 정렬의 동작 과정은 다음과 같다.

1. 리스트 가운데서 하나의 원소를 고른다. 이렇게 고른 원소를 피벗이라고 한다.
2. 피벗 앞에는 피벗보다 값이 작은 모든 원소들이 오고, 피벗 뒤에는 피벗보다 값이 큰 모든 원소들이 오도록 피벗을 기준으로 리스트를 둘로 나눈다. 이렇게 리스트를 둘러 나누는 것을 분할이라고 한다. 분할을 마친 뒤 피벗은 더 이상 움직이지 않는다.
3. 분할된 두 개의 작은 리스트에 대해 재귀적으로 이 과정을 반복한다. 재귀는 리스트의 크기가 0이나 1이 될 때까지 반복된다.

재귀 호출이 한번 진행될때 마다 최소한 하나의 원소는 최종적으로 위치가 정해지므로, 알고리즘의 종결성이 보장된다.

#### C++

```cpp
#include <iostream>
#include <vector>
using namespace std;

int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }

    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pivotIndex = partition(arr, low, high);

        quickSort(arr, low, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, high);
    }
}

int main() {
    vector<int> arr = {5, 3, 8, 4, 2, 7, 1, 10};

    quickSort(arr, 0, arr.size() - 1);

    for (int num : arr) {
        cout << num << " ";
    }
}
```

#### Java

```java
public class Main {

    static int partition(int[] arr, int low, int high) {
        int pivot = arr[high];
        int i = low - 1;

        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;

                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }

        int temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;

        return i + 1;
    }

    static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pivotIndex = partition(arr, low, high);

            quickSort(arr, low, pivotIndex - 1);
            quickSort(arr, pivotIndex + 1, high);
        }
    }

    public static void main(String[] args) {
        int[] arr = {5, 3, 8, 4, 2, 7, 1, 10};

        quickSort(arr, 0, arr.length - 1);

        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}
```

### 시간 복잡도

| 최선 | $O(n \log (n))$ |
| ---- | --------------- |
| 평균 | $O(n \log (n))$ |
| 최악 | $O(n^2)$        |

### 공간 복잡도

$O(n)$ : 재귀 호출 스택

### 장점

- 평균적으로 매우 빠르고 캐시 효율이 좋다.
- 추가적인 메모리 사용이 적다. (재귀 스택만 사용)
- 구현이 비교적 간단하다.

### 단점

- 불안정 정렬이다.
- 기준이 되는 pivot 값에 따라 성능이 크게 달라진다.

---

# 힙 정렬

> 최대값(혹은 최소값)을 빠르게 찾을 수 있는 힙을 만든 뒤, 루트 원소를 하나씩 꺼내면서 정렬한다.

힙 정렬은 힙(Heap) 자료구조를 이용해 정렬하는 알고리즘이다. 최대 힙 트리(오름차순)나 최소 힙 트리(내림차순)를 구성해 정렬을 수행한다.

동작 과정은 다음과 같다.

1. n개의 노드에 대한 완전 이진 트리를 구성한다. 이때 루트노드부터 부모노드, 왼쪽 자식노드, 오른쪽 자식노드 순으로 구성한다.
2. 최대 힙을 구성한다.
3. 가장 큰 수(루트에 위치)를 마지막 원소와 교환한다.
4. 2와 3을 반복한다.

#### C++

```cpp
#include <iostream>
#include <vector>
using namespace std;

void heapify(vector<int>& arr, int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && arr[left] > arr[largest])
        largest = left;

    if (right < n && arr[right] > arr[largest])
        largest = right;

    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(vector<int>& arr) {
    int n = arr.size();

    // 최대 힙 생성
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    // 정렬
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}

int main() {
    vector<int> arr = {4, 10, 3, 5, 1};

    heapSort(arr);

    for (int num : arr)
        cout << num << " ";
}
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

void heapify(vector<int>& arr, int n, int i) {

    while (true) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;

        if (left < n && arr[left] > arr[largest])
            largest = left;

        if (right < n && arr[right] > arr[largest])
            largest = right;

        if (largest == i)
            break;

        swap(arr[i], arr[largest]);

        i = largest;
    }
}

void heapSort(vector<int>& arr) {
    int n = arr.size();

    // 최대 힙 생성
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    // 정렬
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}

int main() {
    vector<int> arr = {4, 10, 3, 5, 1};

    heapSort(arr);

    for (int x : arr)
        cout << x << " ";
}
```

#### Java

```java
public class Main {

    static void heapify(int[] arr, int n, int i) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;

        if (left < n && arr[left] > arr[largest]) {
            largest = left;
        }

        if (right < n && arr[right] > arr[largest]) {
            largest = right;
        }

        if (largest != i) {
            int temp = arr[i];
            arr[i] = arr[largest];
            arr[largest] = temp;

            heapify(arr, n, largest);
        }
    }

    static void heapSort(int[] arr) {
        int n = arr.length;

        // 최대 힙 생성
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(arr, n, i);
        }

        // 정렬
        for (int i = n - 1; i > 0; i--) {
            int temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;

            heapify(arr, i, 0);
        }
    }

    public static void main(String[] args) {
        int[] arr = {4, 10, 3, 5, 1};

        heapSort(arr);

        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}
```

```java
public class Main {

    static void heapify(int[] arr, int n, int i) {

        while (true) {
            int largest = i;
            int left = 2 * i + 1;
            int right = 2 * i + 2;

            if (left < n && arr[left] > arr[largest]) {
                largest = left;
            }

            if (right < n && arr[right] > arr[largest]) {
                largest = right;
            }

            if (largest == i) {
                break;
            }

            int temp = arr[i];
            arr[i] = arr[largest];
            arr[largest] = temp;

            i = largest;
        }
    }

    static void heapSort(int[] arr) {
        int n = arr.length;

        // 최대 힙 생성
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(arr, n, i);
        }

        // 정렬
        for (int i = n - 1; i > 0; i--) {
            int temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;

            heapify(arr, i, 0);
        }
    }

    public static void main(String[] args) {
        int[] arr = {4, 10, 3, 5, 1};

        heapSort(arr);

        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}
```

### 시간 복잡도

| 최선 | $O(n \log (n))$ |
| ---- | --------------- |
| 평균 | $O(n \log (n))$ |
| 최악 | $O(n \log (n))$ |

### 공간 복잡도

$O(1)$ : 반복문으로 구현 시

$O(\log n)$ : 재귀문으로 구현 시

### 장점

- 퀵 정렬과 달리 최악의 경우도 $O(n \log n)$이 보장된다.
- 추가적인 메모리 사용이 없다.
- 입력 데이터에 영향을 받지 않는다.

### 단점

- 캐시 효율이 나쁘다.
- 일반적으로 퀵 정렬보다 느리다.
- 불안정 정렬이다.
