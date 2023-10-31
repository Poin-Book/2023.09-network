# 4.2 What’s inside a router

> 작성자: 조혜원

### Router Ports

Network Interface Card (=LAN)을 Router에서는 Port라고 부름

각각의 구멍에 광케이블이 연결되어 그 케이블을 통해 packet이 들어오면, 어디 port로 나갈지 Router가 결정

### Router Architecture Overview

![Untitled](4%202%20What%E2%80%99s%20inside%20a%20router%204fb772e388bf4afcb2c205a83350fd04/Untitled.png)

**Routing Processor :**

sw적으로 어디로 갈지 정보를 가지고 해석 ⇒ control plane (제어)

**Hight-seed switching fabric :**

switching 역할을 하는 기판

매우 빨라야 함

input port와 output port와 함께 forwarding data plane (HW) 담당

## Input port functions

![Untitled](4%202%20What%E2%80%99s%20inside%20a%20router%204fb772e388bf4afcb2c205a83350fd04/Untitled%201.png)

**line termination :**

- physical layer

물리적 신호를 전기적 신호로 바꿔줌

**link layer protocol (receive) :**

- data link layer

link 계층에서 필요한 정보 처리

link에서 처리한 내용 제거 후 IP 정보를 담은 network layer가 올라옴

**link layer protocol → lookup, forwarding queueing :**

이 단계, 즉 다음 forwarding 단계로 넘어 갈 때, 해당 packet의 error 여부 확인

없으면 header를 보고 destination IP 주소 읽고 경로 결정

⇒ 몇 번 output port로 switching 해야 하는지 결정 하는 건 forward queueing 부분에서

**lookup, forwarding queueing :**

- decentralized switching

분산 switching ⇒ 수많은 input port에서 각각 forwarding을 위한 switching

이 input port 단계에서 실제 routing에 관련된 행동인 forwarding이 이루어짐

**‘line speed’** : switching 할 때, 멈추지 않고 그대로 “매우 빨리” 일어나야함

**queueing** : 나가는 것보다 들어오는 게 빨라지면 queueing 발생 ⇒ input port와 output port 모두 queueing 발생 가능

### Destination-based Forwarding

- Input Port에서 발생하는 routing을 반영한 Forwarding
- Destination IP Address를 기반으로 운영됨
  ⇒ Destination IP Address를 통해 몇 번 output port로 Mapping 해야 하는지 결정 ⇒ 실제 Switch fabric에서 교환이 이루어짐
- Each router has a forwarding table ⇒ 이를 활용해 각각의 Input Port에서 Decentralized Switching을 함
- Upon receiving a packet ⇒ packet 도착
  - header 안에 있는 destination address 확인
  - Table lookup
  - output port determine
  - output port로 packet 내보냄 (switching)
    ⇒ **nanoseconds** : switching은 매우 빨라야 함!
- The next router in the path repeats
- 어느 목적지 (destination address)이던 어느 output port로 나가짐야 할지 처리가 되어야 함 ⇒ 최대 43억개의 IP 주소를 처리해야 함

### Forwarding Table

![Untitled](4%202%20What%E2%80%99s%20inside%20a%20router%204fb772e388bf4afcb2c205a83350fd04/Untitled%202.png)

- 각각 Input Port에서 이루어짐
- IP 주소가 $2^{32}$개라 forwarding table이 너무 길어짐 ⇒ 각각의 value가 아니라 range 형태로 관리하면 좋음 ⇒ table 길이는 줄고, lookup 속도는 빨라짐

![Untitled](4%202%20What%E2%80%99s%20inside%20a%20router%204fb772e388bf4afcb2c205a83350fd04/Untitled%203.png)

이렇게 정해진 host의 prefix만큼을 기준으로 range를 잡아서 처리하면 됨

![Untitled](4%202%20What%E2%80%99s%20inside%20a%20router%204fb772e388bf4afcb2c205a83350fd04/Untitled%204.png)

하지만 이렇게 중간에 애매한 prefix 만큼을 또 구분해줘야 한다면 복잡해짐

그래서 필요한 것이 **Prefix Matching!**

## Prefix Matching

destination address ranges를 prefix(subnet address)로 표현

![Untitled](4%202%20What%E2%80%99s%20inside%20a%20router%204fb772e388bf4afcb2c205a83350fd04/Untitled%205.png)

wild card를 사용해서 4개의 범위로 표현 가능

### Longest prefix matching

![Untitled](4%202%20What%E2%80%99s%20inside%20a%20router%204fb772e388bf4afcb2c205a83350fd04/Untitled%206.png)

2번의 예시처럼 2개 이상의 범위에 포함 되는 IP address일 경우, **“더 긴”** prefix matching 된 range의 link를 따라감

## Hierarchical addressing : route aggregation

계층적 주소 할당 : 라우트 집약

ISP가 자신이 가지고 있던 IP 주소를 n개의 organization에 자신의 network를 설치 해서 나눠줌

- ISP가 host가 접속할 Internet에 n개의 organization이 할당 받은 각각의 범위가 아닌, 하나로 통합된 자신의 IP 주소를 보내주면 됨 ⇒ 자신이 중간 단계 역할
  - 자신의 IP 주소를 나눠가진 n개의 organizations의 IP Range에 해당하는 IP 주소를 Destination IP 주소로 가진 host가 Internet에서 ISP Router로 넘어올 수 있게 정보를 제공하는 것이 ISP의 역할
- **Advertise**
  - Routing에 필요한 정보를 Router들에게 뿌리는 message
  - 이 정보를 받은 router들은 routing table에 반영
  - Router들이 Network상에서 변경된 주소 정보들을 업데이트 하기 위해 새로운 목적지 주소들에 대해 알려주는 Message!
  - ex) Internet에서 들어온 host의 message는 일단 Routing table을 통해 ISP로 이동 후, ISP의 Router에서 Forwarding Table을 통해 destination으로 이동

**More Specific routes**

만약 두 개의 ISP가 합병 되고, 1번 ISP에 있던 organization이 어떤 link에 의해 2번 ISP로 옮겨진 상황

⇒ 1번 ISP의 범위 안에서 특정 bit 주소에 해당하는 주소만 2번 port로 와야 하는 번잡한 문제 발생

**!! Longest Prefix Matching으로 해결 할 수 있음 !!**

옮겨진 organization의 ip 주소를 advertise message를 통해 새로 보내줌 ⇒ 200.23.18.0/23에 해당하는 주소는 2번 port로 보내줘~

원래 속해 있던 1번 entry와 똑같지만, 조금 더 디테일 한 주소는 2번으로 오게 만드는 것 ⇒ Longest Prefix matching

CIDR (서브넷 주소, Host 주소 계층적 사용)와 Longest Prefix Matching 구조로 Destination IP를 찾아가는 방식을 통해 Routing table과 Routing table의 update를 효율적으로 사용 가능

![Untitled](4%202%20What%E2%80%99s%20inside%20a%20router%204fb772e388bf4afcb2c205a83350fd04/Untitled%207.png)

## Router architecture overview

router의 핵심 기능 = switching

![Untitled](4%202%20What%E2%80%99s%20inside%20a%20router%204fb772e388bf4afcb2c205a83350fd04/Untitled%208.png)

### Switching fabrics (스위칭 기판)

Input port로 들어와 잠시 동안 input buffer로 들어간 packet을 적절한 output buffer로 내보내는 역할

switching rate : fabric 종류에 따라 속도 달라짐

- memory - 메모리 거침
- bus - 중간 단계 없이 직접 output으로 전달
- crossbar - bus들을 병렬로 사용

memory → bus → crossbar 순서로 switching 속도가 빨라짐

## Input Port의 Queueing은 왜 발생할까?

1. Switching Fabric이 Input Rate에 비해 느릴 때
2. 목적지가 같은 애들이 동시에 여러 Input Port에서 대기 중일 때

   목적지가 같은 애들은 crossbar를 사용하더라도 동시에 전송 불가

Head-of-the-Line (HOL) blocking : 앞에 있는 packet이 block 돼서 뒤에 있는 packet은 block과 상관 없이 보내질 수 있는데 함께 block 되어 있는 상태가 발생

## Output port의 Queueing은 왜 발생할까?

1. **Buffering** : 어떤 packet을 drop할 지 결정
2. **Scheduling discipline** : 어떤 packet을 먼저 보낼지 결정
3. 전송 속도보다 도착 속도가 더 빠른 경우

## Buffer Management

- **drop**
  - buffer가 full 상태일 때 packet을 drop해야 함
  - **tail drop** : 늦게 온 packet drop
  - **priority** : 우선순위로 drop
  - **random** : 확률 적으로 random 돌림
  - tail이 제일 기본적인관리법
- **marking**
  - buffer의 혼잡 신호를 알리기 위해 packets에 표시를 남김
  - 도착 packet의 header에 원래는 ECN 값이 없음 → queue에 packet이 많으면 router가 ECN = 1로 marking하여 Header에 추가하여 보냄 → 이를 본 다음 end(= host)는 두 end 사이의 router의 buffer가 많이 차 있음을 OS에서 파악 → end에서 input speed를 낮추는 대처를 함
  - ECN이라는 복잡도 함수를 1로 활성화 시켜 marking해주는 방식

## Packet Scheduling : FCFS

- **packet scheduling** : 다음 link에 어떤 packet을 보낼지 결정하는 것
  - first come, first served (FCFS)
  - priority (우선순위)
  - round robin
  - weighted fair queueing
