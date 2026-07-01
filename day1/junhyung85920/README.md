# Selection Sort

Date: 2026년 7월 1일
Status: Done

## 개념

<aside>
📜

정렬되지 않은 데이터들 중 가장 작은 데이터를 찾아 가장 앞의 데이터와 교환함으로써 전체 데이터를 정렬하는 방식

→ 가장 왼쪽부터 차례대로 위치를 정해놓고 우측에 있는 정렬되지 않은 데이터들 중 가장 작은 값을 찾아 해당 위치로 옮긴다.

</aside>

---

## 과정

1. 정렬되지 않은 값들 중 최소값 찾기
2. 정렬되지 않은 값들 중 맨 앞에 위치한 값과 교체
3. 정렬되지 않은 나머지 값들에 대해서 1과 2를 반복

![image.png](Selection%20Sort/image.png)


---

## 구현

### Python

```python
def selection_sort(arr):
    for i in range(0, len(arr) - 1):
        min_idx = i
        for j in range(i+1, len(arr)):
            if arr[j] < arr[min_idx]:
                min_idx = j

        if min_idx != i:
            arr[i], arr[min_idx] = arr[min_idx], arr[i]

if __name__ == '__main__':
    test_case = [3,9,6,1,5,2,0]
    print("정렬 전: ", test_case)
    selection_sort(test_case)
    print("정렬 후: ", test_case)
```

### Java

```java
import java.util.Arrays;

public class SelectionSort {

    public static void selectionSort(int[] arr) {
        int len = arr.length;

        for (int i = 0; i < len - 1; i++) {
            int min_idx = i; // 현재 탐색에서 가장 작은 원소의 인덱스

            for (int j = i + 1; j < len; j++) {
                if (arr[j] < arr[min_idx]) {
                    min_idx = j; // 최솟값의 위치만 기억
                }
            }

            // 안쪽 루프가 끝난 후, 원래 i 자리에 있던 값과 최솟값을 Swap
            if (min_idx != i) {
                int tmp = arr[min_idx];
                arr[min_idx] = arr[i];
                arr[i] = tmp;
            }
        }
    }

    // 실행을 위한 main 메서드
    public static void main(String[] args) {
        int[] arr = {64, 25, 12, 22, 11};

        System.out.println("정렬 전: " + Arrays.toString(arr));
        selectionSort(arr);
        System.out.println("정렬 후: " + Arrays.toString(arr));
    }
}
```

---

## 시간복잡도

이중 반복문을 통해 (n-1) * (n-(i+1)) 만큼 반복하므로 O(n^2)

---
---

# Bubble Sort

Status: Done

## 개념

<aside>
📜

가장 왼쪽부터 인접한 두 원소를 비교하며 오름차순으로 정렬하는 방식

</aside>

---

## 과정

1. 인접한 두 원소를 비교하며 큰 값을 우측에 옮기는 동작을 반복
2. 1을 반복하면 배열 우측부터 정렬된 원소가 위치하고 배열의 길이만큼 반복 시 모든 원소가 정렬됨

![Screenshot 2026-06-30 at 1.57.08 PM.png](Bubble%20Sort/Screenshot_2026-06-30_at_1.57.08_PM.png)


---

## 구현

### Python

```python
def bubble_sort(arr):
    for i in range(len(arr)):
        for j in range(len(arr)-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]

if __name__ == '__main__':
    test_case = [3,9,6,1,5,2,0]
    print("정렬 전: ", test_case)
    bubble_sort(test_case)
    print("정렬 후: ", test_case)
```

### Java

```java
import java.util.Arrays;

public class BubbleSort {
    public static void bubbleSort(int[] arr){
        int len = arr.length;

        for (int i = 0; i < len; i++) {
            for (int j = 0; j < len - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    int tmp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = tmp;
                }
            }
        }
    }

    // 실행을 위한 main 메서드
    public static void main(String[] args) {
        int[] arr = {64, 25, 12, 22, 11};

        System.out.println("정렬 전: " + Arrays.toString(arr));
        bubbleSort(arr);
        System.out.println("정렬 후: " + Arrays.toString(arr));
    }
}

```

---

## 시간복잡도

O(n^2)

---
---

# Merge Sort

Status: Done

## 개념

<aside>
📜

배열을 두 개의 균등한 크기로 분할하고, 분할한 각 배열들을 정렬 후, 다시 합치면서 정렬하는 방식 (Divide & Conquer)

</aside>

---

## 과정

1. 입력 배열을 같은 크기의 2개의 부분 배열로 분할
2. 부분 배열을 정렬하고, 부분 배열의 크기가 충분히 작지 않다면 그 배열에 대해서 1과 2를 반복
3. 정렬된 부분 배열들을 하나의 배열에 합침

![Screenshot 2026-06-30 at 2.44.49 PM.png](Merge%20Sort/Screenshot_2026-06-30_at_2.44.49_PM.png)


![Screenshot 2026-06-30 at 2.45.05 PM.png](Merge%20Sort/Screenshot_2026-06-30_at_2.45.05_PM.png)


---

## 구현

### Python

```python

def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    left = merge_sort(list(arr[:mid]))
    right = merge_sort(list(arr[mid:]))

    return merge(left, right)

def merge(left, right):
    result = []
    l_idx, r_idx = 0, 0

    while l_idx < len(left) or r_idx < len(right):
        if l_idx < len(left) and r_idx < len(right):
            if left[l_idx] < right[r_idx]:
                result.append(left[l_idx])
                l_idx += 1
            else:
                result.append(right[r_idx])
                r_idx += 1
        elif l_idx < len(left):
            while l_idx < len(left):
                result.append(left[l_idx])
                l_idx += 1
        elif r_idx < len(right):
            while r_idx < len(right):
                result.append(right[r_idx])
                r_idx += 1

    return result

if __name__ == '__main__':
    test_case = [3,9,6,1,5,2,0]
    print("정렬 전: ", test_case)
    sorted_arr = merge_sort(test_case)
    print("정렬 후: ", sorted_arr)
```

### Java

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class MergeSort {

    public static List<Integer> mergeSort(List<Integer> arr) {
        if (arr.size() <= 1) {
            return arr;
        }

        int mid = arr.size() / 2;

        List<Integer> left = mergeSort(new ArrayList<>(arr.subList(0, mid)));
        List<Integer> right = mergeSort(new ArrayList<>(arr.subList(mid, arr.size())));

        return merge(left, right);
    }

    public static List<Integer> merge(List<Integer> left, List<Integer> right) {
        List<Integer> result = new ArrayList<>();
        int lIdx = 0, rIdx = 0;

        while (lIdx < left.size() || rIdx < right.size()) {
            if (lIdx < left.size() && rIdx < right.size()) {
                if (left.get(lIdx) < right.get(rIdx)) {
                    result.add(left.get(lIdx));
                    lIdx++;
                } else {
                    result.add(right.get(rIdx));
                    rIdx++;
                }
            } else if (lIdx < left.size()) {
                while (lIdx < left.size()) {
                    result.add(left.get(lIdx));
                    lIdx++;
                }
            } else if (rIdx < right.size()) {
                while (rIdx < right.size()) {
                    result.add(right.get(rIdx));
                    rIdx++;
                }
            }
        }

        return result;
    }

    public static void main(String[] args) {
        List<Integer> testCase = new ArrayList<>(Arrays.asList(64, 25, 12, 22, 11));

        System.out.println("정렬 전: " + testCase);
        List<Integer> sortedArr = mergeSort(testCase);
        System.out.println("정렬 후: " + sortedArr);
    }
}
```

---

## 시간복잡도

O(nlogn)

---
---

# Insertion Sort

Status: Done

## 개념

<aside>
📜

자료 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 방식

</aside>

---

## 과정

1. 입력 배열의 첫 번째 부분은 정렬되어 있다고 가정
2. 두 번째 원소부터 정렬된 앞쪽 원소들과 비교해서 작으면 왼쪽으로 한 칸씩 이동하고, 크면 멈춤
3. 정렬되지 않은 나머지 원소들에 대해서 2를 반복

![Screenshot 2026-07-01 at 1.12.33 PM.png](Insertion%20Sort/Screenshot_2026-07-01_at_1.12.33_PM.png)


---

## 구현

### Python

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        for j in range(i, 0, -1):
            print(j)
            if arr[j] < arr[j - 1]:
                arr[j], arr[j - 1] = arr[j - 1], arr[j]
            else:
                break

if __name__ == '__main__':
    test_case = [3,9,6,1,5,2,0]
    print("정렬 전: ", test_case)
    insertion_sort(test_case)
    print("정렬 후: ", test_case)

```

### Java

```java
import java.util.Arrays;

public class InsertionSort {

    private static void insertionSort(int[] arr) {
        for (int i = 1; i < arr.length; i++) {
            for (int j = i; j > 0; j--) {
                if (arr[j] < arr[j - 1]) {
                    int tmp = arr[j];
                    arr[j] = arr[j - 1];
                    arr[j - 1] = tmp;
                }
            }
        }
    }

    public static void main(String[] args) {
        int[] arr = {64, 25, 12, 22, 11};

        System.out.println("정렬 전: " + Arrays.toString(arr));
        insertionSort(arr);
        System.out.println("정렬 후: " + Arrays.toString(arr));
    }
}

```

---

## 시간복잡도

보통 O(n^2)이지만, 최적의 경우 즉, 오름차순으로 정렬되어 있는 경우 O(n)도 가능하다.
