# 2.2 The Web and HTTP

# The Web and HTTP

> 작성자: 조혜원

## 목차

1. HTTP overview

   1-1. HTTP

   1-2. HTTP Connection

   1-3. HTTP Connection : Response time (응답시간/소요시간)

   1-4. Persistent HTTP / connection

1. Maintaining user/server states

   2-1. Cookies

1. Web caches (proxy servers)

   3-1. Web caches (proxy servers)

   3-2. Conditional GET

---

# 1. HTTP overview

### 1. HTTP = Hyper Text Transfer Protocol

Web server와 Web hosts(client) 사이를 연결해주는 protocol

**_HTTP에서 P가 이미 protocol을 의미하지만, HTTP Ptrotocol이라고 한 번 더 말해줄 수 있음_**

**_HT 즉, Hyper Text는 우수한 기능을 내포한 Text를 의미_** <br/> <br/>

**Client / Server**

- **Web Client** : HTTP protocol을 사용해 Web objects를 **request**하고, **receive**하는 browser(application), 그리고 받아온 Web objects를 **display**함
  ⇒ client가 항상 켜져 있는 server에 접속해서 request하고, server의 response를 receive함
- **Web Server** : HTTP protocol을 사용해 Web objects를 **send**함 (**Response**)
  ⇒ 항상 어딘가에 실행 되어 있음 <br/> <br/>

**uses TCP**

절대 loss가 일어나면 안 되기 때문에 **안정적인 (reliable한)** 전송이 필수임! ⇒ **TCP 사용**

1. Client가 TCP connection을 열어서 Server와 연결 ( 여기선 HTTP 연관 X )
2. Server가 TCP connection을 허용함
3. HTTP messages, 요청 내용이 browser (HTTP Client)와 Web server (HTTP Server)에서 전송되고, 후속 작업이 진행 됨

   ⇒ 여기서 HTTP messages는 client와 server 사이에 데이터가 교환되는 방식을 의미

   구성이 정해져 있어, 필요한 정보만 입력하면 자동으로 양식이 완성되어 전송

   response와 request 2가지 메세지 형식이 있음

   ![Untitled](2%202%20The%20Web%20and%20HTTP%20a4f3facde18e45df965fd9878816a0b0/Untitled.png)

   ![Untitled](2%202%20The%20Web%20and%20HTTP%20a4f3facde18e45df965fd9878816a0b0/Untitled%201.png)

   request message는 앞에 metho가 포함 됨 / 요청으로 어떤 method를 원하는지 의미

   ![Untitled](2%202%20The%20Web%20and%20HTTP%20a4f3facde18e45df965fd9878816a0b0/Untitled%202.png)

   response는 protocol과 Response 메세지가 상단에 보내짐

4. 모든 작업과 전송이 완료 되면 **TCP Connection closed** <br/> <br/>

**_HTTP is stateless!!_**

복잡도가 커지기 때문에 state 없이 과거 (history)를 기억하지 않고 매번 새롭게 인식함

과거 기록 (상태)는 server/client에서 유지되어야함

만약 server/client가 충돌하면, 그들의 “상태”관점이 일치하지 않을 수 있어 조화를 이루어야 함

> **이러한 특성 덕에 연결을 맺을 때 발생하는 오버헤드가 줄어들고, 데이터를 빠르고 확실하게 처리할 수 있으며, 요청간의 상태를 공유하지 않으므로 확장성을 가짐**

**_이러한 특징을 갖게 된 이유는, 처음 HTTP는 단순히 HTML 문서만을 주고 받는 것이 목적의전부였기 때문에 최대한 단순하게 설계 되었기 때문이다_**

**_이젠 요청간의 상태가 유지되어야 할 필요가 있어졌지만, HTTP는 유지 할 수 없음. 하지만 우리는 페이지를 이동할 때마다 새롭게 로그인 한 적이 없는데, 이는 상태를 유지해주는 쿠키와 세션이 있기 때문!_** <br/> <br/> <br/>

### 2. HTTP Connection <Br/> <br/>

URL : www.someSchool.edu/someDepartment/home.index <br/> <Br/>

**1a.** HTTP client가 **TCP connection**을 통해 www.someSchool.edu의 port 80번에 있는 HTTP server에 연결함

**1b.** HTTP server는 www.someSchool.edu의 port 80번에서 **TCP connection**을 기다리며, 연결 요청이 올 경우 client에게 알리고 연결을 **수락(accepts)**함

**2.** HTTP clients는 TCP connection socket으로 **HTTP request message**를 보냄

⇒ someDepartment/home.index object를 client가 원하고 있다는 메세지를 담음

**3.** HTTP server는 request message를 받아서 요청 객체 (someDepartment/home.index)를 포함한 **response message**를 작성하여 자신의 socket, 즉 HTTP server의 socket으로 보냄

⇒ 여기 이 단계에서 home.index라는 html object가 전달됨

**4.** HTTP server는 response가 끝났으므로, **TCP Connection을 closes**함

**5.** HTTP client는 요청했던 html file이 담긴 **response message**를 받아서 html을diplay 함

⇒ Web browser가 html을 열어보니, 10개의 jpeg object가 있음

**6.** 1~5의 step을 10개의 jpeg object에 대해 반복함

ex) 1번째 이미지의 위치는 어디고 ~ TCP 연결 ~ 요청~ response ~ close

2번째도 ~~

⇒ Web browser 로딩에 걸리는 시간은 URL을 받아오는 시간 + 이미지 10개를 받아오는 시간 <Br/> <Br/> <br/>

### 3. HTTP Connection : Response time (응답시간/소요시간) <br/> <Br/>

client에서 특정 object를 요청하고, object 수신 완료까지 걸리는 시간 = **Response Time**
<br/><br/>

**RTT (Round-trip time)**

하나의 object를 보내고 응답 받는데 걸리는 시간을 RTT로 표현

즉, 작은 packet 하나를 client에서 server로 보내고, 다시 돌려 받는데 걸리는 시간을 의미

HTTP response time :

![Untitled](2%202%20The%20Web%20and%20HTTP%20a4f3facde18e45df965fd9878816a0b0/Untitled%203.png)

1. 초기 **TCP Connection** ⇒ **1 RTT**
2. HTTP request 및 HTTP response의 처음 몇 bytes를 반환 ⇒ **1 RTT**
3. File transmission time ⇒ 파일 전송 시간 / object의 크기에 따라 시간이 달라짐
4. none-persistent HTTP response time = **2 RTT + file transmission time**

   ⇒ 비지속적인 HTTP 응답 시간 = 2 RTT + file의 마지막 bit를 받는데까지 걸리는 시간

<br/>

**HTTP issues**

1. 각 객체마다 2 RTT를 필요로 함 ⇒ 연결 요청을 위한 TCP 전송 **1 RTT +** HTTP response **1 RTT**

   ⇒ 이렇게 되면, object 요청 후에, 10개의 이미지 파일이 있다는 것을 알게 됐을 때, 10개의 이미지에 대한 각각의 요청, 반응 시간이 소요되고, 각각의 파일들을 전송 받는 시간도 걸리게 됨

   ![Untitled](2%202%20The%20Web%20and%20HTTP%20a4f3facde18e45df965fd9878816a0b0/Untitled%204.png)

2. 이러한 문제점을 위해 browser는 참조된 객체를 가져오기 위해 **TCP connection**을 **parallel (병렬)**로 열게 된다 ⇒ 효율성 극대화

![Untitled](2%202%20The%20Web%20and%20HTTP%20a4f3facde18e45df965fd9878816a0b0/Untitled%205.png)

1. OS는 각 TCP를 연결할 때 오버헤드 (추가비용)이 발생하기 때문에, 보통 5~10개의 TCP 연결을 열고 있음

### 4. Persistent HTTP / connection

1. **non-persistent HTTP issues**:

   (비지속연결 HTTP/1.0)

   - 하나의 TCP 연결에서 최대 **하나의 객체**를 전송함
   - 전송이 끝나면 연결을 닫음
   - 여러 개의 객체를 받아오려면 각 객체마다 2 RTT의 시간이 필요함 ⇒ 불필요한 시간

   ⇒ parallel (병렬)하게 열어도 object를 위한 TCP들이 별도로 존재하고, connection 유지를 위한 자원 (ex. buffer)이 필요함

   시간이 오래 걸릴 수 있음 ⇒ 단점 / 간단함 ⇒ 장점

2. **persistent HTTP**

   (지속연결 HTTTP/1.1 이후)

   - Server는 응답을 보낸 후에도 **연결을 열어둠**
   - Client는 웹 페이지를 로드하는 동안 참조된 객체를 만나면 즉시 Server에 요청을 보냄 ⇒ 객체를 받아오는 동안 추가적인 지연이 발생하지 않음
   - 하나의 TCP 연결로 **여러 객체를 전송** 할 수 있음
   - HTTP server는 일정 시간동안 요청이 없으면 TCP 연결을 닫음 (**Timeout**)

⇒ 이러한 차이점과 기능은 웹 페이지 및 리소스 로딩 성능에 영향을 미침

<br/>

**HTTP response Status Codes**

![Untitled](2%202%20The%20Web%20and%20HTTP%20a4f3facde18e45df965fd9878816a0b0/Untitled%206.png)

1. **200 OK (요청이 성공함):**
   - 요청이 성공적으로 처리되었으며, 요청한 객체는 이 응답 메시지의 내용으로 포함되어 있습니다.
2. **301 Moved Permanently (영구적으로 이동함):**
   - 요청한 객체가 새로운 위치로 영구적으로 이동했음을 나타냅니다. 새로운 위치는 이 응답 메시지의 "Location" 필드에 지정되어 있습니다.
3. **400 Bad Request (잘못된 요청):**
   - 서버가 요청 메시지를 이해하지 못했거나 처리할 수 없는 형식의 요청이 들어왔음을 나타냅니다.
4. **404 Not Found (찾을 수 없음):**
   - 요청한 문서나 객체가 서버에서 발견되지 않았음을 나타냅니다. 즉, 요청한 리소스가 서버에 없음을 의미합니다.
5. **505 HTTP Version Not Supported (지원되지 않는 HTTP 버전):**
   - 서버가 요청한 HTTP 버전을 지원하지 않음을 나타냅니다. 클라이언트가 요청한 HTTP 버전이 서버에 지원되지 않을 때 발생합니다.

---

# 2. Maintaining user/server states

### 1. Cookies

되짚어 보기 : HTTP GET/response는 **stateless**

client가 server에 request할 때, 이전에 어떤 request와 response가 있었는지 참조하지 않고 그 순간에 온 request에만 집중 ⇒ server는 수많은 TCP connection에 대해 HTTP Request를 **동시에 처리(computation)**해야 하기 때문!!

하지만, 웹 사이트는 User의 과거 기록을 무시하지 않고, User별로 다른 형태의 웹사이트를 제공해야 할 경우가 있음 ex) 권한 별로 달라지는 UI, UX ⇒ Stateless한 HTTP는 이런 사용자 맞춤형 정보 제공을 할 수 없음

이를 위해 Cookies 필요!

<br/>

**User-Server State : Cookies**

4가지 구성요소

1. **cooke header line of HTTP response message**

   (HTTP 응답 메세지의 쿠키 헤더 라인)

   웹 서버는 HTTP response protocol 안의 header에 쿠키 정보를 포함 시킴 → 클라이언트에게 할당된 쿠키의 식별자 및 기타 데이터가 포함됨

2. **cookie header line in next HTTP request message**

   (다음 HTTP 요청 메세지의 쿠키 헤더 라인)

   client는 이전 server로부터 받은 쿠키 정보를 다음 HTTP 요청 메세지의 헤더 라인에 포함 시킴 → 이를 통해 **server는 client를 식별하고, 개별화된 경험 제공**

3. **cookie file kept on user’s host, managed by user’s browser**

   (사용자 호스트에 저장된 쿠키 파일 / 브라우저에 의해 관리됨)

   client의 **web browser(host)** 는 쿠키 파일을 **유지, 관리함**

   알아서 다음 Request에서 이 쿠키 파일의 정보에서 사용하기 위함 (2번 요소)

4. **back-end database at web site**

   (웹 사이트의 백엔드 데이터 베이스)

   웹 사이트는 client에 할당한 각 **쿠키**에 대한 정보를 **백엔드 데이터 베이스**에 **저장**

   이 정보를 사용하여 client 식별, 맞춤형 서비스 제공

example :

1. Susan이 처음으로 특정 전자 상거래 사이트를 방문
2. Server는 Susan에 대한 고유한 쿠키 식별자를 생성, DB에 저장
3. 이후의 요청에서 쿠키를 서버로 다시 전송하여 Susan을 식별하고 사용자 정의 서비스 제공

**Cookies : Keeping “state”**

![Untitled](2%202%20The%20Web%20and%20HTTP%20a4f3facde18e45df965fd9878816a0b0/Untitled%207.png)

---

# 3. Web caches (proxy servers)

### 1. **Web caches (proxy server)**

data가 이동하는데 걸리는 시간이 크기 때문에 CPU 안에 memory 장치를 두고, 한 번 읽어온 data는 또 사용할 것 같다고 판단하여 caches에 저장

> **목적 : Client 요청을 원본 Server를 거치지 않고 만족 시키기**

1. user sets browser : Web accesses via cache

   proxy server를 사용하겠다는 설정이 필요

   자신의 브라우저를 웹 캐시를 통한 웹 자원에 접근하도록 설정

1. browser sends all HTTP requests to cache

   브라우저는 모든 HTTP 요청을 웹 cache에 보냄

   - object in cache : 객체가 캐시에 있으면, 캐시가 객체를 반환 ⇒ **cache hit**
   - else : 그렇지 않으면, 캐시가 원본 서버에 객체를 요청하고, 그 후에 클라이언트에게 객체 반환 ⇒ **cache miss**

   ![Untitled](2%202%20The%20Web%20and%20HTTP%20a4f3facde18e45df965fd9878816a0b0/Untitled%208.png)

**Web caches와 proxy server는 같은 개념!**

- **Web cache acts as both client and server**

  (client와 server 역할 둘 다 함)

  1. server에게 request 하기 때문에 client의 역할을 함
  2. server의 response를 저장해 뒀다가, client에게 반환하기 때문에 server 역할을 함
     <br/>

- **typically cache is installed by ISP**

  (일반적으로 웹 cache는 ISP(인터넷 서비스 제공자 /KT, SKT, 대학교, 기관)등에 의해 설치 되고, 중앙에서 관리됨)

<br/>

- **Why Web caching?**
  (Web caching의 목적?)

  1. **reduce response time for client request**

     사용자 관점 : response time을 파격적으로 줄일 수 있음

     - cache is closer to client ⇒ ISP 내에서 관리하기 때문에 cache는 client에 가까이 위치해 있음 ⇒ 요청에 빠르게 응답 가능
       <br/>

  2. **reduce traffic on an institution’s access link**

     access link를 관리하는 기관의 traffic을 줄일 수 있음 ⇒ ISP에 많은 사람이 몰릴 수 도 있는데, 첫 번째만 origin으로 가고, 나머지는 내부 cache에서 처리
     <br/>

  3. **Internet is dense with caches**

     인터넷은 캐시로 가득 차 있음

     - enables “poor” content providers to more effectively deliver content

     ⇒ Internet (전체 network) 관점 : proxy server는 매우 일상적으로 퍼져있고, 촘촘히 설치 되어 있음 ⇒ 콘텐츠 제공자들이 더 효과적으로 콘텐츠를 전달 할 수 있고, 성능이 부족한 콘텐츠 제공자도 더 효과적으로 콘텐츠를 제공할 수 있게 됨

### 2. **Conditional GET**

<br/>

cache 사용의 문제점 ⇒ **Data 순결성**

첫 요청과 나중 요청 사이에 contents가 바뀌면 새로운 contents를 받지 못 함

⇒ HTTP Protocol에 특정 field를 둬서 contents 변경 여부 확인
<Br/>

> **목적 : cache version이 update 되었으면 object를 전송하지 않음**

효율적으로 과거 자원 재활용 위해 HTTP Protocol 사용!

![Untitled](2%202%20The%20Web%20and%20HTTP%20a4f3facde18e45df965fd9878816a0b0/Untitled%209.png)

![Untitled](2%202%20The%20Web%20and%20HTTP%20a4f3facde18e45df965fd9878816a0b0/Untitled%2010.png)
