# 1.4 Network Performance

**학습 목적**

앞에서 배운 **Packet Switching**에 대해 자세히 알기 위해 **성능** 관점에서 봐야 함

**학습 목표**

네트워크 시스템에서의 성능을 이해 하기

### 목차

1. Understanding Performance
2. How do loss and delay occur?
3. Queueing Delay, Packet Loss
4. Four Sources of packet delay
5. Queueing Delay
6. “Real” internet delays and routes
7. Packet loss
8. Throughput
9. Tramsmission vs. Propagation delay

---

## 1. UnderStanding Performance

- **Algorithm**
  - 알고리즘에서의 성능은 N개의 input이 있을 때, 실행 횟수를 표현
  - ex) input : N → Performance : $O(N^2)$
- **Processor and memory system**
  - 하드웨어의 경우, 프로세스가 얼마나 빠른가, 초당 몇 번의 연산을 할 수 있는가 (Hz 헤르츠)
  - 메모리 시스템의 경우, 얼마나 많은 용량을 얼마나 빠른 access time으로 메모리를 읽어오는가를 가지고 성능을 나타냄
- **Network System** : 통신을 하기 위한 시스템
  - 네트워크 시스템에서의 주된 성능 지표3가지
    - **Delay : 지연시간**
      - Packet이 하나의 network를 가로지를 때 걸리는 시간
    - **Throughput : 처리율**
      - 단위 시간 동안 송수신 되는 data의 양 (bps) / bit 단위
    - **Loss : 손실률**
      - host로 보내지는 100개의 packet당 잃는 packet의 수
      - ex) 10000개의 bit를 보낼 때, 1개 정도의 손실이 있음 → 1/10000 손실률

---

## 1.4 delay, loss, thorughput in netwrok

**학습 목표**

- delay, loss가 왜 발생할까?
- end-end system에서 어떻게 성능이 정해지는가?

---

### 2. How do loss and delay occur?

Packet Switching의 핵심인 router에 대해 알아야 이해 가능

**Router** → 전송 받은 data들을 목적지 (destination)까지 전달 → **Store + Forward**

loss (손실)과 Delay(지연)은 어떻게 발생하는가?

- 링크에 대한 packet arrival rate(도달률)이 output link capacity(출력 링크 용량)을 초과 함
- 초과된 packet은 queue에 저장되고, turn을 기다림

![Untitled](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled.png)

- queue가 비워지지 않고 계속 들어오게 되면, 너무 많이 쌓여서 문제가 생길 수 있음 ⇒ 일정 **buffer size**를 정해 놓음 → **buffer size**를 넘겨서 들어오는 packet은 버림 (**drop**) ⇒ 그 packet은 **loss** (손실)됨

---

### 3. Queueing Delay, Packet Loss

- **Queueing delay (지연)**

  - 순간적으로 10개의 packet이 **“동시에”** 도착했다면?
    - **router buffer**는 **empty** 상태
    - 처리율 = R = 1.5 → 1초에 1.5개의 packet을 전송 할 수 있는 속도
  - 1st packet은 delay 없이 바로 전송 가능

    - 2nd packet → 1개의 packet이 전송 되는 시간을 기다림
    - 3rd packet → 2개의 packet이 전송 되는 시간을 기다림

           ⇒ **delay 시간 누적**

    - 10th packet은 다른 9개의 packet이 전송 될 때까지 기다림.

  - 뒤에 도착한 packet일수록, 즉, queue에 저장된 packet은 delay 시간이 누적됨

- **Loss (손실)**
  - **router buffer**가 **full** 상태라면?
  - 다음에 도착하는 packet은 **drop**됨 → **packet loss** 발생

⇒ **Traffic intensity (트래픽 강도)가 커지면 Queueing delay가 커지고, 손실이 발생하여, 손실률(loss)이 커짐**

---

### 4. Four sources of packet delay

![1. processing delay    2. queueing delay    3. transmission delay    4. propagation delay](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%201.png)

1. processing delay 2. queueing delay 3. transmission delay 4. propagation delay

![Untitled](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%202.png)

1. $**d_{proc}$ : processing delay\*\*

   - Link를 통해 packet 도착
   - packet bit error 여부 확인
   - header의 IP 주소로 가기 위해 자신의 output link 중 어느 곳으로 가야 할지 결정

   → 까지 걸리는 시간 = **processing delay** / 엄청 빨리 이루어짐

   - msec → 마이크로초 단위로 발생

2. $**d_{queue}$ : queueing delay\*\*
   - 가장 흥미롭고 복잡한 delay
   - processing delay가 끝나면 바로 queueing delay
   - **output link로 전송 되는 것을 기다리는 시간**
   - router congestion level (router 혼잡도)에 따라 시간이 달라짐
3. $**d_{trans}$ : transmission delay (전송 지연)\*\*

   - 모든 packet의 bits를 물리적인 매체인 link로 전송 시켜줌
   - L bits의 packet을 전송 시키는데 걸리는 시간
   - 0101로 이루어진 packet의 시퀀스, bit를 물리적인 link로 변환 시킴 → network interface가 하는 일
   - 한 packet이 첫 번째 bit부터 마지막 bit까지 router를 빠져 나가는 시건
   - $d_{trans} = L/R$
     ![Untitled](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%203.png)
     - L : packet length (bits) / packet의 길이
     - R : link bandwidth (bps) / 해당되는 link의 bps (속도)
       _rate는 전송속도(trasmission rate)로, link bandwidth, link capacity 등과 같은 의미다._
       _size는 패킷 사이즈, 즉 패킷의 크기다. rate와 size가 transmission delay를 결정한다._
       **\*[출처]** [전송지연과 전파지연 - Transmission delay vs Propagation delay](https://blog.naver.com/sepaper/221753583844)|**작성자** [sepaper](https://blog.naver.com/sepaper)\*
   - packet size가 커지거나, bandwidth가 좁아지면 전송 시간이 길어짐

4. $**d_{prop}$ : propagation delay (전파 지연)\*\*

   - A route에서 B router로 이동하는데 걸리는 시간
   - $d_{prop}$ $= d/s$
     - d : length of physical link / link의 물리적 길이
     - s : propagation speed in medium (~$2\times 10^8m/sec$) - 빛의 속도와 비슷
   - _출발지와 도착지 사이 거리가 멀면 멀수록, 랜선이 아니라 속도가 느린 구리선을 쓴다면 "전파"되는데 오래 걸릴 것이다._
     **\*[출처]** [전송지연과 전파지연 - Transmission delay vs Propagation delay](https://blog.naver.com/sepaper/221753583844)|**작성자**[sepaper](https://blog.naver.com/sepaper)\*

   ***

   ### 5. Queueing Delay

   - R : link bandwidth (bps) → **고정값 : 초당 내보낼 수 있는 용량 R bits**
   - L : packet length (bits)
   - a : average packet arrival rate → 평균 packet 도착률
   - Traffic intensity = La / R

   1. ${La \over R}$ ~ $0$ : avg.queueing delay small

   ⇒ traffic이 작다. → queueing delay가 작아짐

   1. ${La \over R}$ → 1 : avg.queueing delay large

   ⇒ traffic이 크다. → queueing delay가 exponential(지수형태)로 커짐

   1. ${La \over R}$ > 1 : average delay infinite!

   ⇒ 불안정 상태. 계속 packet이 loss되는 상태로 무한 delay 발생

   **존재할 수 없는 상태 / 제기능 X**

   ![Untitled](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%204.png)

   ![Untitled](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%205.png)

   - ${La \over R} \approx 0$
     같은 0.05라도 bursty (몰려서 들어옴)에 따라 delay가 달라짐
     bursty → queueing delay 발생
     bursty X → delay 없이 바로 바로 나감
     ex ) 10분에 1명씩 → delay 없음 / 30분에 3명씩 동시에 → delay 발

   ***

   ### 6. “Real” Internet delays and routes

   - **ping**

     - traceroute program과 대조되는 프로그램
     - ping을 이용해서 delay의 총합을 알려줌

   - **traceroute program ( Window → tracert)**

     - 각 라우터까지 가는데 걸리는 delay를 전부 계산해서 보여줌
       ![Untitled](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%206.png)
     - 몇 개의 router를 거치고, 각각의 router들이 어느 정도의 delay를 겪는지 보여줌
       ![Untitled](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%207.png)

     ***

     ### 7. Packet loss

     - router buffer는 finite capacity (유한한 용량)을 가짐
     - packet이 full queue에 도착하면, **drop 됨 (lost)**
       ![Untitled](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%208.png)
     - delay가 특정 ms 속도를 넘지 않게 하기 위해 링크의 속도에 맞춰 buffer size를 잡음

     ***

     ### 8. Throughput

     - **Throughput (처리율)**
       ![Untitled](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%209.png)
     - 단위 시간 동안 sender와 receiver 사이에 얼마 정도의 bit를 전송할 수 있는가?

     - **instantaneous** : 특정한 시간에 얼마나 나왔는지 → 순간 throughput
     - **average** : 평균 throughput → 특정한 data. 전체 file 보낼 때 걸리는 시간
       ![Untitled](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%2010.png)
     - server가 F bits의 file을 여러개의 link를 통해 client로 보낼 때, Link bandwidth가 달라질 수 있음
     - $R_s = R_c$ 일수도 있지만, 차이가 생긴다면, end에서 end로 보내는 throughput에 어떤 영향을 미칠까?
       1st case. $R_s < R_C$
     - 무엇에 의해 average end-end throughput이 결정될까?
       ![bottleneck link = $R_S$ : server쪽 link의 bandwidth가 좁음](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%2011.png)
       bottleneck link = $R_S$ : server쪽 link의 bandwidth가 좁음
       2nd case. $R_s>R_C$
     - 무엇에 의해 average end-end throughput이 결정될까?
       ![bottleneck link= $R_C:$ client 쪽 bandwidth가 좁음](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%2012.png)
       bottleneck link= $R_C:$ client 쪽 bandwidth가 좁음
       > Bottleneck link - 병의 목 모양 생각하기
       > → 폭이 작은 link에 의해 결정!

     ### Throughput : Internet scenario

     ![Untitled](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%2013.png)

   ***

   ### 9. Transmission vs. Propagation Delay

   전송 vs 전파 지연

   **Tansmission delay** : Packet이 하나의 router를 빠져 나가는데 걸리는 시간

   - $d_{trans} = L/R$
     - 링크의 packet 길이 (L bits)와 전송 속도 (R bps)의 함수
   - 두 router 사이의 거리와 무관

   **Propagation delay** : Packet이 A router에서 B router로 이동하는 데 걸리는 시간

   - $d_{prop} = D/S$
     - 두 라우터 사이의 거리 함수
   - packet 길이나 link 전송률과 무관

   ### Caravan Analogy - 고속 도로 상황에 비유하기

   ![Untitled](1%204%20Network%20Performance%20a3034754dca14213be65c6c57ded1002/Untitled%2014.png)

   - cars “propagate” at $100km/hr$ : 차량 한 대가 진행하는 속도→ **Propagation delay**
   - toll booth takes 12 sec to service car (bit transmission time) : 톨게이트에서 차량 한 대가 딱 넘어갈 때 12초 소요→ **Transmission delay**
   - 마지막 차량이 톨게이트를 통과하는 데에 120초 소요 → **Queueing delay**

   - 전체 10개의 차량이 첫 번째 톨게이트를 통과할 때까지 걸리는 시간은 12\*10=120초이니, 120초 이후에 첫 톨게이트를 다 지나게 됨 → **Transmission delay**
   - 시속 100km로 달리니까, 마지막 차량이 두 번째 톨게이트에 도착할 때까지 걸리는 시간은 1시간 = $100km / (100km/hr) = 1hr$ → **Propagation delay**
   - 따라서 **62분** 후면, 두 번째 톨게이트에 도착
