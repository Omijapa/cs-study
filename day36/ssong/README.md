# Day 36 - LIS

## Longest Increasing Subsequences(LIS)

LIS는 DP문제로, 원소가 N개인 배열에서 일부 원소들을 골라내어 만든 부분 수열 중에서 이전 원소보다 크면서 그 길이가 최대인 부분 수열을 찾는 문제이다.

예를 들어 {10, 20, 40, 25, 20, 50, 30, 70, 85} 라는 input이 있다

여기서 LIS를 찾으면 {10, 20, 40, 50, 70, 85}가 된다

(증가하는 순서대로 숫자를 고르면서, 고른 부분 수열의 길이가 최대 길이가 되는 경우의 길이)

### 이중 반복문으로 구현

- 시간 복잡도 : O(N)

```java
// array dp : x번째 수를 마지막 원소로 가지는 lis의 길이
for (int i = 0; i < n; i++) {
	if (dp[i] == 0) dp[i] = 1;
	for (int j = 0; j < i; j++) {
		if (arr[i] > arr[j]) {
			if (dp[i] < dp[j] + 1) {
				dp[i] = dp[j] + 1;
			}
		}
	}
}
```

### 이분 탐색으로 구현

- 시간 복잡도 : O(NlogN)
- 구현에 대한 설명은 [블로그](https://jason9319.tistory.com/113) 참고

```java
static int lis(int[] arr) {
	int[] lis = new int[arr.length];
	int size = 0;


	for (int num: arr) {
		int left = 0;
		int right = size;

		// Lower Bound 찾기
		while (left < right) {
			int mid = (left + right) / 2;

			if (lis[mid] >= num) {
				right = mid;
			}
			else {
				left = mid + 1;
			}
		}

		lis[left] = num;

		if (left == size) {
			size++;
		}
	}

	return size;
}

static void main(String[] args) {
	int[] arr = {10, 20, 10, 30, 20, 50};

	System.out.println(lis(arr)); // 4
}
```
