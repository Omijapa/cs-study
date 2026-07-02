# Day 3 - 투포인터 알고리즘

# 투포인터 알고리즘

투포인터는 슬라이딩 윈도우와 유사한 알고리즘이다. 슬라이딩 윈도우는 정해진 범위가 주어지지만, 투포인터는 범위가 정해지지 않는다는 차이점이 있다. (슬라이딩 윈도우에 대한 개념은 하단에 있음)

알고리즘의 동작 방식은 다음과 같다.

1. 범위의 시작과 끝을 가리키는 start, end 포인터 변수를 0으로 지정한다.
2. 특정한 값 X가 되기 전까지 end 범위를 키워가면서 더해준다.
3. end 포인터까지의 합계가 X보다 커지게 되면 start 범위를 키워 앞의 숫자를 빼준다.

   (항상 start ≤ end를 만족해야 한다.)

4. 범위의 합계가 X가 되는 부분의 개수를 구한다.

정리하자면, start와 end를 증가시키면서 부분 배열의 합이 정확히 X가 되는 경우를 세는 것이다.

#### C++

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> arr = {1, 2, 3, 2, 5};
    int M = 5;

    int left = 0;
    int right = 0;
    int sum = 0;
    int count = 0;

    while (true) {
        if (sum >= M) {
            sum -= arr[left++];
        } else if (right == arr.size()) {
            break;
        } else {
            sum += arr[right++];
        }

        if (sum == M) {
            count++;
        }
    }

    cout << count << endl;
}
```

#### Java

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 2, 5};
        int M = 5;

        int left = 0;
        int right = 0;
        int sum = 0;
        int count = 0;

        while (true) {
            if (sum >= M) {
                sum -= arr[left++];
            } else if (right == arr.length) {
                break;
            } else {
                sum += arr[right++];
            }

            if (sum == M) {
                count++;
            }
        }

        System.out.println(count);
    }
}
```

### 시간 복잡도

$O(n)$

### 공간 복잡도

$O(1)$

### 장점

- 이중 반복문 구조를 $O(n)$ 수준으로 줄일 수 있다.
- 추가적인 메모리 사용이 없다.

### 단점

- 적용할 수 있는 문제가 제한적이다. (정렬된 배열, 연속된 구간에서만 효과적이다)

---

# 슬라이딩 윈도우 알고리즘

슬라이딩 윈도우란 특정 구역의 합계나 평균을 구하는데 유용한 방식이다. 예시와 함께 알아보자.

천지삐까리에서 일별로 새로 오미자가 팔리는 횟수를 다음과 같이 기록해두었다. 이제 5일 동안 가장 많이 팔린 구간에서 몇 개를 팔았는지 알아보자.

![](https://static.wikidocs.net/images/page/206308/Screenshot_at_Jul_08_23-48-31.png)

가장 단순하게 접근하면 두 번의 반복문을 통해 첫 번째 날부터 5번째 되는 날까지의 합계를 구하고, 두 번째 날부터 6번째 날까지의 합계를 구하면 된다. 하지만 시간이 오래걸린다. 이런 경우에 슬라이딩 윈도우가 유용하게 쓰인다.

![5 + 3 + 2 + 5 + 7 = 22](https://static.wikidocs.net/images/page/206308/Screenshot_at_Jul_08_23-49-05.png)

5 + 3 + 2 + 5 + 7 = 22

우선 첫 번째 5일치는 정상적으로 계산해서 구한다. 다음으로 넘어갈 때부터는 이제 아래와 같이 계산한다.

![](https://static.wikidocs.net/images/page/206308/Screenshot_at_Jul_08_23-49-34.png)

앞에서 구한 22에서 윈도우에서 벗어나는 5는 빼주고, 새로 윈도우에 들어오는 1은 더해준다. 따라서 22 - 5 + 1 = 18이 된다.

![](https://static.wikidocs.net/images/page/206308/Screenshot_at_Jul_08_23-50-19.png)

다음 계산 역시 앞에서 구한 18에서 3을 빼주고, 4를 더해준다. 이런 방식으로 전체를 계산하면 특정 부분에 대한 최대값을 쉽게 계산할 수 있다.

#### C++

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> arr = {1, 2, 3, 4, 5};
    int K = 3;

    int windowSum = 0;

    for (int i = 0; i < K; i++) {
        windowSum += arr[i];
    }

    int maxSum = windowSum;

    for (int i = K; i < arr.size(); i++) {
        windowSum += arr[i];
        windowSum -= arr[i - K];

        maxSum = max(maxSum, windowSum);
    }

    cout << maxSum << endl;
}
```

#### Java

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};
        int K = 3;

        int windowSum = 0;

        for (int i = 0; i < K; i++) {
            windowSum += arr[i];
        }

        int maxSum = windowSum;

        for (int i = K; i < arr.length; i++) {
            windowSum += arr[i];
            windowSum -= arr[i - K];

            maxSum = Math.max(maxSum, windowSum);
        }

        System.out.println(maxSum);
    }
}
```

### 시간 복잡도

$O(n)$

### 공간 복잡도

$O(1)$

### 장점

- 구현이 단순하고 고정 길이 구간 문제에 최적이다.
- 추가적인 메모리 사용이 없다.

### 단점

- 적용할 수 있는 문제가 제한적이다. (정렬된 배열, 고정 길이 구간에서만 효과적이다)

---

코테를 위해서는 아래 템플릿만 알아둬도 풀 수 있을 정도로 구현이 단순하다.

#### 투포인터

```java
int left = 0;
int right = 0;

while (right < n) {
    // 조건 만족할 때까지 right 증가

    while (조건 만족) {
        // left 증가
    }

    right++;
}
```

#### 슬라이딩 윈도우

```java
for (int i = K; i < n; i++) {
    windowSum += arr[i];
    windowSum -= arr[i - K];
}
```
