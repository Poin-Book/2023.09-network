# Circuit Switching vs Packet Switching

> 작성자: 최선규

## 목차

- [Circuit Switching vs Packet Switching](#circuit-switching-vs-packet-switching)
  - [목차](#목차)
  - [Packet](#packet)
  - [Packet Switching](#packet-switching)
  - [Circuit Switching](#circuit-switching)
  - [Circuit Switching vs Packet Switching](#circuit-switching-vs-packet-switching-1)
    - [Packet Switching - 장단점](#packet-switching---장단점)
    - [Circuit Switching - 장단점](#circuit-switching---장단점)

## Packet

- 데이터를 전송하는 단위이며 크게 Header와 Body로 구성되어 있다.
- 이러한 패킷의 구조나 주소 체계 등이 protocol의 일종이다.

[header]

- 목적지(end-system) 주소
- 패킷 번호
- 길이
- protocol
- checksum
- ... etc.

[payload(body)]

- 실제 데이터

## Packet Switching

![image](https://github.com/Poin-Book/2023.09-network/assets/98688494/f45cad1d-50c5-4666-80f6-0feaadb10b55)

- `비연결 지향형`
- 데이터를 전송하기 전에 논리적인 연결이 설정되지 않아서 패킷이 독립적으로 전송되고 패킷을 수신한 라우터는 최적읜 경로를 찾아서 패킷을 목적지로 전달하게 됨.
- 모든 패킷은 각기 다른 경로를 통해 목적지에 도달할 수 있으며, 도착 순서 또한 매 시행마다 달라질 수 있음.
- 때문에 모든 패킷이 도착하면 순서를 확인하고 재조합을 해야함.

[주의점]

- 라우팅 알고리즘을 이용하여 경로를 설정하고, 중간의 라우터들을 거처 최종 목적지에 도달하게 됨.
- 이러한 과정에서 패킷은 다음 라우터로 이동하기 위해 큐에서 대기(queueing)하게 되는데, 라우터가 수용할 수 있는 큐의 범위를 초과하게 되면 손실(loss)가 발생하게 됨.

## Circuit Switching

![image](https://github.com/Poin-Book/2023.09-network/assets/98688494/6a1de82f-4029-4520-b70f-da689986d7b5)

- '연결 지향형'
- 데이터를 전송하기 전에 `논리적으로 목적지와 연결`한다.
- 각 패킷에 가상회선 식별 번호가 존재하고 이 식별 번호를 통해서 전송된 순서대로 도착하게 됨.
- 패킷이 도착하는 순서가 보장되기 때문에 패킷의 순서를 재조합할 필요가 없음.

[주의점]

- 통신을 위한 연결을 해야하며 연결을 하는 동안 출발지(source) to 목적지(destination)까지의 회선을 독점하게 됨.
  - 즉, 회선을 독점(dedicated)하기 때문에 다른 통신을 할 수 없음.
- 그만큼 안전하지만 circuit를 예약하는 방식이 복잡하며 오랜 시간이 걸림.

## Circuit Switching vs Packet Switching

### Packet Switching - 장단점

[장점]

- 회선 이용률이 높고, 속도 변환, 프로토콜 변환이 가능하며, 음성 통신도 가능하다
- 고 신뢰성 : (경로선택, 전송 여부 판별 및 장애 유무 등) 상황에 따라 교환기 및 회선 등의 장애가 발생하더라도 패킷의 우회 전송이 가능하므로 전송의 신뢰성이 보장된다.
- 고품질 : 디지털 전송이므로 인접 교환기 간 또는 단말기와 교환기 간에 전송 오류검사를 실시 하여 오류 발생 시 재전송이 가능함
- 고효율 : 다중화를 사용하므로 사용 효율이 좋음
- 이 기종 단말장치 간 통신 : 전송 속도, 전송 제어 절차가 다르더라도 교환망이 변환 처리를 제공하므로 통신이 가능

[단점]

- 경로에서의 각 교환기에서 다소의 지연이 발생한다
이러한 지연은 가변적이다. 즉, 전송량이 증가함에 따라 지연이 더욱 심할 수도 있다.
패킷별 헤더 추가로 인한 오버헤드 발생 가능성이 존재한다.

### Circuit Switching - 장단점

[장점]

- 대용량의 데이터를 고속으로 전송할 때 좋음
- 고정적인 대역폭(Bandwidth)을 사용
- 접속에는 긴 시간이 소요되나 접속 이후에는 접속이 항상 유지되어 전송 지연이 없으며, 데이터 전송률이 일정함
- 아날로그나 디지털 데이터로 직접 전달
- 연속적인 전송에 적합

[단점]

- 회선 이용률 측면에서 비효율적임
- 연결된 두 장치는 반드시 같은 전송률과 같은 기종 사이에 송수신이 요구됨(다양한 속도를 지닌 개체 간 통신 제약)
- 속도나 코드의 변환이 불가능(교환망 내에서의 에러 제어 기능이 어렵다)
- 실시간 전송보다 에러 없는 데이터 전송이 요구되는 구조에서는 부적합하다
- 통신 비용이 고가이다.
