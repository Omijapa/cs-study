# Day 6 - 이분탐색, 최대공약수/최소공배수

# 이분탐색

이분탐색은 검색 알고리즘으로 오름차순 혹은 내림차순으로 정렬된 공간에서 작동한다. 목표 값을 찾기 위해 반복적으로 공간을 1/2로 나누어 $O(\log n)$ 시간 안에 정답을 찾는다.

![](https://media.geeksforgeeks.org/wp-content/uploads/20260314112909421169/frame_1.webp)

알고리즘 동작 과정은 다음과 같다.

1. 가운데 인덱스인 mid를 기준으로 두 개의 공간으로 분리한다.
2. mid에 해당하는 값(middle)이 key(찾고자 하는 값)와 일치하는지 확인한다.
3. 만약 찾지 못했다면, 다음 검색 공간을 선택한다.
   1. 만약 key 값이 middle 값보다 크다면 오른쪽 영역을
   2. 만약 key 값이 middle 값보다 작다면 왼쪽 영역을 선택한다.
4. 위 과정을 key 값을 찾거나 모든 검색 공간을 탐색할 때까지 반복한다.

#### 반복문을 이용한 구현

```java
class Main {
	static int binarySearch(int arr[], int x) {
		int low = 0, high = arr.length - 1;
		while (low <= high) {
			int mid = low + (high - low) / 2;

			if (arr[mid] == x)
				return mid;

			if (arr[mid] < x)
				low = mid + 1;
			else
				high = mid - 1;
		}

		return -1;
	}

	public static void main(String args[]) {
		int arr[] = { 2, 3, 4, 10, 40};
		int x = 10;
		int result = binarySearch(arr, x);
		if (result == -1)
			System.out.println("Element is not present in array");
		else
			System.out.println("Element is present at " + result);
	}
}
```

### 시간 복잡도

| 최선 | $O(1)$      |
| ---- | ----------- |
| 평균 | $O(\log n)$ |
| 최악 | $O(\log n)$ |

### 공간 복잡도

$O(1)$

### 장점

- 추가 메모리 사용이 거의 없다.
- 함수 호출 오버헤드가 없다.
- 배열 크기가 매우 커도 스택 오버플로우 위험이 없다.

### 단점

- 재귀 방식보다 덜 직관적이다.
- 탐색 범위를 직접 관리해야 한다.

#### 재귀문을 이용한 구현

```java
class Main {
	static int binarySearch(int arr[], int low, int high, int x) {
		if (low <= high) {
			int mid = (low + high) / 2;

			if (arr[mid] == x)
				return mid;

			if (arr[mid] < x)
				return binarySearch(arr, mid + 1, high, x);
			else
				return binarySearch(arr, low, mid - 1, x);
		}
		return -1;
	}

	public static void main(String args[]) {
		int arr[] = { 2, 3, 4, 10, 40};
		int x = 10;
		int result = binarySearch(arr, 0, arr.length - 1, x);
		if (result == -1)
			System.out.println("Element is not present in array");
		else
			System.out.println("Element is present at " + result);
	}
}
```

### 시간 복잡도

| 최선 | $O(1)$      |
| ---- | ----------- |
| 평균 | $O(\log n)$ |
| 최악 | $O(\log n)$ |

### 공간 복잡도

$O(\log n)$ : 재귀 호출 스택

### 장점

- 코드를 분할 정복 개념으로 접근하면 이해하기 쉽다.

### 단점

- 함수 호출 스택이 사용되어 추가적인 메모리 공간이 필요하다.
- 입력이 매우 크면 스택 오버플로우가 발생할 수 있다.
- 반복문에 비해 성능이 약간 떨어진다.

---

# 최대공약수(GCD)와 최소공배수(LCM)

최대공약수(Greatest Common Divisor)와 최소공배수(Least Common Multiple)의 개념은 생략한다.

면접이나 코테에서는 주로 유클리드 호제법(Euclidean Algorithm)을 사용한다는 점 기억하자.

## 브루투포스(완전탐색)

### GCD

`min(a, b)`부터 내려가면서 공약수를 찾는다.

```java
public static int gcd(int a, int b) {
	int min = Math.min(a, b);

	for (int i = min; i >= 1; i--) {
		if (a % i == 0 && b % i == 0) return i;
	}
}
```

### LCM

$$
LCM = {{a \times b} \over GCD(a, b)}
$$

LCM은 위 공식을 활용하여 계산한다.

```java
public static int lcm(int a, int b) {
	return a * b / gcd(a, b);
}
```

또는 직접 탐색할 수도 있다.

```java
public static int lcm(int a, int b) {
	int max = Math.max(a, b);

	while (true) {
		if (max % a == 0 && max % b == 0) {
			return max;
		}
		max++;
	}
}
```

### 시간 복잡도

#### GCD : $O(min(a, b))$

#### LCM : 공식 사용 시 $O(min(a, b))$

### 공간 복잡도

$O(1)$

### 장점

- 이해하기 쉽고 구현이 간단하다.

### 단점

- 많이 느리다.

## 유클리드 호제법 (반복문)

원리는 다음과 같다. $gcd(a, b) = gcd(b, a \% b)$라는 성질을 사용한다.

증명은 다음 [링크](https://ko.wikipedia.org/wiki/%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C_%ED%98%B8%EC%A0%9C%EB%B2%95) 참고

### GCD

```java
public static int gcd(int a, int b) {
	while (b != 0) {
		int temp = b;
		b = a % b;
		a = temp;
	}
	return a;
}
```

### LCM

```java
public static int lcm(int a, int b) {
	return a / gcd(a, b) * b;  // 오버플로우 방지를 위해 계산 순서를 바꾼 것임
}
```

### 시간 복잡도

#### GCD : $O(\log(min(a, b)))$

#### LCM : 공식 사용 시 $O(\log(min(a, b)))$

### 공간 복잡도

$O(1)$

### 장점

- 매우 빠르고 구현도 간단하다.

### 단점

- 알고리즘이 직관적이지 않다.

## 유클리드 호제법 (재귀문)

### GCD

```java
public static int gcd(int a, int b) {
	if (b == 0) return a;
	return gcd(b, a % b);
}
```

### LCM

```java
public static int lcm(int a, int b) {
	return a / gcd(a, b) * b;  // 오버플로우 방지를 위해 계산 순서를 바꾼 것임
}
```

### 시간 복잡도

#### GCD : $O(\log(min(a, b)))$

#### LCM : 공식 사용 시 $O(\log(min(a, b)))$

### 공간 복잡도

$O(\log(min(a, b)))$ : 재귀 호출 스택

### 장점

- 매우 빠르고 구현도 간단하다.

### 단점

- 반복문보다 약간 비효율적이다.

이외에도 소인수분해나 Binary GCD 기법도 존재하지만 CS나 코테에서는 이정도면 충분할 듯
