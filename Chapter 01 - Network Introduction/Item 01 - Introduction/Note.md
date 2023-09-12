# Introduction

> 작성자: 김나현

## 목차

1. 네트워크(인터넷)란?
2. 직접 연결과 간접 연결
3. 패킷이란?
4. Internetwork
5. 컴퓨터 네트워크

## 1. 네트워크(인터넷)란?

- 서로 연결된 컴퓨터와 다른 하드웨어 구성 요소들의 집합
- 물리적이든, 논리적이든 특별한 하드웨어와 소프트웨어를 사용함
- 컴퓨터들이 통신 기술을 이용하여 그물망처럼 연결된 통신 이용 형태
- 서로의 리소스, 정보를 공유/교환할 수 있게 함
- 노드는 링크로 연결되는데, 이 때 노드가 컴퓨터들이고, 노드를 연결하는 것이 링크임
  > _노드_: 서버, 라우터, 스위치 등 네트워크 장치
  > _링크_: 유선 또는 무선

## 2. 직접 연결과 간접 연결

### Direct Links

![](https://steemitimages.com/1280x0/https://cdn.steemitimages.com/DQmRgc41Fc11fjzc6BfDYXdD1aRFeZ2XaepstBVhH8TLhct/Screen%20Shot%202018-07-09%20at%2012.08.09%20AM.png)
물리적 링크는 (a)처럼 한 쌍의 노드로 제한되기도 하고, (b)의 사진처럼 두 개 이상의 노드가 단일 링크를 공유할 수 있다.

- (a): 접대점 (Point-to-point) 링크
- (b): 다중접속 (Multiple-access) 링크

Node는 Host라고도 부르며, 애플리케이션 프로그램을 실행시키거나 패킷(데이터의 블록)을 다른 노드에 봬기도 한다.

### Indirect Links

> 교환망 (Switched network)

![](https://steemitimages.com/1280x0/https://cdn.steemitimages.com/DQmZjH63KFr3ChMkqSyBEUFppFtAL1Qcs9gmXr2Ufz4D9Ku/Screen%20Shot%202018-07-09%20at%2012.10.33%20AM.png)

- Switch의 역할: 패킷을 저장하고 전달하는 역할

### 3. 패킷이란?

- 쉽게 말하면 데이터의 조각
- 데이터를 한 번에 보내면 데이터 중 일부만 오류가 나더라도 그 데이터 전체를 다시 보내야하는데, 이 때문에 데이터를 조각으로 나눠서 여러 번 보냄
- 패킷은 header에 출발지와 목적지 등의 데이터를 가지고 네트워크로 보내짐
- 클라이언트에서 패킷에 출발지와 목적지를 적고 인터넷에 실어보내면 인터넷상의 노드를 통해 서버까지 데이터가 전달이 되게 된다
  ![](https://asec.ahnlab.com/wp-content/uploads/tistory/0156_157a59464e9e64020e.jpg)
  패킷은 4개의 레이어 모델로 전체 구조를 갖고 있으며 이 규칙에 맞추어 각 단계별로 header 정보가 붙는다

## 4. Internetwork

inter-network 혹은 internet

- 네트워크를 연결하는 네트워크
- 비슷한 네트워크들끼리 routers라는 장비를 통해 연결됨

`Router`: 네트워크를 확장하고, 패킷이 어느 길로 가야하는지 찾아주는 역할을 하는 장비
`Routing`: 목적지를 식별하고 데이터가 어떤 길로 가야할지 찾는 것. Switch와 Router가 하는 일

## 5. 컴퓨터 네트워크

컴퓨터 네트워크의 목적: `보편적인 전파(Universal Communication)`. any to any라고도 할 수 있다.

### Any-to-Any Connectivity

: 인프라를 공유

- Swtich와 Router 같은 중간 노드들은 `multiplexing`을 통해 호스트가 인프라를 공유할 수 있도록 한다.
  ![](https://velog.velcdn.com/images/metamong/post/e8071b20-70c5-446d-827f-1ff2cc8ff468/image.png)

### Multiplexing

: 하나의 링크에서 여러 데이터가 지나갈 수 있도록 하는 것

- 방식으로는 `TDM(Time Division Multiplexing)`, 시분할 방식이 있다. 이는 리소스를 시간 단위로 나눠갖는 것이다.

### Protocol

- 사전적 의미: 규칙, 규약
- `네트워크 프로토콜`: 컴퓨터 디바이스 사이의 규칙
- 인터넷에서의 모든 의사소통 활동은 프로토콜에 의해 통제됨
  > 프로토콜은 네트워크 엔티티 사이에서 송수신되는 메시지의 형식, 순서, 메시지 전송, 수신에 대한 조치를 정의
