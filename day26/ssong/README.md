# Day 26 - Array vs ArrayList vs LinkedList

## Array

- 생성될 때 크기가 결정되고 이후 변경할 수 없다
- int, char 같은 primitive와 object 모두 저장할 수 있다
- index를 통해 $O(1)$에 접근이 가능하다
- 배열의 길이는 length property를 통해 알 수 있다
- 중간에 삽입/삭제 시 원소의 이동이 필요해 $O(n)$의 시간이 필요하다

## ArrayList

- 내부적으로 배열을 사용하는 List 구현체이다
- Array와 달리 크기를 동적으로 조절한다
- 오직 객체만 저장하고 원시 값의 경우 Integer, Double, Character 등의 Wrapper 클래스로 저장한다
- add(), remove(), get(), contains() 등 많은 built-in 메소드들이 있다
- 배열과 마찬가지로 인덱스를 가지고 있어 검색이 빠르다 ($O(1)$)
- 중간에 삽입/삭제 시 원소의 이동이 필요해 $O(n)$의 시간이 필요하다

## LinkedList

- 이중 연결 리스트로 구현되어 있다
- “노드의 위치를 알고 있다면” 삽입/삭제가 빠르지만 ($O(1)$) 접근 연산의 경우 순차 탐색이 필요해 매우 느리다 ($O(n)$)
- 각 노드에 이전/다음 노드에 대한 참조를 추가적으로 저장해 ArrayList 보다 메모리 오버헤드가 크다
