# day6-2 GCD(Greatest Common Divisor; 최대공약수) / LCM(Least Common Multiple; 최소공배수)

## 1. 개념
### 1.1. 최대공약수 GCD
- 두 자연수의 공통된 약수 중 가장 큰 수

### 1.2. 최소공배수 LCM
- 두 자연수의 공통된 배수 중 가장 작은 수
- 최소공배수는 최대공약수를 이용해서 구할 수 있음
    - 두 수가 a, b일 때
    LCM(a,b) = a * b // GCD(a,b)
    
    (ex) LCM(72, 30) = 72 * 30 / 6
                     = 2160 / 6
                     = 360

### 1.3 유클리드 호제법(Euclidean Algorithm)
- 두 자연수의 최대공약수를 빠르게 구하는 알고리즘
- a를 b로 나눈 나머지를 r이라고 할 때,
    `GCD(a, b) = GCD(b, r)`
-> 두 수 a,b가 있을 때 `a % b`의 결과를 이용해서 문제의 크기를 계속 줄여나간다.
- 나머지가 0이 될 때까지 이 과정을 반복하고, 나머지가 0이 되었을 때의 나누는 수가 최대공약수가 된다

## 2. 알고리즘 흐름
1. 두 자연수 a,b 준비
2. a % b를 계산하여 나머지 r을 구한다.
3. 나머지 r이 0이면, 이때의 b가 최대공약수이다.
4. 나머지 r이 0이 아니면, a = b, b = r로 바꾼다.
5. 다시 a % b를 계산한다.
6. 나머지가 0이 될 때까지 3-5를 반복한다.
7. 최대공약수 GCD를 구한 뒤, 처음 입력받은 두 수를 이용해 `a * b / GCD`로 최소공배수를 구한다.

### 예시
a = 85, b = 51

85 % 51 = 34
51 % 34 = 17
34 % 17 = 0

나머지가 0이 되었을 때 나누는 수: 17
-> 85와 51의 최대공약수: 17

GCD(85, 51) = 17

## 3. 코드(python)
```python
# 직접 구현
def gcd(a, b):
    while b != 0:
        r = a % b
        a = b
        b = r

    return a

def lcm(a, b):
    return a * b // gcd(a, b)

if __name__ == "__main__":
    a = 72
    b = 30

    print("GCD:",gcd(a, b))
    print("LCM:",lcm(a, b))

'''
결과
GCD: 6
LCM: 360
'''
```
```python
# math 모듈 사용
import math

if __name__ == "__main__":
    a = 72
    b = 30

    print("GCD:", math.gcd(a, b))
    print("LCM:", math.lcm(a, b))

'''
결과
GCD: 6
LCM: 360
'''
```

## 4. 시간복잡도 / 공간복잡도
### 4.1. 시간복잡도
- 유클리드 호제법: 나머지 연산 반복하면서 문제 크기 줄임
시간복잡도: O(logN)

GCD 계산: O(logN)
LCM 계산: O(logN)
LCM은 GCD를 이용해서 계산하므로, 전체 시간복잡도는 GCD 계산에 의해 O(logN)이 된다.

### 4.2 공간복잡도
- a,b,r 정도의 변수만 사용
공간복잡도: O(1)