# 2.1 Principles of Network Applications

# Principle of Network Applications

> 작성자: 조혜원

## 목차

## 1. Network Application

**Network Application** ⇒ 네트워크를 응용하는 프로그램.

protocol을 통해 떨어진 두 개 이상의 app이 communication 함

### Creating a network app

> = **write programs**와 같은 의미

여러 다른 컴퓨터(= end systems, hosts)에서 동작하는 program을 만드는 것

⇒ 그 프로그램들이 network 위에서 소통하며 data를 주고 받음

> **no need to write software for network-core devices**

network core를 위한 sw는 신경 쓰지 않고, 응용 프로그램만 만들면 된다

**! 매우 큰 장점**

---

## 2. Application architectures

1. **client-server**
2. **peer-to-peer (P2P)**

### Client-server Paradigm

**Server :**

- **always-on host :** 항상 전원이 켜 있는 end-system. client의 접속을 기다림
- **permanent IP address** : 영구 적인 IP 주소. client가 요청을 보낼 서버의 IP주소
- **often in data centers, for scaling** : 가정집이 아닌 센터에 있음
  - data 센터는 에너지가 많이 필요하고, 발열이 심해서 발전소 옆이나, 추운 지역에 데이터 센터를 짓는다

**Clients :**

- **contact, communicate with server** : 먼저 server에 접속을 시도하는 end-system. 연결 하면, 무언가를 요청하며 communication 함
- **may be intermittently connected :** 항상 켜져 있지 않고, 간헐적으로 연결
- **may have dynamic IP addresses** : 고정된 IP 주소가 필요하지 않음. 주소가 바뀔 수 있음
- **do _not_ communicate directly with each other** : client끼리 communication 하지 않음
- examples: HTTP (웹), IMAP(이메일), FTP(파일)

### Peer-Peer architecture

→ client와 server 역할 구분이 없음 / peer = 동료

- **no always-on server** : 항상 기다리는 서버가 있지는 않음
- **arbitrary end systems directly communicate** : server를 거치는 것이 아닌, end-systems끼리 communication
- **peers request service from other peers, provide service in return to other peers** : peer들끼리 서로 요청을 교환 하며 1대1에서 1대10 등 점점 커짐
  - **_self scalability_** = 네트워크 연결성이 점점 커짐 / 새로운 피어가 새로운 서비스 용량은 물론 새로운 서비스 요구사항을 제공합니다
- **peers are intermittently connected and change IP addresses** : peer들이 간헐적으로 연결 되어 IP 주소 변경 / peers들의 주소를 관리하는 **보조 server** 존재
- example: P2P file sharing → data center 속 server가 아닌 각 computer의 HDD에 data가 저장됨

---

# 3. Processes communicating

⇒ 2개 이상의 process들이 network를 통해 communication 하며 돌아가는 것이 network application이 특정한 service를 구현하는 모습

> **Program** : 패시브 상태. 수동적으로 disk에 저장된 상태

> **Process** : program running within a host | 호스트에서 동작하고 있는 프로그램

⇒ program을 실행 시켜 운영체제에 의해 자원이 관리 되면서 active한 상태

- 같은 host에 있는 2개의 process가 (같은 계급을 갖는 process, inter-process) **Inter-process communication**을 사용하여, OS를 통해 communication 함
- Network communication도 다르지 않음 ⇒ 물리적으로 다른 hosts에 있는 process들이 OS가 아닌 Network를 통해 communication 하며 **meesages**를 주고 받음 ⇒ 범위가 넓어짐

**Client process** : client 역할을 하는 proces로 연결 요청을 먼저 하는 process.

**Server process** : server 역할을 하는 process로 먼저 실행 되어 있으면서, 다른 process의 연결 요청을 기다리는 process.

### Sockets

- **Software Interface** : SW 연결 창구로, **Application layer**와 **Transport layer**(OS에 존재하는 계층)을 연결 시켜 줌
- **Application Programming Interface (API)** : application과 network 사이의 api

> **Network Programming = Socket Programming**

Process가 아래의 network와 어떻게 연결 돼서 message를 송수신 하는가? ⇒ **Socket**을 통해 OS 계층과 communication 함

![Untitled](2%201%20Principles%20of%20Network%20Applications%20a9c8b3155a544b36905a928907f2dd1a/Untitled.png)

![Untitled](2%201%20Principles%20of%20Network%20Applications%20a9c8b3155a544b36905a928907f2dd1a/Untitled%201.png)

client-server는 한 개의 pair!

1. socket 주소를 통해 client와 server에 존재하는 socket끼리 연결
2. client Process에서 보내는 message는 socket을 통해 아래 transport layer에 도착
3. socket을 통해 client의 message를 server로 전송
4. socket을 통해 server의 transport layer에서 server Process로 message가 전달

### Addressing process (IP address)

물리적인 인터넷 주소

- to receive messages, process must have unique **identifier** : 내가 지칭하는 특정한 object를 식별하는 고유 식별자 필요 = IP 주소
- host는 32-bit의 IP address를 가짐

### IP address & Port number

하나의 host 안에서도 어떤 process인지 지칭해주기 위해 port number 필요 (int 형)

![Untitled](2%201%20Principles%20of%20Network%20Applications%20a9c8b3155a544b36905a928907f2dd1a/Untitled%202.png)

스마트폰이 배당 받은 물리적인 인터넷 주소, 즉 IP 주소로 해당 스마트폰과 관려된 신호들이 오게 되어 있음

그 중에서도 이렇게 각각 네이버, 카카오톡, 멜론 등의 서비스와 연결된 socket이 있고, 그 socket들을 구분하는 port 넘버에 따라 나뉘어서 신호가 가게 됨

ex ) 멜론의 음악, 카톡의 메세지, 네이버의 검색 결과 …

- **identifier** includes both **IP address** and **port numbers** associated with process on host.
  host가 아닌 process를 addressing 한다는 것은 host + app 정보를 구분해서 찾아간다고 생각하기!
  host를 찾을 땐 IP 주소만, host에서 실행 되고 있는 **process는 IP 주소 + Port 번호** → 다른 process와 구분하기 위함
- example port number :
  - HTTP server : 80
  - mail server : 25

---

## What transport Service does an app need?

network app service가 필요한 transport service는 무엇일까?

⇒ App을 만들 때 process(app)이 어떤 기능을 필요로 하는 지에 따라 transport를 적절히 골라야 함

**판단 요소 4가지**

1. **Data integrity 신뢰도**

- Reliable data transfer : 안정성 있게 전달 돼야 함! email, file등은 내용이 깨지면 안됨

1. **Timing 전 시간**

- some apps (e.g., Internet telephony, interactive games) require low delay to be “effective” : 효과적이기 위해서는 지연 시간이 짧아야 함

1. **Throughput 처리율**

- data를 업로드하거나 다운로드 받을 때 최소 처리량을 원함

1. **Security 보안**

| application            | data loss     | throughput                            | timing         |
| ---------------------- | ------------- | ------------------------------------- | -------------- |
| file transfer/download | no loss       | elastic                               | no             |
| e-mail                 | no loss       | elastic                               | no             |
| Web documents          | no loss       | elastic                               | no             |
| real-time audio/video  | loss-tolerant | audio: 5Kbps-1Mbps video:10Kbps-5Mbps | yes, 10’s msec |
| streaming audio/video  | loss-tolerant | same as above                         | yes, few secs  |
| streaming audio/video  | loss-tolerant | Kbps+                                 | yes, 10’s msec |
| text messaging         | no loss       | elastic                               | yes and no     |

### Internet transport protocols services

두 개념은 대립 구조

**TCP service :**

loss가 있어선 안 됨!

- **reliable transport** : 안정적인 전송 → But, 시간이 더 걸림
- **connection-oriented** : server와 client간에 필요한 설정

**UDP service :**

- **unreliable data transfer**
- **does not provide : 안정성, 연결 설정 제공하지 않음**

시간은 sensitive (예민한) But, loss에 책임 지지 않음
