# DNS

> 작성자: 심세원

## 목차

<br>

## IP Address

> internet host를 식별하기 위한 고유한 주소
> 
- 4 byte = (32bits)

1. 컴퓨터가 네트워크 상에서 통신을 하고 싶음
2. 자신이 누구인지 식별할 수 있는 수단이 있어야함
3. 이 “식별 수단”이 IP주소 임

**→ IP 주소는 겹치지 않고 고유해야함**

“*IP 주소가 부족할 수 있지 않나?”*

→ 사설 IP 주소를 사용해 공인 IP 주소로 바꾸는 기술이 나옴

ex) `NAT` , `서브네팅`, `IPv6` 

### Domain name

→ ex) www.naver.com

- 사람이 알아보기에는 Domain name이 편함
- Router가 알아보기에는 IP가 편함

### 따라서, Domain name을 IP로 바꿔주기 위한 시스템이 필요

= domain name을 ip주소로 바꿔달라고 하는 애플리케이션 레이어 프로토콜이 필요하다.

→ 이것이 **DNS**

# DNS (Domain Name System)

- 분산 형태의 DB
- Application-layer Protocol
- 도메인 이름을 아이피 주소로 변환 시킬 수 있는 데이터를 가지고 있는 DB
- TCP 연결

### Services

- Host name을 IP로 바꿔줌
- Load Distribution
    - 서버 부하를 분산 시킬 수 있음
- host aliasing
- mail server aliasing

## Distributed(분산), Hierarchical(계층) Database이다.

### why?

만약에, Single centralized server라면?

- central server에서 먼곳은 느릴 수도
- 많은 요청이 특정한 지점으로 몰리면 Traffic Volume이 많아짐
- maintenance
- single point of failure

### 레이블 단위에 따라 계층 구조를 이룸

![Untitled](DNS%20d9f67057f9dc48528f826a433b6942e3/Untitled.png)

1. 클라이언트가 amazon.com의 IP 주소를 원함
2. Root server에서 .com DNS server를 질의함
3. .com DNS server에서 Amazon이 관리하는 DNS server를 질의함
4. Amazon이 관리하는 DNS server에서 IP 주소를 얻음

### **Iterated Query (연산이 순차적으로 이루어짐)**

![Untitled](DNS%20d9f67057f9dc48528f826a433b6942e3/Untitled%201.png)

**연결한 서버가 하위 서버를 알려주고 다시 하위 서버로 접속하고의 반복**

### Recursive Query (재귀적으로 처리)

![Untitled](DNS%20d9f67057f9dc48528f826a433b6942e3/Untitled%202.png)

루트 DNS 서버가 답을 자신이 직접 알아와서 줌

→ 답이 있으면 해결되고, 해결되는 방식으로 한번에 답을 줌

> **루트 DNS 서버의 Over Head가 커짐 (load가 커짐)**
> 

## DNS: Caching, Updating records

DNS 서버는 Caching을 한다

### 방식

1. 저번에 방문 했던 ip 주소
    
    ip 주소를 한번이라도 알게 됨
    
2. Cash로 남아있음
3. Cash가 hit가 되면 한번에 ip 주소를 알려줌

→ Root server 등 서버의 부담이 엄청 줄게 됨

### **특정 서버가 아이피 주소를 바꾼다면?**

**바뀐 아이피 주소를 업데이트 해야하지만, Cash로 이전 서버의 주소가 남아있어 해당 서버로 연결을 하지 못하게 됨**

→ `**순결성**`이 깨짐

### 따라서, Cash에 out-of-date을 준 뒤, 그 기한까지만 Caching을 한다

만료되면 새로 알아와서 Cashing을 함

## DNS Security

### DDoS attacks

> 여러 컴퓨터 또는 장치에서 대상 DNS 서버로 동시에 대량의 트래픽을 전송하여 서버를 과부하로 만드는 것
> 
- 정상 사용자의 요청이 처리되지 못하고 서비스 중단이 발생

***루트 서버 폭격 (Root Server Bombardment)***

- 공격자는 인터넷의 핵심 루트 서버를 대상으로 대량의 DNS 쿼리를 전송

***TLD 서버 폭격 (TLD Server Bombardment)***

- 공격자는 대상 TLD(Top-Level Domain) 서버에 대량의 DNS 쿼리를 전송

### Redirect attacks

> 사용자를 악의적인 웹 사이트로 유도하거나 중요한 정보를 탈취
> 
- 유용한 웹 사이트로 보이는 곳에서 사용자의 데이터를 도난당하거나 악의적인 앱을 설치하도록 유도

***중간자 공격 (Man-in-the-Middle)***

- 공격자가 사용자와 DNS 서버 사이에 위치하여 DNS 트래픽을 감시하고 조작하는 공격
- 공격자는 사용자가 DNS 서버에 요청하는 정보를 가로채거나 변경할 수 있음

***DNS 독점 (DNS Poisoning)***

- DNS 캐시에 악의적인 정보를 삽입하여 사용자를 악의적인 웹 사이트로 리디렉션하려는 시도
- 공격자는 사용자의 DNS 캐시를 변조하여, 원래의 도메인 이름을 입력했을 때 올바른 IP 주소 대신 악의적인 IP 주소를 반환

### Exploit DNS for DDoS

> DNS를 악용한 DDoS 공격은 공격자가 DNS 쿼리를 이용하여 대량의 트래픽을 생성하고 대상 서버를 과부하로 만듬
> 

***소스 주소 위조를 사용한 쿼리 전송 (Sending Queries with Spoofed Source Address: Target IP)***

- 공격자가 DNS 쿼리를 위조된 출발지 주소와 함께 전송
- 공격자는 다른 사람처럼 보이며, 다른 시스템에서 쿼리를 실행한 것처럼 가장
- 공격자는 대상 시스템을 혼란스럽게 만들고 공격을 숨길 수 있음

***증폭 요구 (Amplification Required)***

- DNS 서버에 쿼리를 보내는 데 있어서 데이터를 증폭시켜 공격을 강화
- 공격자는 작은 쿼리로부터 큰 응답을 얻어내는 방식을 사용하여 공격을 더 강력하게 만듬
- 대상 시스템을 더 쉽게 과부하로 몰아넣음