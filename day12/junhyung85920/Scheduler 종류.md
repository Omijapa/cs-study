# Scheduler 종류

Status: Done

# 개념

<aside>
📜

**Scheduler란?**

어떤 프로세스에게 자원을 할당할지 결정하는 커널의 모듈

프로세스의 상태 변화에 따라 단, 중, 장기 Scheduler로 나뉜다.

</aside>

---

# Scheduler 종류

### 단기 Scheduler (CPU Scheduler)

- CPU ↔ Memory 사이의 Scheduling
- 프로세스 상태 변화: Ready → Running → Waiting → Ready

### 중기 Scheduler

- 메모리의 여유공간을 마련하기 위해 프로세스를 디스크의 임시 공간으로 Swap Out
    - 여유가 생기면 나중에 다시 Swap In해서 메모리로 가져옴
- 프로세스 상태 변화: Ready / Blocked → Suspended (Swap Out)

<aside>
📜

메모리 부족 시, 가상 메모리의 페이징 기법과 중기 스케줄러의 Swap Out은 뭐가 다른가?

- 단위가 전체 프로세스인지, 프로세스를 쪼갠 페이지 단위인지
- 주체가 Scheduler인지, MMU인지
- 실행 상태가 Suspended인지, Running인지
</aside>

### 장기 Scheduler

- Memory ↔ Disk 사이의 Scheduling
- 어떤 프로세스를 Ready Queue에 넣을 것인지를 결정한다.
- 프로세스 상태 변화: New → Ready
- 현대 컴퓨터에서의 메모리가 굉장히 커진 덕분에 냅다 메모리에 다 올리고, 장기 Scheduler를 거의 사용하지 않는다.