# 과제 풀이

> 작성자: 최선규

- [과제 풀이](#과제-풀이)
  - [TCP vs UDP](#tcp-vs-udp)
    - [데이터가 순서대로 도착하는 프로토콜은 무엇인가?](#데이터가-순서대로-도착하는-프로토콜은-무엇인가)
    - [일정 시간내로 도착한다는 보장이 되는 프로토콜은 무엇인가?](#일정-시간내로-도착한다는-보장이-되는-프로토콜은-무엇인가)
    - [센서 통신에 적합한 프로토콜은 무엇인가?](#센서-통신에-적합한-프로토콜은-무엇인가)
  - [Packet 통신 시간 계산](#packet-통신-시간-계산)
    - [Queuing delay - 1](#queuing-delay---1)
    - [Queuing delay - 2](#queuing-delay---2)
    - [Propagation delay](#propagation-delay)
    - [Total delay](#total-delay)

## TCP vs UDP

### 데이터가 순서대로 도착하는 프로토콜은 무엇인가?

- 정답: TCP
  - TCP는 데이터를 전송할 때, 데이터의 순서를 확인하고, 데이터가 순서대로 도착하지 않으면 재전송을 요청한다.
  - 즉, TCP를 따른 시스템에서는 데이터가 순서대로 도착한다는 것을 보장한다.

### 일정 시간내로 도착한다는 보장이 되는 프로토콜은 무엇인가?

- 정답: 없음
  - TCP와 UDP는 데이터를 전송할 때, 데이터가 일정 시간내로 도착한다는 보장을 하지 않는다.
  - TCP는 데이터의 순서를 보장하며 안정적인 데이터 전송을 목표로한다.
  - UDP는 데이터의 오류, 손실, 중복을 허용하며 데이터의 안전성보다는 속도를 목표로한다. 하지만, 일정 시간내로 도착한다는 것을 보장하는 것은 아니다.

### 센서 통신에 적합한 프로토콜은 무엇인가?

- 가정
  - 초당 1개의 데이터가 센서에서 출력되고 있다.
  - 수신기는 모든 값이 아닌 가장 최근의 값을 가지는 것이 중요하다.
  - 즉, 이미 지나간 값은 중요하지 않으며 가장 최근의 값을 얻는 것이 중요하다.

- 정답: UDP
  - UDP를 이용하는 어플리케이션에서는 데이터의 오류, 손실, 중복을 허용할 수 있어야 하며, 데이터의 안정성보다 `시간`의 민감도가 높다.
  - 그리고 현재 센서 통신의 목적은 전체 데이터를 가지고 있는 것이 아니라 가장 최근의 데이터를 가지고 있는 것이 중요하다.
  - 즉, 데이터의 손실이 발생하더라도 더욱 최근의 데이터를 가지고 있는 것이 중요하다.

## Packet 통신 시간 계산

### Queuing delay - 1

- 문제
  - 대기열이 비어있는 Link에 5개의 Packet이 동시에 도착할때 5 Packet의 평균 Queuing delay를 구하시오.
  - 각 Packet의 길이는 L 이며, Link의 전송 속도(transmission rate)는 R 이다.

- 정답: 2 * L/R
  - 5개의 Packet이 동시에 도착한다 했을 때 각 Packet의 Queuing delay는 아래와 같다.
    - 1번째 Packet의 Queuing delay: 0 * L/R
    - 2번째 Packet의 Queuing delay: 1 * L/R
    - 3번째 Packet의 Queuing delay: 2 * L/R
    - 4번째 Packet의 Queuing delay: 3 * L/R
    - 5번째 Packet의 Queuing delay: 4 * L/R
  - 따라서, 5개의 Packet의 평균 Queuing delay는 아래와 같다.
    - 총 Queuing delay: 10 * L/R
    - 평균 Queuing delay: (10 \* L/R) / 5 = 2 \* L/R

### Queuing delay - 2

- 문제
  - 한 packet switch(= router)에 하나의 packet이 절반 정도 전송 중이며, 그 뒤에 두개의 패킷이 기다리고 있다. 이때, 새로 도착한 패킷의 Queuing delay를 구하시오. (msec 단위)
  - 각 packet의 길이: 1,000 bytes
  - Link의 전송 속도: 2 Mbps

- 정답: 10 msec
  - 기다려야 하는 총 Packet의 길이: 20,000 bits
    - 0.5 \* 1,000 + 2 \* 1,000 = 2,500 bytes = 20,000 bits
  - 전송시간: 2 Mbps = 2 \* 10^6 bit/sec
  - Queuing delay: 20,000 / (2 \* 10^6) = 10 \* 10^-3 sec = 10 msec

### Propagation delay

- 문제
  - 다음의 상황에서 Propagation delay를 구하시오. (msec 단위)
    - Packet의 길이 = 2048 bytes
    - Like의 길이 = 4,000 km
    - 전파 속도(Propagation speed) = 2 \* 10^8 m/sec
    - 전송 속도(Transmission rate) = 4 Mbps

- 정답: 20 msec
  - Propagation delay = 4,000 km / (2 \* 10^8 m/sec) = 20 msec

### Total delay

- 문제
  - 다음의 상황에서 Total delay를 구하시오. (msec 단위)
    - Packet의 길이 = 1000 bytes
    - Link의 길이 = 4,000 km
    - 전파 속도(Propagation speed) = 2 \* 10^8 m/sec
    - 전송 속도(Transmission rate) = 2 Mbps

- 정답: 24 msec
  - Transmission delay = 1000 bytes / (2 \* 10^6 bit/sec) = 4 msec
  - Propagation delay = 4,000 km / (2 \* 10^8 m/sec) = 20 msec
  - Total delay = 4 msec + 20 msec = 24 msec
