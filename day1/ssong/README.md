# Day 1 - 선택/버블/병합/삽입 정렬

# 선택 정렬 (Selection Sort)

> 정렬되지 않은 구간에서 최소값을 찾은 뒤 현재 위치와 교환

![](https://upload.wikimedia.org/wikipedia/commons/9/94/Selection-Sort-Animation.gif)

선택 정렬은 제자리 정렬 알고리즘의 하나로 다음과 같은 순서로 동작한다.

1. 주어진 리스트 중에 최소값을 찾는다.
2. 그 값을 맨 앞에 위치한 값과 교체한다.
3. 맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.

#### C++

```cpp
#include <iostream>
#include <vector>

using namespace std;

void selectionSort(vector<int>& arr) {
    int n = arr.size();

    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;

        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }

        swap(arr[i], arr[minIdx]);
    }
}

int main() {
    vector<int> arr = {64, 25, 12, 22, 11};

    selectionSort(arr);

    for (int num : arr) {
        cout << num << " ";
    }

    return 0;
}
```

#### Java

```java
import java.util.Arrays;

public class Main {
    public static void selectionSort(int[] arr) {
        int n = arr.length;

        for (int i = 0; i < n - 1; i++) {
            int minIdx = i;

            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIdx]) {
                    minIdx = j;
                }
            }

            int temp = arr[i];
            arr[i] = arr[minIdx];
            arr[minIdx] = temp;
        }
    }

    public static void main(String[] args) {
        int[] arr = {64, 25, 12, 22, 11};

        selectionSort(arr);

        System.out.println(Arrays.toString(arr));
    }
}
```

### 시간 복잡도

| 최선 | $O(n^2)$ |
| ---- | -------- |
| 평균 | $O(n^2)$ |
| 최악 | $O(n^2)$ |

### 공간 복잡도

$O(1)$ : 제자리 정렬

### 장점

- 알고리즘이 단순하다.
- 정렬을 위한 추가적인 메모리 공간을 필요로 하지 않는다.

### 단점

- 시간 복잡도가 $O(N^2)$으로 비효율적이다.
- 불안정 정렬(Unstable sort)이다.

---

# 삽입 정렬

> 정렬된 구간에서 현재 원소를 적절한 위치에 삽입

삽입 정렬은 앞쪽 부분은 항상 정렬되어 있다고 가정하고, 현재 원소를 알맞은 위치에 삽입하는 정렬 알고리즘이다. 동작 과정을 다음과 같다.

1. 초기 상태에서 맨 왼쪽 원소 하나는 이미 정렬된 상태로 간주한다.
2. 두번째 원소부터 차례대로 정렬된 구간의 원소들과 뒤에서부터 비교한다.
3. 선택한 원소보다 큰 값들은 한 칸씩 오른쪽으로 이동시킨다.
4. 더 이상 이동할 필요가 없으면 빈 위치에 선택한 원소를 삽입한다.
5. 모든 원소에 대해 위 과정을 반복한다.

#### C++

```cpp
#include <iostream>
#include <vector>

using namespace std;

void insertionSort(vector<int>& arr) {
    int n = arr.size();

    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }

        arr[j + 1] = key;
    }
}

int main() {
    vector<int> arr = {5, 3, 4, 1, 2};

    insertionSort(arr);

    for (int num : arr) {
        cout << num << " ";
    }
}
```

#### Java

```java
public static void insertionSort(int[] arr) {
    for (int i = 1; i < arr.length; i++) {
        int key = arr[i];
        int j = i - 1;

        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }

        arr[j + 1] = key;
    }
}

public static void main(String[] args) {
    int[] arr = {5, 3, 4, 1, 2};

    insertionSort(arr);

    for (int num : arr) {
        System.out.print(num + " ");
    }
}
```

### 시간 복잡도

| 최선 | $O(n)$   |
| ---- | -------- |
| 평균 | $O(n^2)$ |
| 최악 | $O(n^2)$ |

### 공간 복잡도

$O(1)$ : 제자리 정렬

### 장점

- 알고리즘이 단순하다.
- 정렬을 위한 추가적인 메모리 공간을 필요로 하지 않는다.
- 안정 정렬(Stable sort)이다.

### 단점

- 역순 데이터 정렬의 경우 $O(n^2)$ 시간이 필요하므로 대용량 데이터의 경우 병합 정렬이나 퀵 정렬이 유리하다.

---

# 버블 정렬 (Bubble Sort)

> 인접한 원소를 비교하여 큰 값을 뒤로 보내는 정렬

서로 인접한 두 원소를 비교하여 정렬하는 알고리즘이다.

두 수가 정렬되어 있다면 유지하고 아니라면 두 수의 위치를 바꾼다.

이 과정을 배열의 처음부터 끝까지 반복한다.

#### C++

```cpp
#include <iostream>
#include <vector>

using namespace std;

void bubbleSort(vector<int>& arr) {
    int n = arr.size();

    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;

        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }

        if (!swapped) break;
    }
}

int main() {
    vector<int> arr = {64, 34, 25, 12, 22, 11, 90};

    bubbleSort(arr);

    for (int num : arr) {
        cout << num << " ";
    }
}
```

#### Java

```java
import java.util.Arrays;

public class Main {
    public static void bubbleSort(int[] arr) {
        int n = arr.length;

        for (int i = 0; i < n - 1; i++) {
            boolean swapped = false;

            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }

            if (!swapped) break;
        }
    }

    public static void main(String[] args) {
        int[] arr = {64, 34, 25, 12, 22, 11, 90};

        bubbleSort(arr);

        System.out.println(Arrays.toString(arr));
    }
}
```

### 시간 복잡도

| 최선 | $O(n)$   |
| ---- | -------- |
| 평균 | $O(n^2)$ |
| 최악 | $O(n^2)$ |

### 공간 복잡도

$O(1)$ : 제자리 정렬

### 장점

- 알고리즘이 단순하다.
- 정렬을 위한 추가적인 메모리 공간을 필요로 하지 않는다.
- 안정 정렬(Stable sort)이다.

### 단점

- 시간 복잡도가 $O(N^2)$으로 비효율적이다.

---

# 병합 정렬 (Merge Sort)

> 배열을 계속 반으로 나눈 뒤 정렬하면서 다시 합침

![](https://upload.wikimedia.org/wikipedia/commons/c/cc/Merge-sort-example-300px.gif)

병합 정렬은 비교 기반 정렬 알고리즘이다. 분할 정복 알고리즘의 하나이며 안정 정렬이다.

동작 과정은 다음과 같다.

1. 리스트의 길이가 1 이하이면 이미 정렬된 것으로 간주한다. 그렇지 않은 경우,
2. 분할(divide): 정렬되지 않은 리스트를 절반으로 잘라 비슷한 크기의 두 부분 리스트로 나눈다.
3. 정복(conquer): 각 부분 리스트를 재귀적으로 병합 정렬을 이용해 정렬한다.
4. 결합(combine): 두 부분 리스트를 다시 하나의 정렬된 리스트로 병합한다. 이때 정렬 결과가 임시 배열에 저장된다.
5. 복사(copy): 임시 배열에 저장된 결과를 원래 배열에 복사한다.

#### C++

```cpp
#include <iostream>
#include <vector>

using namespace std;

void merge(vector<int>& arr, int left, int mid, int right) {
    vector<int> temp;

    int i = left;
    int j = mid + 1;

    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            temp.push_back(arr[i++]);
        } else {
            temp.push_back(arr[j++]);
        }
    }

    while (i <= mid) temp.push_back(arr[i++]);
    while (j <= right) temp.push_back(arr[j++]);

    for (int k = 0; k < temp.size(); k++) {
        arr[left + k] = temp[k];
    }
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left >= right) return;

    int mid = (left + right) / 2;

    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);

    merge(arr, left, mid, right);
}

int main() {
    vector<int> arr = {38, 27, 43, 3, 9, 82, 10};

    mergeSort(arr, 0, arr.size() - 1);

    for (int num : arr) {
        cout << num << " ";
    }
}
```

#### Java

```java
import java.util.Arrays;

public class Main {

    public static void merge(int[] arr, int left, int mid, int right) {
        int[] temp = new int[right - left + 1];

        int i = left;
        int j = mid + 1;
        int k = 0;

        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j]) {
                temp[k++] = arr[i++];
            } else {
                temp[k++] = arr[j++];
            }
        }

        while (i <= mid) {
            temp[k++] = arr[i++];
        }

        while (j <= right) {
            temp[k++] = arr[j++];
        }

        for (int idx = 0; idx < temp.length; idx++) {
            arr[left + idx] = temp[idx];
        }
    }

    public static void mergeSort(int[] arr, int left, int right) {
        if (left >= right) return;

        int mid = (left + right) / 2;

        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);

        merge(arr, left, mid, right);
    }

    public static void main(String[] args) {
        int[] arr = {38, 27, 43, 3, 9, 82, 10};

        mergeSort(arr, 0, arr.length - 1);

        System.out.println(Arrays.toString(arr));
    }
}
```

### 시간 복잡도

| 최선 | $O(n\log(n))$ |
| ---- | ------------- |
| 평균 | $O(n\log(n))$ |
| 최악 | $O(n\log(n))$ |

### 공간 복잡도

$O(n)$ : 병합 과정에서 임시 배열(temp)을 사용함

### 장점

- 최악의 경우에도 $O(n\log(n))$을 보장한다.
- 안정 정렬(Stable sort)이다.

### 단점

- 정렬을 위한 추가적인 메모리 공간을 필요로 한다.

---
