# Day 7 - LRU Cache

# Cache Replacement Policy

캐싱은 접근성이 더 높은 메모리 공간에 최근에 사용되었거나 자주 사용되는 데이터를 보관하여 성능을 높인다. 이러한 캐시의 공간이 가득차면 알고리즘을 이용해 어떤 데이터를 제거할지 선택해야한다.

교체 방법으로는 다양한 방법들이 존재하는데 전체적인 내용은 다음 [링크](https://en.wikipedia.org/wiki/Cache_replacement_policies#Policies)를 확인하고, 오늘은 그 중에서 LRU에 대해서 확인해보겠다.

## Least Recently Used (LRU)

> Discards least recently used items first.

![](https://upload.wikimedia.org/wikipedia/commons/8/88/Lruexample.png)

age bit를 사용하여 가장 최근에 사용되지 않은 캐시를 추적한다.

예를 들어 좌측 제일 하단과 같이 A, B, C, D가 있는 경우에서 E가 엑세스 되면 MISS가 발생한다. LRU 알고리즘을 사용하면 가장 낮은 순위를 가진 (가장 오래된) A(0)가 E로 대체된다.

만약 이미 캐싱되어 있는 데이터가 다시 엑세스 된다면 (우측에 D가 다시 엑세스 되는 경우) 해당 데이터의 age bit을 업데이터한다.

Java의 경우 두 가지 방법이 존재한다. `LinkedHashMap`을 사용하는 방법과 `HashMap` + `Double LinkedList`로 구현하는 방법이 있다.

### LinkedHashMap 사용

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class LRUCacheWithLinkedHashMap<K, V> {
     private final int capacity;
     private final LinkedHashMap<K, V> cache;

     public LRUCacheWithLinkedHashMap(int capacity) {
         this.capacity = capacity;

         // 두번째 인자인 0.75f는 부하 계수(Load Factor)로 해시 맵의 공간이 얼마나 채워졌을 때 내부 배열의 크기를 키울지 결정하는 기준 비율이다. 0.75f면 데이터가 75%5 채워지는 순간 내부적으로 공간을 약 2배로 늘린다 (Rehashing).
         // 세번째 인자는 접근 순서(Access-order) 모드로, true 시 이미 존재하는 키를 get이나 put 할 경우 해당 엔트리를 리스트의 맨 뒤로 이동시킨다.
         this.cache = new LinkedHashMap<K, V>(capacity, 0.75f, true) {
             @Override
             protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
                 // 캐시 사이즈가 정해진 용량을 초과하면 가장 오래된 데이터(Eldest)를 자동으로 삭제
                 return size() > LRUCacheWithLinkedHashMap.this.capacity;
             }
         };
     }

     public V get(K key) {
         return cache.getOrDefault(key, null);
     }

     public void put(K key, V value) {
         cache.put(key, value);
     }

     public void printCache() {
         System.out.println(cache);
     }

     public static void main(String[] args) {
         LRUCacheWithLinkedHashMap<Integer, String> lru = new LRUCacheWithLinkedHashMap<>(3);

         lru.put(1, "One");
         lru.put(2, "Two");
         lru.put(3, "Three");
         lru.printCache(); // {1=One, 2=Two, 3=Three}

         lru.get(1); // 1번 데이터에 접근! (가장 최신 위치로 이동)
         lru.printCache(); // {2=Two, 3=Three, 1=One} -> 1이 뒤로 감

         lru.put(4, "Four"); // 용량 초과! 가장 오래된 '2'가 삭제됨
         lru.printCache(); // {3=Three, 1=One, 4=Four}
     }
}
```

### HashMap + DoubleLinkedList 사용

```java
import java.util.HashMap;
import java.util.Map;

class ListNode {
    int key;
    int value;
    ListNode prev;
    ListNode next;

    ListNode(int key, int value) {
        this.key = key;
        this.value = value;
    }
}

class DLinkedList {
    private ListNode head;
    private ListNode tail;

    DLinkedList() {
        head = new ListNode(-1, -1);
        tail = new ListNode(-1, -1);

        head.next = tail;
        tail.prev = head;
    }

    public void addFirst(ListNode node) {
        ListNode first = head.next;

        node.next = first;
        node.prev = head;

        head.next = node;
        first.prev = node;
    }

    public void remove(ListNode node) {
        ListNode prevNode = node.prev;
        ListNode nextNode = node.next;

        prevNode.next = nextNode;
        nextNode.prev = prevNode;

        node.prev = null;
        node.next = null;
    }

    public ListNode removeLast() {
        ListNode last = tail.prev;

        if (last == head) {
            return null;
        }

        remove(last);
        return last;
    }
}

class LRUCache {
    private int capacity;
    private Map<Integer, ListNode> cache;
    private DLinkedList cacheQueue;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.cache = new HashMap<>();
        this.cacheQueue = new DLinkedList();
    }

    public int get(int key) {
        if (!cache.containsKey(key)) {
            return -1;
        }

        ListNode node = cache.get(key);

        cacheQueue.remove(node);
        cacheQueue.addFirst(node);

        return node.value;
    }

    public void put(int key, int value) {
        if (cache.containsKey(key)) {
            ListNode node = cache.get(key);

            node.value = value;

            cacheQueue.remove(node);
            cacheQueue.addFirst(node);

            return;
        }

        if (cache.size() >= capacity) {
            ListNode removedNode = cacheQueue.removeLast();
            cache.remove(removedNode.key);
        }

        ListNode newNode = new ListNode(key, value);
        cacheQueue.addFirst(newNode);
        cache.put(key, newNode);
    }
}
```

### 시간 복잡도

$O(1)$

### 공간 복잡도

$O(n)$ (n은 캐시 용량)

---

https://leetcode.com/problems/lru-cache/description/

위 문제 한번 풀어보면 좋을듯!
