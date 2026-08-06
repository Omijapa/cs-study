# day27-2 Hash

## Hash
- 데이터를 특정 규칙에 따라 숫자 값으로 변환하여 빠르게 저장하거나 검색하는 방식

### 해시 테이블
- 해시값을 이용해 데이터를 저장하는 자료구조

```python
student = {}

# 데이터 저장
student["name"] = "홍길동"
student["score"] = 90

# 데이터 조회
print(student["name"])   # 홍길동
print(student["score"])  # 90
```

```python
student = {
    "name": "홍길동",
    "score": 90
}

# 수정
student["score"] = 95

# 삭제
del student["name"]

print(student)  # {'score': 95}
```


### 특징
- 키를 이용해 값을 빠르게 검색할 수 있음
- 평균적인 저장•조회•삭제 시간복잡도: `O(1)`
- 서로 다른 키가 같은 해시값을 가지는 경우 -> **해시 충돌(Hash Collision)**
- 파이썬에서는 `dict`와 `set`이 해시를 기반으로 구현됨