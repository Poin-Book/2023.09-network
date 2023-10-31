# 4.1 Overview of Network Layer

> 작성자: 조혜원

### Network Layer :

**packet을 목적지까지 안전하게 전달해주는 Layer**

**Host, Router network layer functions:**

- **routing protocol**
  - path selection → 목적지까지 가기 위한 경로를 찾아줌
  - RIP, OSPF, BGP
- **Software Defined Networking (SDN)**
  - Network는 대부분 HW 동작
  - 그것을 SW적으로 바꿔주는 비교적 최신 기술
- **IP Protocol**
  - Header 정보
  - packet의 header 정보를 기반으로 routing protocol 동작
- **ICMP protocol**

  - Router들 사이에서 정보 교환, 상황 변환을 주고받는 protocol

- Transport layer의 “segment”를 받음
- segment를 받아, Network Layer의 Header를 추가하기만 할 뿐, segment의 내용을 보진 않음
- **Datagram** : Network Layer에서의 data block을 의미. source IP와 destination IP를 Header에 담아 압축된 segment에 추가한 Data

※ 참조 : host들은 5계층 표현이 다 되어 있으나, Router들은 3계층만 표현 (network ~ physical)한 이유는?

⇒ Data block을 다 받아도, Transport Layer’s Header + Message 부분은 신경 쓰지 않고 Network 부분만 참고하기 때문

Router들은 Network IP 주소만 참조! ⇒ Router가 Network 장치인 이유

## Two key Network-Layer (Router) Functions ★★★★

### Routing

Destination IP를 가진 host로 가기 위해 길을 찾아주는 역할을 Network Layer에서 해주는 일

- **Network-Wide action**

### Forwarding

정해진 길로 따라가기 위해 중간 중간의 선택지들 (router에 연결된 여러 link들) 중에 어떤 link 쪽으로 data를 forwarding 해야 하는지 router 단위로 결정하는 일

- **Router-Local action**
- **Forwarding tables**를 사용

### **두 Function 사이의 관계성**

⇒ Routing 알고리즘에 의해 얻은 Destination IP로 가기 위해 Router에서 어느 방향의 Link로 내보낼지 판단하는 Forwarding Table 생성

**Routing Algorithm**

정책을 만들어 end-end-path를 결정

global한 정책

**Local Forwarding Table**

현재 하나의 router에서 local forwarding 할 곳을 결정

정책에 맞춰 forwarding 하는 걸 table로 만듦

router 하나의 관점에서 local하게 봄 ( 어느 link로 갈까? )
