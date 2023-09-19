# 과제 풀이

> 작성자: 최선규

- [과제 풀이](#과제-풀이)
  - [Q. end-system A에서 end-system B로 대용량의 파일을 전달한다고 할때 아래의 두 질문에 대해 답하시오](#q-end-system-a에서-end-system-b로-대용량의-파일을-전달한다고-할때-아래의-두-질문에-대해-답하시오)
    - [1. High Level의 end-system A가 파일에서 패킷을 생성하는 방법을 설명하시오](#1-high-level의-end-system-a가-파일에서-패킷을-생성하는-방법을-설명하시오)
    - [2. 대용량 파일의 Packet 중 하나가 Packet Switch에 도달했을 때, Switch가 Packet을 보낼 Link를 선택하기 위해 어떤 정보를 사용하는가?](#2-대용량-파일의-packet-중-하나가-packet-switch에-도달했을-때-switch가-packet을-보낼-link를-선택하기-위해-어떤-정보를-사용하는가)

## Q. end-system A에서 end-system B로 대용량의 파일을 전달한다고 할때 아래의 두 질문에 대해 답하시오

### 1. High Level의 end-system A가 파일에서 패킷을 생성하는 방법을 설명하시오

> 참고자료
>
> - [[네트워크] TCP/IP 5계층에서 일어나는 일 (1)](https://zion830.tistory.com/104)

![image](https://github.com/Poin-Book/2023.09-network/assets/98688494/f0f63a4d-32d3-4b7c-bb0e-4857f204134c)

패킷 생성과정(= 데이터의 송신 과정)

  1. Application Layer에서 요청 데이터를 생성한다.
  2. Transport Layer에서 `신뢰할 수 있는 통신`을 구현하기 위한 헤더를 데이터에 붙인다.
      - Header : segment
  3. Network Layer에서 다른 네트워크와 통신하기 위한 헤더를 세그먼트에 붙인다.
      - Header : `datagram` or `packet`
  4. Data Link Layer에서 물리적인 채널을 열기 위해 헤더와 트레일러(trailer)를 패킷에 붙인다.
      - Header + Trailer : `frame`
      - Trailer : 데이터의 끝부분에 붙이는 정보로 오류 검출을 위해 사용된다.
  5. Physical Layer에서 전기적 신호로 변환하여 전송한다.

### 2. 대용량 파일의 Packet 중 하나가 Packet Switch에 도달했을 때, Switch가 Packet을 보낼 Link를 선택하기 위해 어떤 정보를 사용하는가?

> 참고자료
>
> - Computer Network: A Top-Down Approach

![image](https://github.com/Poin-Book/2023.09-network/assets/98688494/e650a002-2e91-483b-b3d9-0dd3cebf3bf0)

1번에서 설명한 것과 반대로 패킷을 Network Layer까지 분해해서 packet/datagram header를 확인한다.
