## 용어정리
  - **URL(Uniform Resource locator)** : web 상의 자원이 존재하는 위치를 가리키는 주소이다.
    - **URL = server host 이름 + object의 경로 이름**   
  - **RTT(Rounding-trip time)**
    - **time for a small packet to travel from client to server and back**
    - **total nodal delay를 포함한다!**
    - initiate request ~ accecpt request ~ get response to request = 1RTT
    - request messages ~ accept request ~ get response to request = 1RTT



# Web   


## 왜 World-Wide-Web 인가?   

![www](https://user-images.githubusercontent.com/59442344/111038538-fb287880-846c-11eb-96ad-68214c4aad10.jpg)

  - web상의 page들이 거미줄처럼 엮여 있기 때문!   


## Web Page의 구성   

  - **HTML file** + **reference objects (참조 객체들)**
    - **HTML** : HyperText(즉시 찾아갈 수 있게 엮인 text) + MarkUp(그 text들을 표현하는) + Language(언어) 
    - **object 종류** : JPEG image, java applet, audio file ...
  
  - HTML file 자체와, reference object 들은 각각 모두 **URL로 접근 가능**하다.
  
  - **URL = server host 이름 + object의 경로 이름**
![url](https://user-images.githubusercontent.com/59442344/111038741-eac4cd80-846d-11eb-99cf-91a1345ccf98.jpg)   


## Web Browser, Web Server

  - **Web Browser**
    - HTTP의 client측을 구현한다.
    - 요구한 Web Page를 보여준다.
    - **HTTP 요청 메시지를 server로 보낸다. (-> 그 과정에서 TCP 프로토콜을 사용)**

  - **Web Server**
    - URL로 지정할 수 있는 Web 객체들을 갖고 있다.
    - **HTTP 응답 메시지를 client로 보낸다. (-> 그 과정에서 TCP 프로토콜을 사용)**



# HTTP

## HTTP는 ?
  - **HyperText transfer protocol : 하이퍼텍스트 전송에 관한 규칙을 담고 있다.**
  - **Web's application layer protocol : application layer protocol이다.**
  - **비상태 프로토콜(stateless protocol)이다.**
    - server에서 browser의 state에 대해서 관심이 없다. 

## TCP connection (HTTP가 어떻게 전송되는가?)
  - **HTTP로 만들어진 message를 socket을 통해서 TCP를 이용해서 전송한다.**
    - **socket interface를 지나는 순간 TCP에게 인도된 것이고, 나머지 과정들은 TCP가 처리한다.**
    - ex) 서버에 도착하는 일, 데이터가 무결하게 전달되는 사항들..

### 3가지 TCP connection 방식

![tcp connection](https://user-images.githubusercontent.com/59442344/111039297-996a0d80-8470-11eb-9e8b-0e1e19b1bbe2.jpg)

### 1. non-persistent connection
  - Requires 2 RTTs per object
  - OS overhead for each TCP connection
  - Browsers often open parallel TCP connections to fetch referenced objects
  - connection마다 최대 1개의 객체를 보내고 받는다.
  - 여러 web page 객체를 받는데 소요되는 시간
    - initiate request ~ accecpt request ~ get response to request = 1RTT
    - request messages ~ accept request ~ get response to request = 1RTT
    - file receiving time
    - n * (2RTT + file receiving time)
  - 단점
    - 각 요청 객체들에 대한 새로운 연결들이 설정되고 유지되어야 한다. (TCP buffer의 할당과, TCP 변수들의 클라이언트, 서버에서의 유지)
    - 각 객체가 2RTT를 요구한다! (시간 많이 소요)

![nonpersistenthttp](https://user-images.githubusercontent.com/59442344/111039833-7ee56380-8473-11eb-9e22-bce2cf8e0f3a.jpg)

### 2. persistent connection
  - Server leaves connection open after sending response
  - Subsequent HTTP messages between same client/server sent over open connection
  - Client sends requests as soon as it encounters a referenced object
  - As little as one RTT for all the referenced objects
  - connection을 유지한채로 같은 연결을 통해서 요청과 응답을 보내고 받는다.
  - 여러 web page 객체를 받는데 소요되는 시간
    - initiate request ~ accecpt request ~ get response to request = 1RTT
    - request messages ~ accept request ~ get response to request = 1RTT
    - file receiving time
    - 1RTT + n * (1RTT + file receiving time)

### 3. persistent connection (pipelining)
  - pipelining은 요청에 대한 응답을 기다리지 않고, 요청을 계속 받을 수 있는 시스템이다.
  - 매우 짧음.


