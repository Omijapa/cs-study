# LRU Cache

Date: 2026년 7월 9일
Status: Done

# 개념

<aside>
📜

### Least Recently Used + Cache

→ 즉, 가장 오랫동안 사용되지 않은 데이터를 캐시에서 가장 먼저 제거하는 전략 (언제나 개념은 쉽다)

</aside>

# 시뮬레이션

```bash
캐시의 용량(Capacity or Max size)이 3일 때, [1, 2, 3, 1, 4] 순서로 데이터 요청이 들어온다고 하자.

[1] 요청: Cache Miss -> Cache에 [1] 추가, [1]
[2] 요청: Cache Miss -> Cache에 [2] 추가, [1, 2]
[3] 요청: Cache Miss -> Cache에 [3] 추가, [1, 2, 3] FULL
[1] 요청: Cache Hit  -> 1의 우선 순위를 최신으로 갱신, [2, 3, 1] FULL
[4] 요청: Cache Miss -> 가장 오랫동안 사용되지 않은 [2] 제거, Cache에 [4] 추가, [3, 1, 4] FULL
```

---

# 구현

### Python (OrderedDict는 해시 맵에 이중 연결 리스트가 내장되어 있음 → 순서 보장)

```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.cache = OrderedDict()
        self.capacity = capacity

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        # 조회가 발생했으므로 최근 사용된 것으로 간주하여 맨 뒤(오른쪽)로 이동
        self.cache.move_to_end(key)
        return self.cache[key]

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        
        # 용량을 초과하면 가장 오랫동안 안 쓴 맨 앞(왼쪽) 데이터 삭제
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False) # last=False가 맨 앞 제거
```

### Python (@lru_cache 사용 예제, @cache에서 불가능한 메모리 제한이 가능!)
→ 직접 구현보단 이거 쓰는게 빠르고 메모리 효율 좋음

```python
from functools import lru_cache  

def fetch_user(user_id):
    print(f"DB에서 아이디가 {user_id}인 사용자 정보를 읽어오고 있습니다...")
    return {
        "userId": user_id,
        "email": f"{user_id}@test.com",
        "password": "test1234"
    }

# 데코레이터가 붙은 메소드의 parameter를 key로 return 값을 value로 하여 LRU 캐싱이 적용됨
# lru_cache(max_size=2) -> 이렇게 capacity 조절 가능
@lru_cache
def get_user(user_id):
    return fetch_user(user_id)
    
if __name__ == "__main__":
    from random import choice

    for _ in range(10):
        get_user(user_id = choice(["A01", "B02", "C03"])) # 랜덤으로 user_id 선택

    print(get_user.cache_info()) # CacheInfo(hits=7, misses=3, maxsize=128, currsize=3)
```

### Java (LinkedHashMap 사용 → Thread-Safe가 아니라서 주의해야함)

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class LruCache<K, V> extends LinkedHashMap<K, V> {
    private final int maxCapacity;

    public LruCache(int maxCapacity) {
        // 1. 초기용량, 2. 부하율, 3. accessOrder를 true로 설정
        super(maxCapacity, 0.75f, true);
        this.maxCapacity = maxCapacity;
    }

    // 데이터가 추가된 후, 가장 오래된 녀석을 지울지 결정하는 자바 내장 메서드
    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        // 사이즈가 지정한 용량을 넘어가면 참(true)을 반환해 가장 오래된 데이터를 자동 삭제함
        return size() > maxCapacity;
    }
}
```

### ** Java의 고성능 인메모리 캐싱 라이브러리인 Caffeine이라는 것도 있음

---

# 시간복잡도

해시 맵과 이중 연결 리스트의 조합으로 구현한다면 조회, 갱신, 삭제 모두 O(1)

배열 또는 연결 리스트 기반으로 구현한다면 O(n)