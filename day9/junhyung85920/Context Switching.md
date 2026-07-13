# Context Switching

Status: Done

# 개념

<aside>
📜

**Context란?**

CPU가 해당 프로그램을 실행하기 위해 기억해야 하는 핵심 정보들의 총 집합이라고 할 수 있지.
→ 포켓몬스터 할 때 “리포트 파일 작성” 고런 느낌

- 범용 Register
- 상태 Register
- Stack Pointer
- PC
- Process ID / Thread ID
- State(Running, Ready, Blocked or Waiting)
- Priority
- Page Table Base Register (PTBR)

**Context Switching이란?**

CPU가 현재 실행 중인 프로세스(또는 스레드)의 상태를 어딘가(PCB)에 저장하고, 다음 실행할 프로세스의 세이프 파일을 Load하여 교체하는 작업

1. Interrupt 발생 : 다른 거 실행하자
2. 기존 실행 중이던 프로세스 A 저장 : 다음에 이어서 하려면 지금 상태를 저장해야지
3. OS Scheduler 작동 : 다음엔 프로세스 B한테 기회를 주겠어
4. 프로세스 B 복원 : 지난 번에 어디까지 했는지 불러오자
5. 실행 : 연산 시작~
</aside>

---

# Context Switching에서 말하는 Overhead

- 잦은 Switching → CPU가 순수 연산을 수행하는 시간 외에, PCB에 쓰고 읽어오는 시간의 비중이 커지기 때문에 컴퓨터가 쓸모 있는 일을 하지 못한다.
- Cache Miss → Process가 바뀌면 L1, L2 Cache에 들어있던 기존 프로세스의 데이터가 전부 쓸모 없어진다. 즉, Cache Flush 과정이 있기 때문에 Switching이 자주 발생하면 캐시가 계속 비기 때문에 miss 확률 이 올라간다.

---

# Process 관점

- 메모리 영역 전체 공간 전환
- Cache Flush (전부 초기화)
- MMU(Memory Management Unit)에서 가상 주소 매핑 테이블을 통째로 교체
- 무겁고 느림

---

# Thread 관점

- Stack, Register만 전환
- 기존 캐시 데이터 일부 유지 가능
- MMU에서 가상 메모리 교체할 필요 X
- 가볍고 빠름