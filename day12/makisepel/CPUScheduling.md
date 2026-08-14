## CPU Scheduling (CPU 스케쥴링)

CPU Scheduling은 **여러 프로세스 중 CPU를 사용할 프로세스를 선택하고 CPU를 할당하는 과정**이며 CPU를 효율적으로 사용하면서 프로세스의 대기 시간과 응답 시간을 줄이는 것이 목적임

- **선점형(Preemptive)** : 실행 중인 프로세스의 CPU를 다른 프로세스가 강제로 빼앗을 수 있음
- **비선점형(Non-Preemptive)** : 프로세스가 CPU를 할당받으면 종료되거나 I/O 등의 이유로 CPU를 반납할 때까지 다른 프로세스가 CPU를 빼앗을 수 없음

### 선점형 스케줄링

- **SRT(Shortest Remaining Time)** : 현재 실행 중인 프로세스보다 남은 CPU Burst Time이 더 짧은 프로세스가 도착하면 현재 프로세스를 중단하고 새 프로세스를 실행함
- **Round Robin(RR)** : 각 프로세스에 일정한 시간인 Time Quantum을 할당하고 시간이 지나면 다음 프로세스에게 CPU를 넘겨줌. 모든 프로세스가 돌아가면서 CPU를 사용하기 때문에 시분할 시스템에 적합함
- **Multi-level Queue** : Ready Queue를 여러 개로 나누고 프로세스의 종류나 우선순위에 따라 각각의 큐에 배치함. 각 큐마다 서로 다른 스케줄링 알고리즘을 적용할 수 있음
- **Multi-level Feedback Queue** : Multi-level Queue와 비슷하지만 프로세스가 하나의 큐에 고정되지 않고 실행 상태나 대기 시간 등에 따라 다른 큐로 이동할 수 있음. 오래 기다린 프로세스를 높은 우선순위 큐로 이동시켜 Starvation을 줄일 수 있음

### 비선점형 스케줄링

- **FCFS(First Come First Served)** : Ready Queue에 먼저 도착한 프로세스부터 CPU를 할당함. 구현이 간단하지만 CPU Burst Time이 긴 프로세스가 먼저 실행되면 뒤의 프로세스들이 오래 기다리는 **Convoy Effect가 발생할 수 있음
- **SJF(Shortest Job First)** : CPU Burst Time이 가장 짧은 프로세스부터 CPU를 할당함. 평균 대기 시간이 짧다는 장점이 있지만 실제 실행 전에는 정확한 CPU Burst Time을 알기 어렵다는 단점이 있음
- **Priority Scheduling** : 우선순위가 높은 프로세스부터 CPU를 할당함. 우선순위가 낮은 프로세스가 계속 CPU를 할당받지 못하는 **Starvation**이 발생할 수 있으며, 이를 해결하기 위해 Aging을 사용할 수 있음