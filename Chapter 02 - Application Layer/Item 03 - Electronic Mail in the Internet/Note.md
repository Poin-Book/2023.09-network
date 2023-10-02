# Electronic Mail in the Internet 
> 작성자: 심세원

## 목차


<br>

## Email

→ Asynchronous Communication

비동기적인 통신 수단

<br>

## Email의 Major Components

### 1. User Agent

> 이메일을 작성하고 보내고, 관리하고, 회신하는 등의 역할
> 

*(실제적 서비스인 비즈니스 로직이 구현되어 있는 부분이라고 생각됨)*

<br>

### 2. Mail servers

> 인프라 스트럭처의 역할
> 

[IT 인프라란 - 개념, 운영, 구조, 보안 및 관리](https://www.redhat.com/ko/topics/cloud-computing/what-is-it-infrastructure)

**메일 서버가 하는 일**

1. 유저가 메일을 발송
2. 메일 서버로 저장
3. 수신자로 해당하는 메일 서버로 발송
4. 메일 박스에 수신 된 메일을 담아 놓음

**구성요소**

| message queue | 발송을 위한 메시지 큐 |
| --- | --- |
| mailbox | 도착하는 메시지를 담고 있는 메일 박스 |
| SMTP protocal | 메일 서버끼리 통신할 때 쓰는 프로토콜 |

<br>

## SMTP

> 메일 서버끼리 통신할 때 쓰는 프로토콜
> 
- TCP 통신을 함 (손상이 없어야 하므로)

1. 각각의 메일 서버 어플리케이션은 수신하기 위한 포트를 열어 놓음
2. 수신자의 도메인 주소를 보고 연결을 요청함 
    
    (동작을 잘 안 하면 연결을 주기적으로 맺으려 시도함)
    

**SMTP 절차**

1. 연결 과정
2. 메일 전송에 필요한 절차
3. 연결을 끊기

<br>

## SMTP vs HTTP

| HTTP | SMTP |
| --- | --- |
| Pull protocol (클라이언트가 서버로 Pull) | Push protocol (클라이언트가 서버로 Push) |
| TCP connection | TCP connection |

둘 다 ASCII 코드로 이루어진 command/response interation를 가지고 있음

상태 코드(status codes)가 비슷함

<br>

## Mail access protocol

![Untitled](SMTP%2009cb9f41573f4624a8298f71e5dc0036/Untitled.png)

메일을 보내는 절차

`sender’s UA(user agent)` → `sender’s mail server` → `receiver’s mail server` → `receiver’s UA(user agent)`

**option 1으로 했을 때**

`sender’s UA(user agent)` → `sender’s mail server` → `receiver’s UA(user agent)`

- `receiver’s UA(user agent)` 가 메일을 받기 위해 항상 네트워크로 접속할 수 있는 환경이 되어야 함

**option 2으로 했을 때**

`sender’s UA(user agent)` → `receiver’s mail server` → `receiver’s UA(user agent)`

- `receiver’s mail server` 의 이슈에 대해 `sender’s UA(user agent)` 가 대응하기 어려움
    
    *ex) 수신자 메일 서버가 연결이 안되거나, 용량이 넘치거나…*
    

<br>

## Mail box에서 메일을 읽어오는 법

### 1. POP3 (Post Office Protocol)

메일 서버에  UA가 엑세스해서 인증 과정을 거치면, 클라이언트가 요청할 수 있게 됨 

- 클라이언트가 요청한 것에 대해 `OK` `ERR` 로 응답
- 메일 박스에서 읽고, 삭제할 수 있음

### 2. IMAP (Internet Mail Access Protocol)

POP3에서 기능이 좀 더 추가됨

### 3. Wed-based Email

HTTP를 이용해 메일 박스에 있는 것을 읽어옴

<br>

## Example

1. 앨리스가 밥에게 메일을 보냄
    
    밥의 메일 주소 : bob@naver.com
    
2. 앨리스는 메일을 작성한 뒤 보내기 버튼을 누름
3. 앨리스의 메일 서버로 해당 메일이 보내짐
    - 유저 에이전트가 앨리스 서버와 TCP 연결을 해서 보냄
    - SMTP 프로토콜 사용
4. 앨리스 서버는 밥의 서버로 접속을 해서 메일을 딜레이 걸어놓음
    - TCP 연결
    - SMTP 프로토콜 사용
    - 밥의 서버 ip 주소는 DNS를 이용해서 찾음
5. 밥의 메일 서버로 전송됨
6. 밥의 메일 박스로 메일을 위치시킴
7. 밥은 메일이 있는지, 유저 에이전트를 이용해서 확인하면 전송된 메일을 띄움
    - HTTP 프로토콜 사용