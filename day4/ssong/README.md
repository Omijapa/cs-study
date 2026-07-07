# Day 4 - 순열 알고리즘

# 순열(Permutation)이란

순열의 정의는 ‘서로 다른 n개 중 r를 골라 순서를 고려해 나열한 경우의 수’이다.

수학 공식은 다음과 같다.

$$
_n\mathrm{P}_r = \dfrac{n!}{(n-r)!}
$$

예를 들어, 5명 중 3명을 뽑아 순서를 정하는 경우는 다음과 같이 계산된다.

$$
P(5,3)=5×4×3=60
$$

(순열에 대한 수학적 지식은 다음 [링크](https://terms.naver.com/entry.naver?docId=3405187&cid=47324&categoryId=47324)를 참고)

순열을 구하는 방법은 크게 네 가지가 있다.

이 중에서 DFS + visited와 Swap 방식만 제대로 이해해도 코테와 면접에서는 충분하다.

| 알고리즘         | 시간 복잡도 | 추가 공간 | 특징                             |
| ---------------- | ----------- | --------- | -------------------------------- |
| DFS + visited    | O(n × n!)   | O(n)      | 코테에서 가장 많이 사용          |
| Swap             | O(n × n!)   | O(1)      | 메모리 효율 좋음                 |
| next_permutation | O(n × n!)   | O(1)      | 이미 구현된 라이브러리 활용(C++) |
| 비트마스킹 DFS   | O(n × n!)   | O(1)      | visited 배열 대신 비트 사용      |

### 1. DFS + visited 배열

#### 원리

| 변수         | 설명                                         |
| ------------ | -------------------------------------------- |
| n            | 서로 다른 r의 총 개수                        |
| r            | 나열하기 위해 선택하는 개수                  |
| arr 배열     | 최초 n개의 숫자 값을 가지고 있는 배열(n)     |
| result 배열  | 나열된 n개의 숫자 값을 출력하기 위한 배열(n) |
| visited 배열 | 처리한 배열 위치 값에 대해 체크하는 배열(n)  |

예를 들어

```java
arr = [1, 2, 3]
r = 2
```

라면 우리가 원하는 결과는 다음과 같다.

```java
1 2
1 3
2 1
2 3
3 1
3 2
```

여기서 결과 배열을

```java
result = [ , ]
```

라고 하자.

처음 `depth = 0` 일때, 즉 `permutation(0);`의 의미는 `result = [_, _]`에서 0번째 칸을 채우라는 의미이다. 즉 반복문에서 `i = 0`일 때, `result = [1, _]`가 된다.

그 다음 `permutation(1);`을 호출한다.

`depth = 1`이면 `result = [1, _]`에서 1번째 칸을 채우라는 의미이다. 반복문에서 `vistied[0]`은 true이므로 그 다음 값인 `i = 1`이 선택되고, `result = [1, 2]`가 된다.

그 다음 `permutation(2);`를 호출한다.

`depth = 2`이면 이제 `if (depth == r)` 조건이 true가 되어 `1 2`를 출력하고 다시 처음으로 돌아와 visited 배열을 false로 만든다.

다음 반복인 `i = 2`에서는 `result = [1, 3]`이 되어 `1 3`을 출력한다.

depth를 트리의 레벨(level)로 생각하면 가장 이해하기 쉽다.

![](https://github.com/ParkJiwoon/Algorithm/raw/master/Algorithm/image/perm_2.png)

한 단계 내려갈 때마다 원소를 하나씩 더 선택하는 것이다.

#### Java

```java
public class Main {

	static int arr[] = { 1, 2, 3};
	static int result [] = new int[r];
	static boolean[] visited = new boolean[n];
	static int n = 3;  // arr.length로 대체 가능
	static int r = 2;

	static void permutation(int depth) {

		if (depth == r) {
			for (int i = 0; i < r; i++)
				System.out.print(result[i] + " ");

		  System.out.println();
			return;
		}

		for (int i = 0; i < n; i++) {
			if (!visited[i]) {
				visited[i] = true;
				result[depth] = arr[i];
				permutation(depth+1);
				visited[i] = false;
			}
		}
	}

	static void print() {
		for (int i = 0; i < r; i++)
			System.out.print(result[i] + " ");

		System.out.println();
	}

	public static void main(String[] args) {
		permutation(0);
	}
}
```

```java
// 출력
1 2
1 3
2 1
2 3
3 1
3 2
```

#### 장점

- 구현이 직관적
- nPr 구현이 쉬움
- 코테에서 가장 많이 사용됨

#### 단점

- visited 배열 필요
- 메모리 사용량 증가

#### 시간 복잡도

- $O(n \times n!)$

#### 공간 복잡도

- $O(n)$

### 2. Swap

DFS + visited가 선택 여부를 visited 배열로 관리했다면, swap 방식은 다음과 같다.

```java
depth 이전 영역 = 이미 선택 완료
depth 이후 영역 = 아직 선택 안함
```

마찬가지로 예시를 통해 알아보자

```java
arr = [1, 2, 3]
r = 3
```

우선 `depth = 0` 일 때, `i = 0`부터 시작한다. 그럼 `swap(0, 0)`이 실행되는데 이는 첫 번째 자리에 1이 선택되었다는 의미이다. 이후 `permutation(1)`이 호출된다.

`depth = 1`이 시작되기 전 현재 상황은 다음과 같다.

```java
[1,2,3]
   ^
```

첫 번째 자리는 이미 1로 결정되었고 두 번째 자리를 결정해야한다.

`i = 1`부터 시작하므로 `swap(1, 1)`이 실행되고 이는 두 번째 자리에 2를 선택한다는 의미이다.

이후 `permutation(2)`가 호출된다.

`depth = 2`에서는 세 번째 자리를 결정한다. `i = 2`부터 시작하므로 최종적으로 `[1, 2, 3]`이 결정되고 `permutaiton(3)`에서 `if (dpeth == r)` 조건을 만족하여 출력된다.

permutation 이후 실행되는 swap을 통해 원래 순서가 유지된다.

이것도 그림을 통해 보면 이해하기 더 쉽다.

![](https://velog.velcdn.com/images%2Fsoyeon207%2Fpost%2Fabf3b393-cf99-413b-91de-9219adefc173%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.44.52.png)

#### Java

```java
public class Main {

    static int[] arr = {1, 2, 3};
    static int n = arr.length;
    static int r = 2;

    static void permutation(int depth) {

        if (depth == r) {

            for (int i = 0; i < r; i++) {
                System.out.print(arr[i] + " ");
            }
            System.out.println();
            return;
        }

        for (int i = depth; i < n; i++) {

            swap(depth, i);

            permutation(depth + 1);

            swap(depth, i);
        }
    }

    static void swap(int a, int b) {
        int temp = arr[a];
        arr[a] = arr[b];
        arr[b] = temp;
    }

    public static void main(String[] args) {
        permutation(0);
    }
}
```

```java
// 출력
1 2
1 3
2 1
2 3
3 2
3 1
```

#### 장점

- visited 배열이 필요 없음
- 추가 메모리 사용이 거의 없음
- 실제로 가장 효율적인 순열 중 하나임

#### 단점

- nPr 구현이 DFS보다 어려움

#### 시간 복잡도

- $O(n \times n!)$

#### 공간 복잡도

- $O(1)$ (재귀 스택 제외)
