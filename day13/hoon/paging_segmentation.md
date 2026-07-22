# day 13-2 Paging / Segmentation

## 1. Paging / Segmentation
- 운영체제가 메모리를 관리(Memroy Management)하는 방법

```text
어떠한 프로그램을 실행할 때, 컴퓨터에서는 프로그램들을 메모리 공간에 연속적으로 할당하게 됩니다. 만약 여러 프로그램들이 메모리에 할당되고 해제되는 것이 반복되다 보면 메모리 공간이 조각조각 나뉘게 되어 총 메모리가 충분함에도 불구하고 프로그램에 메모리를 할당하는 것이 불가능한 상태가 발생하게 됩니다. 이러한 현상을 바로 메모리 단편화라고 하며, 페이징과 세그멘테이션은 이러한 메모리 단편화의 해결방법입니다.
```
[출처](https://poca.tistory.com/entry/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-Paging%EA%B3%BC-Segmentation%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-%EC%84%A4%EB%AA%85%ED%95%B4-%EB%B3%B4%EC%84%B8%EC%9A%94)

## 2. Memory Management
- 메모리 관리에서는 아래와 같은 관점으로 효율성이 있는지 확인
1. `Utilization`: 물리 메모리를 얼마나 아껴쓰는가?
- Fragmentation: 단편화
2. `Performance`: 메모리 접근 속도가 얼마나 빠른가?
- Address translation (logical to physical), swapping: 주소 변환(논리적 주소에서 물리적 주소로), 스와핑

## 3. Management schemes: 메모리 관리 체계
1. Contigouous allocation
2. Segmantation
3. Paging
4. Demand paging(state-of-the-art)

### 3.1. Contiguous allocation: 연속 할당
Logical Address Space를 있는 그대로 Pysical Address에 처리 없이 그대로 붙인 기법

1. Utilization:
Fragmentation 영역의 활용이 제한적 -> Utilization를 100% 활용하기 어려운 기법
내부에서 Stack, heap의 활용도 차이로 인해 Utilization이 낮아질 수 있음
>>> 심각한 문제 가능

2. Performance: Address 변환이 쉬워 큰 단점이 없음

### 3.2. Segmentation
Logical Address Space를 조각 내서 Pysical Address에 할당하는 기법.
조각의 크기가 프로그래머 의존적 & 임의적임

1. Utilization:
    1) External Fragmentation: 임의적인 조각 크기로 인해 완화가 될 수 있으나 프로그래머에 의존적이다 보니 완화 정도가 명확하지 않다. 완전한 해소 불가능
    2) Internal Fragmentation: 동일하거나 더 심해짐.(Segmentation의 차이가 크면 더욱 심화)
    - 기법만 확인하였을 떄 Utilization이 더 좋아질만 하지만 실제 적용 시에는 오히려 더 나빠질 수 있음

2. Performance: 연속할당 대비 Address translation의 경우 2배 나아짐. Swapping은 조금 나아짐

### 3.3. Paging
Segmentation과 비슷, 그러나 Fixed Size 사용
약속된 사이즈로 정하고 그 만큼 조각내는 기법
프로그래머는 전혀 신경쓰지 않는다.

1. Utilization:
    1) External Fragmentaion: 완전 해소
    2) Internal Fragmentation: 한 프로세스 내에서 최대 발생 = 1frame - 1byte로 제한됨

2. Performance: Address translation의 경우 Segmentation 기법과 동일하며, Swapping의 경우 효율이 증대

## 참고
[자료](https://cocoon1787.tistory.com/860)