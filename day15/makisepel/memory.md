## Computer Memory

CPU가 프로그램을 실행하고 데이터를 처리하기 위해 사용하는 저장 공간으로 CPU와 가까울수록 속도가 빠르고 용량이 작으며 가격이 비싼 특징이 있음

### Register

CPU 내부에 존재하는 가장 빠른 저장 공간, CPU가 현재 실행 중인 명령어나 연산에 필요한 데이터를 임시로 저장함

용량은 매우 작지만 CPU가 직접 접근하기 때문에 다른 메모리보다 접근 속도가 빠름

### Cache

CPU와 Main Memory 사이에 위치하는 고속 메모리, CPU가 자주 사용하는 데이터와 명령어를 저장하여 Main Memory에 접근하는 횟수를 줄임

→ Cache는 시간적 지역성과 공간적 지역성을 이용하여 필요한 데이터를 미리 저장

### Main Memory

현재 실행 중인 프로그램과 데이터를 저장하는 주기억장치, (일반적으로 RAM)

CPU가 직접 접근할 수 있으며 Register와 Cache보다 용량이 크지만 속도는 느리며 전원이 꺼지면 저장된 데이터가 사라지는 휘발성을 지님

### Secondary Storage

프로그램과 데이터를 장기간 저장하는 보조기억장치이며, ex) SSD, HDD

Main Memory보다 속도는 느리지만 용량이 크고 전원이 꺼져도 데이터가 유지되는 비휘발성 저장장치

### Memory Hierarchy

메모리는 CPU와의 접근 속도와 저장 용량에 따라 계층 구조를 가지며, CPU에 가까울수록 빠르고 용량이 작으며 CPU에서 멀어질수록 느리지만 용량이 커짐

Register → Cache → Main Memory → Secondary Storage 순