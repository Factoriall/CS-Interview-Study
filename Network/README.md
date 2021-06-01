# Network

- HTTP 관련
  * [HTTP vs HTTPS](#HTTP-vs-HTTPS)
  * [HTTP 1.1 vs 2.0 vs 3.0](#HTTP-1-vs-2-vs-3)
  * [REST](#REST)
  * [HTTP 응답코드](#HTTP-응답코드)
- [웹브라우저에 구글을 쳤을 때 일어나는 일](#웹브라우저에-구글을-쳤을-때-일어나는-일)
- TCP vs UDP

## HTTP vs HTTPS
### HTTP
![HTTP](../image/network_http.png)
#### 특징
* Application 층의 프로토콜, 서버 및 클라이언트 간의 정보를 주고받기 위한 프로토콜
* 상태를 가지지 않으며(Stateless), Method, Path, Version, Header, Body로 구성된 평문의 Message 형태

#### 문제점
* 평문 통신이기 때문에 경로 상에 **도청**이 가능하다
* 통신 상대를 확인하지 않기 때문에 **위장**이 가능하다
* 수신한 것과 받은 것이 완전히 일치함을 증명할 수 없기 때문에 **변조**가 가능하다

### HTTPS
#### 특징
* HTTP에 데이터 암호화가 추가된 것으로, SSL(Secure Sockets Layer) 또는 TLS(Transport Layer Security)를 추가
* 공개키/개인키 암호화 방식을 이용해 데이터를 암호화

#### 공개키/개인키 암호화
![Key](../image/network_key.png)
- 모두에게 공개되는 **공개키**, 자신만 가지고 있는 **개인키**로 암호화되는 방식
- 공개키로 암호화 시 개인키로만 복호화 가능 -> **안정성 확보**
- 개인키로 암호화 시 공개키로만 복호화 가능 -> **인증 정보 확인 및 신뢰성 확보**

#### HTTPS 동작 과정
![SSL](../image/network_ssl.png)
1. A기업은 HTTP 기반의 애플리케이션에 HTTPS를 적용하기 위해 공개키/개인키를 발급
2. CA 기업에게 돈을 지불하고, 공개키를 저장하는 인증서의 발급을 요청
3. CA 기업은 CA기업의 이름, 서버의 공개키, 서버의 정보 등을 기반으로 인증서를 생성하고 CA 기업의 개인키로 암호화하여 A기업에게 이를 제공함
4. A기업은 클라이언트에게 암호화된 인증서를 제공
5. 브라우저는 CA기업의 공개키를 미리 다운받아 갖고 있어, 암호화된 인증서를 복호화
6. 암호화된 인증서를 복호화하여 얻은 A기업의 공개키로 데이터를 암호화하여 요청을 전송

---
## HTTP 1 vs 2 vs 3
### HTTP 1.1
#### 특징
- Keep-Alive: 한 번 맺었던 연결을 끊지 않고 지속적으로 유지하기 위해 넣는 헤더 정보
- Pipelining: 한 번의 커넥션에 한 방에 여러 순차적인 요청을 연속적으로 보내고 순서에 맞춰서 응답받는 방식

![Pipelining](../image/network_pipelining.png)

#### 문제점
- HOL(Head Of Line) Blocking: 먼저 들어온 데이터들도 앞에 오지 않은 데이터로 인해 대기하는 현상 -> pipelining의 가장 큰 문제점
- RTT(Round Trip Time)의 증가: 매 요청 때마다 커넥션 생성, Keep-Alive의 시간을 넘으면 끊기고 이 때마다 3-way handshaking으로 인한 불필요한 RTT의 증가
- 무거운 헤더 구조: 많은 메타 정보 저장(20bytes), 반복적인 헤더 전송으로 인한 오버헤드 증가

### HTTP 2
![HTTP2](../image/network_http2.png)
#### 특징
- SPDY 프로토콜 기반
- Multiplexted Streams(Multiplexing): 커넥션 하나로 동시에 여러 개의 메세지를 주고받고 응답은 순서에 상관없이 Stream으로 주고 받는 것

![Multiplexing](../image/network_multiplexing.png)
- Stream Prioritization: 받는 자원들의 의존관계를 설정하여 먼저 받을 자원 지정 가능
- Server Push: 클라이언트 요청에 대해 요청하지 않은 부가 리소스도 같이 보내줌

![ServerPush](../image/network_serverpush.png)
- Header Compression: Hpack 압축방식을 이용해 중복 헤더 제거

![Compression](../image/network_compression.png)

#### 문제점
- 여전히 TCP에 대한 HOL Blocking(패킷 loss 발생 시 일어나는 대기 현상)은 해결 안됨

### HTTP 3
![HTTP3](../image/network_http3.png)
- 기존 TCP의 문제점인 HOLB 문제 해결을 위해 TCP 대신 UDP(QUIC) 사용
- 3-way handshaking 필요 없음 -> 1 RTT 연결설정
- 패킷 손실 감지에 걸리는 시간 단축
- HTTP 2와 같은 Multiplexing 사용
- 기존 IP 주소 대신 랜덤 값인 Connection ID 사용 - 기존 연결을 유지하기 쉬움(Wifi -> LTE 전환 등등...)
---
## REST
- 정의: REpresentational State Transfer)의 약자, World Wide Web 같은 분산 하이퍼미디어 시스템을 위한 SW 아키텍쳐의 한 형식

### 특징
1. Uniform: 특정한 인터페이스 사용
2. Stateless: 상태를 따로 저장 및 관리하지 않음
3. Cacheable: HTTP가 가지는 캐싱을 활용 가능
4. Self-Descriptiveness: REST API 메세지만 보고도 쉽게 이해가 가능한 자체 표현 구조로 표현
5. Client-Server: 서버에서 API 제공, 클라이언트는 사용자 인증 및 컨텍스트를 직접 관리하눈 구조
6. Hierachical Structure: 다층 계층으로 구성 가능, 보안 및 암호화 계층 추가 및 네트워크 기반의 중간 매체를 활용 가능

### RESTful 디자인
![REST](../image/network_restful.png)
1. 리소스와 행위를 명시적으로 직관적으로 분리
- URI는 정보의 자원을 표시, 동사 대신 명사로 표시
- 자원에 대한 행위는 HTTP Method로 표현
  + GET: URI가 가진 정보를 검색하고 응답
  + POST: 요청된 자원 생성, 새로 작성된 리소스의 경우 HTTP 헤더 항목에 Location: URI 포함
  + PUT: 요청된 자원을 수정
  + DELETE: 요청된 자원을 삭제 요청
2. 메세지의 경우 Header와 Body를 명확히 분리
- 보낼 Entity 정보는 Body, API 버전 정보 및 응답받고자하는 MIME 타입 등은 Header에 담기
3. API 버전 관리
- 환경에 따라 API 버전 관리, 변경 시 하위호환성을 보장해야 됨
4. 서버와 클라이언트가 같은 방식을 사용해서 요청
- 서버/클라이언트가 둘다 json으로 보내던가 text로 보내던가 하는 식으로, URI가 플랫폼 중립적으로 작동
---
## HTTP 응답코드
- 1XX: Information responses, 서버가 요청을 받았고 클라이언트는 작업을 계속 진행하라는 의미
  + 100 Continue: 진행중
  + 101 Switching Protocol: 업그레이드 요청 헤더에 대한 응답
  + 102 Processing: 요청을 수신하고 처리 중이나 제대로 된 응답을 줄 수 없음
- 2XX: Sucessful responses: 요청 성공
  + 200 OK: 요청을 성공적으로 수행
  + 201 Created: 요청 성공 및 새로운 리소스 생성
- 3XX: Redirection messages
  + 301 Moved Permanently: 요청한 리소스의 URI가 변경됨
- 4XX: Client error responses
  + 400 Bad Request: 잘못된 문법으로 인해 서버가 요청을 이해할 수 없음
  + 401 Unauthorized: 인증된 클라이언트가 아니라서 답할 수 없음, 인증을 해야됨
  + 403 Forbidden: 컨텐츠에 접근할 권리가 없음, 401가 다르게 서버가 클라이언트가 누군지 알고 있음
  + 404 Not Found: 요청받은 리소스(URI)를 받을 수 없음
  + 405 Not Acceptable: 요청 메소드를 서버에서 알고 있지만 제거되어 사용할 수 없음
- 5XX: Server error responses
  + 500 Internal Server Error: 웹사이트 서버에 문제가 발생했음
---
## 웹브라우저에 google.com를 쳤을 때 일어나는 일
![google](../image/network_google.png)
### 1. 웹브라우저에서 일어나는 일
![dns](../image/network_dns.png)

- 1-1) URI 입력 시 도메인 네임을 DNS 서버에서 검색
- 1-2) 검색해서 나온 IP 주소를 찾아서 URI 정보와 같이 HTTP에 송신
- 1-3) 이 정보를 HTTP 프로토콜에 따라 HTTP 요청 메세지 생성
### 2. 프로토콜 스택에서 일어나는 과정
![protocol_stack](../image/network_protocolstack.png)
- 프로토콜 스택(TCP/IP 4-5계층): 운영체제에 내장된 네트워크 제어용 소프트퀘어
- 2-1) 전송(Transport) 계층에서 TCP 프로토콜 따라서 신뢰성 있게 전송을 보장 및 헤더 붙인 세그먼트 형태로 네트워크 계층에 전달
- 2-2) 네트워크(Network) 계층에서 IP 프로토콜 따라 목적지로 데이터를 전송하기 위해 거치는 경로 결정, 이러한 정보가 담긴 헤더를 붙여 패킷(IP 다이어그램) 형태로 전달
- 2-3) 링크(Link) 계층에서 ARP/RARP 프로토콜에 따라 기기 고유의 주소인 MAC 주소를 대응, 관련 정보를 붙인 프레임으로 보냄
- 2-4) 물리(Physical) 계층에서 스위칭 허브를 경유해서 전기 신호로 변환시켜 신호 송출
### 3. 인터넷에서 일어나는 일
- 3-1) IP 프로토콜 따라 얻은 라우팅 경로 따라 목적지로 이동
- 3-2) 도착 후 방화벽이 패킷이 정상적인지 확인
- 3-3) 엑세스 페이지의 데이터가 캐시 서버에 존재할 시 웹서버로 보낼 필요 없이 바로 캐시 서버에서 값을 꺼냄
### 4. 웹서버에서 일어나는 일
- 4-1) 웹서버에 도착 시 웹 서버 쪽의 프로토콜 스택이 패킷 추출 및 메세지 복원
- 4-2) 요청에 따른 데이터를 응답 메세지에 넣어 클라이언트로 회송
- 4-3) 왔던 방식 그대로 응답 메세지가 클라이언트로 전달
