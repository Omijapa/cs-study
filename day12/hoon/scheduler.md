# day12-2. Scheduler

## 1. 스케쥴러
- 한정적인 메모리를 여러 프로세스가 효율적으로 사용할 수 있도록 다음 실생 시간에 실행할 수 있는 프로세스 중에 하나를 선택하는 역할

## 2. Queue 종류
프로세스를 스케쥴링하기 위한 Queue의 종류
```text
- `Job Queue`: 현재 시스템 내에 있는 모든 프로세스의 집합
- `Ready Queue`: 현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는 프로세스의 집합
- `Device Queue`: Device I/O 작업을 대기하고 있는 프로세스의 집합
```

## 3. 스케쥴러 종류
각각의 Queue에 프로세스들을 넣고 빼주는 스케쥴러의 종류

### 3.1. 장기 스케쥴러(Lont-term scheduler / job scheduler)
메모리는 한정되어 있는데, 많은 프로세스들이 한꺼번에 메모리에 올라올 경우 대용량 메모리(일반적으로 디스크)에 임시로 저장함. 이 pool에 저장되어 있는 프로세스 중 어떤 프로세스에 메모리를 할당하여 ready queue로 보낼지 결정하는 역할 수행

```text
- 메모리와 디스크 사이의 스케쥴링 담당
- 프로세스에 메모리(및 각종 리소스)를 할당(admit)
- degree of Multiprogramming(메모리에 여러 프로그램이 올라가는 것) 제어
- 프로세스의 상태(new -> ready(in memeory))
```

메모리에 프로그램이 너무 많이 올라가도, 너무 적게 올라가도 성능이 좋지 않은 것

### 3.2. 단기 스케쥴러(Short-term scheduler / CPU scheduler)

```text
- CPU와 메모리 사이의 스케쥴링 담당
- Ready Queue에 존재하는 프로세스 중 어떤 프로세스를 running시킬지 결정
- 프로세스에 CPU를 할당(schduler dispatch)
- 프로세스의 상태(ready -> running -> waiting -> ready)
```

### 3.3. 중기 스케쥴러(Medium-term scheduler / Swapper)

```text
- 여유 공간 마련을 위해 프로세스를 통쨰로 메모리에서 디스크로 쫒아냄(swapping)
- 프로세스에게서 memory를 deallocate
- degree of Multiprogramming 제어
- 현 시스템에서 메모리에 너무 많은 프로그램이 동시에 올라가는 것을 조절하는 스케줄러
- 프로세스의 상태(ready -> suspended)
```

## 참고
### `Suspended(stopped)`
- 외부적인 이유로 프로세스의 수행이 정지된 상태. 메모리에서 내려간 상태
- 프로세스 전부 디스크로 swap out됨.
- blocked 상태는 다른 I/O 작업을 기다리는 상태이기 때문에 스스로 ready state로 돌아갈 수 있지만, suspended 상태는 외부적인 이유로 suspending되었기 때문에 스스로 돌아갈 수 없음

[출처](https://velog.io/@ragnarok_code/OS-%EC%8A%A4%EC%BC%80%EC%A5%B4%EB%9F%AC-Scheduler%EB%9E%80)