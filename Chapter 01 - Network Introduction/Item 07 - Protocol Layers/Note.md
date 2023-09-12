# Protocol Layers

> 작성자: 최선규

## 목차

## Internet Protocol Stack

- Application Layer
  - network application을 위한 interface를 제공
  - HTTP, SMTP, FTP, DNS
- Transport Layer
  - process 간의 data transfer를 제공
  - TCP, UDP
- Network Layer
  - data를 source(출발점)에서 destination(목적지)으로 전달하는 routing 기능을 제공
  - IP, routing protocols
- Link Layer
- Physical Layer

## Protocol Layering and Data

![image](https://github.com/Poin-Book/2023.09-network/assets/98688494/fe5f8f46-7fc2-4ff1-bee5-73040d52e7cb)

[Source(목적지)]

- 데이터는 각 계층을 거치면서 헤더가 추가되고, 트레일러가 추가되는 과정을 거쳐서 전송됨.
  - 헤더와 트레일러는 각 계층에서 사용하는 정보를 담고 있음.

[Destination(목적지)]

- Source에서 전송된 데이터는 Destination의 각 계층을 거치면서 헤더와 트레일러를 제거하고 전달됨.
  - 이렇게 전달 된 데이터를 Application Layer에서 사용됨.
