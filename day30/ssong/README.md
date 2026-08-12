# Day 30 - 이진 탐색 트리, Trie 자료 구조

# 이진 탐색 트리

- 노드의 왼쪽 하위 트리에는 노드의 키 보다 작은 키가 있는 노드만 있음
- 노드의 오른쪽 하위 트리에는 노드의 키 보다 큰 키가 있는 노드만 있음
- 모든 하위 트리들 또한 이진 탐색 트리임
- 중복된 키를 허용하지 않음
- in-order 순회를 통해 정렬된 순서대로 키 값을 뽑을 수 있음
- 탐색 시간 복잡도는 균형 상태인 경우 O(log N), 불균형 상태라면 O(N)이 걸림

### 삽입 과정

1. 루트 노드부터 시작해 노드의 키 값보다 작으면 왼쪽으로, 크다면 오른쪽으로 재귀적으로 반복함
2. 리프 노드에 도달 후 노드보다 작으면 왼쪽, 크다면 오른쪽에 삽입함

```java
Class Node {
	int value;
	Node left;
	Node right;

	Node(int value) {
		this.value = value;
	}
}

public class BinarySearchTree {
	Node root;

	// Insert
	public void insert(int value) {
		root = insertNode(root, value);
	}

	private Node insertNode(Node node, int value) {
		if (node == null) return new Node(value):

		if (value < node.value) {
			node.left = insertNode(node.left, value);
		}
		else {
			node.right = insertNode(node.right, value);
		}

		return Node;
	}
}
```

### 삭제 과정

#### 1. 삭제할 노드가 리프 노드인 경우

그냥 삭제하면 됨

#### 2. 삭제할 노드에 자식이 하나만 있는 경우

해당 노드를 삭제한 후 자식 노드를 삭제된 위치로 옮기면 됨

#### 3. 삭제할 노드에 자식이 둘인 경우

이 경우 삭제될 위치에 들어갈 노드를 찾아줘야 함

정렬 순서를 고려해야하므로 오른쪽 서브트리에서 최소 값을 가진 노드를 찾아 바꿔줘야함

```java
public class BinarySearchTree {
	// Insert

	// Delete
	public void delete(int value) {
		root = deleteNode(root, value);
	}

	private Node deleteNode(Node node, int value) {
		if (node == null) return null;

		if (value < node.value) {
			node.left = deleteNode(node.left, value);
		}
		else if (value > node.right) {
			node.right = deleteNode(node.right, value);
		}
		else {
			// 1. 삭제할 노드가 리프노드인 경우
			if (node.left == null && node.right == null) {
				return null;
			}

			// 2. 삭제할 노드에 자식이 하나만 있는 경우
			if (node.left == null) {
				return node.right;
			}
			if (node.right == null) {
				return node.left;
			}

			// 3. 삭제할 노드에 자식이 둘인 경우
			Node successor = findMin(node.right);
			node.value = successor.value;

			node.right = deleteNode(node.right, successor.value);
		}

		return node;
	}

	private Node findMin(Node node) {
		while (node.left != null) {
			node = node.left;
		}
		return node;
	}
}
```

# Trie

트라이는 문자열을 효율적으로 탐색하기 위한 트리 형태의 자료구조이다.

e.g. 자동완성 기능, 사전 검색 등

![](https://twpower.github.io/images/20190804_186/trie-example-base.png)

- 시간 복잡도 측면에서는 문자열을 하나하나 비교하며 탐색하는 것에 비해 빠르지만, 자식 포인터를 저장하기 위한 공간이 따로 필요하기에 메모리 측면에서는 비효율적일 수 있다
- 문자열의 길이를 L이라고 할 때, 삽입/검색/삭제 모두 O(L)의 시간복잡도를 가진다
