# Day11-2 System Call(시스템 콜)

## 1. 시스템 콜
OS -> 다양한 서비스들을 수행하기 위해 하드웨어를 직접 관리
응용 프로그램 -> OS가 제공하는 인터페이스를 통해서만 자원 사용할 수 있음

- OS가 제공하는 이러한 인터페이스를 `시스템 콜(system call)`이라고 함

> 시스템콜은 커널 영역의 기능을 사용자 모드가 가용 가능하게, 즉 프로세스가 하드웨어에 직접 접근해서 필요한 기능을 할 수 있게 해줌. (응용 프로그램은 시스템 콜을 사용해서 원하는 기능을 수행할 수 있음)

## 2. 종류
### 2.1. 프로세스 제어(Process Control)
- 끝내기(exit), 중지(abort)
- 적재(loac), 실행(execute)
- 프로세스 생성(create process) - fork
- 프로세스 속성 획득과 속성 설정
- 시간 대기(wait time)
- 사건 대기(wait event)
- 사건 알림(signal event)
- 메모리 할당 및 해제

### 2.2 파일 조작(File Manipulation)
- 파일 생성 / 삭제 (create, delete)
- 열기 / 닫기 / 읽기 / 쓰기 (open, close, read, write)
- 위치 변경(reposition)
- 파일 속성 획득 및 설정(get file attribute, set file attribute)

### 2.3. 장치 관리(Device Manipulation)
- 하드웨어의 제어와 상태 정보를 얻음(icotl)
- 장치를 요구(request device), 장치를 방출(relese device)
- 읽기 (read), 쓰기(write), 위치 변경
- 장치 속성 획득 및 설정
- 장치의 논리적 부착 및 분리

### 2.4. 정보 유지(Information Maintence)
- getpid(), alarm(), sleep()
- 시간과 날짜의 설정과 획득(time)
- 시스템 데이터의 설정과 획득(date)
- 프로세스 파일, 장치 속성의 획득 및 설정

### 2.5. 통신(Communication)
- pipe(), shm_open(), mmap()
- 통신 연결의 생성, 제거
- 메시지의 송신, 수신
- 상태 정보 전달
- 원격 장치의 부착 및 분리

### 2.6. 보호(Protection)
- chmod()
- umask()
- chown()

[출처](https://brightstarit.tistory.com/13)