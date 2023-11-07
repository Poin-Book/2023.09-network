# Generalized Forwarding and SDN

> 작성자: 김나현

## 목차

- [SDN](#sdn)
  - [SDN 작동 방식](#sdn-작동-방식)
- [Generalized Forwarding and SDN](#generalized-forwarding-and-sdn)
  - [Flow Table](#flow-table)
- [OpenFlow](#openflow)
  - [Flow Table Entries](#flow-table-entries)
  - [OpenFlow Example](#openflow-example)
  - [OpenFlow abstraction](#openflow-abstraction)
    </br></br>

## SDN

> **Software Defined Networking**
> : 소프트웨어를 통해 네트워크 리소스를 가상화하고 추상화하는 네트워크 인프라에 대한 접근 방식

</br></br>

### SDN 작동 방식

[](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FUx31d%2FbtrastS3hRc%2FBHUTpzFMPSQBbrE4arJiVK%2Fimg.png)

- SDN의 핵심: 네트워크 장비의 **Control Plane(제어부)와 Data Plane(전송부)의 분리**
  - `Control Plane`: 네트워크 장비를 제어
  - `Data Plane`: 데이터를 전송하는 역할

</br></br></br>

## Generalized Forwarding and SDN

![](https://slideplayer.com/slide/12385597/74/images/10/Generalized+Forwarding+and+SDN.jpg)

- 각 라우터는 **논리적으로 중앙 집중화**(logically centralized routing controller)된 라우팅 컨트롤러에 의해 계산되고 배포되는 플로우 테이블을 포함함
  - `control plane`이 각각의 라우터에 들어가 있지 않고, 이 logically-centralized routing controller에 따로 존재함
  - 라우터가 굉장히 단순해짐 - 원격에 있는 컨트롤러에서 플로우 테이블을 다 만들어서 보내줌 - 각각의 라우터들은 `data plane` 동작만 수행하면 됨

</br></br>

### Flow Table

> 라우터의 match + action 규칙을 정의해줌

- 헤더의 값을 확인하고 해당하는 조건이 있으면 → `match`
- 특정한 액션을 수행 → `action`
- 이외에도 `counter`를 통해 트래픽의 양을 패킷 수와 바이트로 표시해줌
  </br></br></br>

## OpenFlow

> 네트워크 컨트롤러가 스위치망을 통해 네트워크 패킷의 경로를 정의하는 SDN 프로토콜

</br></br>

### Flow Table Entries

[](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/c6XsUY/btqEmvaDpIO/SQjKmjGXXt8GGY8znRsbuk/img.png)

1. 라우팅
2. 패킷을 보내서 추가 분석
3. Firewall
4. NAT

</br></br>

### OpenFlow example

![](https://blog.kakaocdn.net/dn/dDcCgV/btqEoTugQw9/eImmjead6YWfsl5KEMUxDk/img.png)

- `Destination-based forwarding` : dest. IP가 `51.6.0.8` 이면 포트 `6`으로 포워딩하라
- `Firewall` : TCP 포트가 `22`이면 드랍하라 / src. IP가 `128.119.1.1`이면 드랍하라
- `Destination-based layer 2 (switch) forwarding` : MAC주소가 `22:A7:23:11:E1:02` 이면 포트 `6`으로 포워딩하라

\* (참고) ‘\*’는 와일드카드 문자로서 모든 문자에 매칭된다(필터링 조건이 없다)는 뜻
</br></br>

### OpenFlow abstraction

> match와 action의 조합에 대해서 처리를 할 수 있음

1. `Router` : longest destination IP prefix를 가지고 특정 output port로 forward하는 것
   - match : longest destination IP prefix
   - action : 링크 바깥으로 forward
2. `Switch` : MAC 주소를 가지고 어떤 특정 port로 보내는 것
   - match : 목적지의 MAC 주소
   - action : forward 또는 flood
3. `Firewall` : 특정 IP주소나 port number를 가지고 drop 혹은 block을 하는 것.
   - match : IP 주소 & TCP/UDP 포트 전호
   - action : 허용 또는 거부
4. `NAT` : IP주소에 대해서 rewrite 하는 것
   - match : IP 주소 & 포트
   - action : 주소와 포트 rewrite
