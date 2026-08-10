## Paging vs Segmentation

앞서 가상 메모리(Virtual Memory)는 **실제 물리 메모리보다 큰 프로그램도 실행할 수 있도록 프로세스의 일부만 메모리에 올리고 나머지는 보조기억장치에 저장하는 기법**. CPU가 사용하는 가상 주소는 MMU를 통해 실제 물리 주소로 변환됨

### Paging

프로세스의 주소 공간을 고정된 크기의 Page로 나누고, 물리 메모리를 같은 크기의 Frame으로 나누어 서로 다른 위치에 배치하는 방식임

- 프로세스 :  Page 단위로 분할
- 메모리 : Frame 단위로 분할

Page와 Frame의 크기는 동일하고, Page Table을 이용하여 논리 주소를 물리 주소로 변환함. 마지막 Page의 남는 공간에서 내부 단편화가 발생할 수 있음

→ 주소 변환

논리 주소는 Page Number와 Offset으로 나뉘며 Page Number를 Page Table에서 찾으면 해당 Page가 저장된 Frame Number를 얻을 수 있음. Offset은 그대로 사용함

- Logical Address = Page Number + Offset
- Physical Address = Frame Number + Offset

### Segmentation

프로세스의 주소 공간을 Code, Data, Stack과 같은 논리적인 단위인 Segment로 나누어 메모리에 배치하는 방식

- 프로세스 : Segment 단위로 분할
- 메모리 : 각 Segment의 크기에 맞게 할당

- Segment는 각각 크기가 다르며, Segment Table을 이용하여 논리 주소를 물리 주소로 변환함
- Segment Table에는 각 Segment의 시작 주소인 Base와 Segment의 크기를 나타내는 Limit이 저장됨
- 논리적인 단위로 나누기 때문에 메모리 보호와 공유에 유리하지만 Segment의 크기가 서로 달라 외부 단편화가 발생할 수 있음

→ 주소 변환

논리 주소는 Segment Number와 Offset으로 나뉘며 Segment Number를 Segment Table의 인덱스로 사용하여 해당 Segment의 Base와 Limit을 확인함. 이후 Offset이 Limit보다 작은지 확인하고, 범위 내에 있다면 Base에 Offset을 더하여 물리 주소를 계산함

- Logical Address = Segment Number + Offset
- Physical Address = Base + Offset
- Offset < Limit : 접근 가능
- Offset >= Limit : 잘못된 메모리 접근

⇒ Paging은 고정된 크기로 나누어 외부 단편화를 해결하는 데 유리하고, Segmentation은 프로그램의 논리적인 단위로 나누어 보호와 공유에 유리