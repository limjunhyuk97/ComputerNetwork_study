## 용어정리
  - **network application (네트워크 애플리케이션)**
    - network를 사용하는 응용시스템들을 아우르는 말. 
    - network application은 end-system들에서만 작동한다. 
    - e-mail, web, text messaging, p2p file sharing, multi-user network games, Voice over IP, social networking, search ...
  - **peer**
    - service provider가 제공하지 않고, 사용자들이 제어하는 desktop이나, laptop들을 말한다. 
  - **self-scalability (자가 확장성)**
      - new peers bring new service capacity, as well as new service demands
      - peer들이 데이터를 요구하면서 작업 부하를 만들지만, 나누게되면 서비스 능력이 추가된다.
  - **message exchange (메시지 교환)**
    - 서로 다른 end-system 간의 process 통신은 message exchange를 통해 이뤄진다.
  - **socket interface (소켓 인터페이스)**
    - Process sends/receives messages to/from its socket
    - process에서 network로 message가 빠져나가고 들어오는 문이다.
    - 정확하게는, **application layer와 transport layer간의 interface 이다.**
  - **loss-tolerant application (손실 허용 애플리케이션)**
    - 어느 정도의 데이터 손실을 감내할 수 있는 실시간, 저장 오디오/비디오 멀미티디어 애플리케이션.     



# Network Application principle

## 1. network application architecture

  - **network application을 설계할 때, 염두해둘만 한 우수한 2가지 구조!**

![client - server, p2p](https://user-images.githubusercontent.com/59442344/111036113-e8a84200-8460-11eb-8455-35ee4d3ade0f.jpg)

  - **client - server architecture (클라이언트 - 서버 구조)**
    - **server에 의존적이다.** 
    - **client**
      - may be intermittently connected (종종 연결됨)
      - may have dynamic IP address (여러개의 IP 주소 가능)
      - communicate with server, do not communicate directly with each other (무조건 서버 통해서 연결!)
    - **host**
      - always-on host (항상 켜져있음)
      - Permanent IP address (단일 IP 주소 가능)
      - Data centers for scaling  (확장 위해서 data center를 운영 가능)   
  
  - **peer - to - peer architecture (P2P 구조)**
    - **서버에 최소로 의존하거나, 아예 의존하지 않는다.**
    - **peer**
      - arbitrary end systems directly communicate (임의의 종단들끼리 연결)
      - peer request(client process) and provide(server process) service (peer들끼리 서로 주고받음) 
      - intermittently connected and change IP address (peer들은 종종 연결되고, IP 주소 변경 가능)
    - **self-scalability (자가 확장성)**
      - new peers bring new service capacity, as well as new service demands
      - peer들이 데이터를 요구하면서 작업 부하를 만들지만, 나누게되면 서비스 능력이 추가된다.   

  - **hybrid architecture (하이브리드 구조)**
    - server에서 user의 IP를 추적만하고, data는 user들끼리 주고받는 경우의 architecture 

## 2. process 간 통신
  
  - **process란?**
    - **program running within a host (실제 동작중인 end-system 내의 프로그램을 process라 한다.)**
    - 작업관리자 켜보면, 돌아가고 있는 수많은 프로그램들이 process들이다!
  
  - **process communication (통신 프로세스)**
    - inter process communication (IPC, communication within same host)
      - end-system 내부의 process간의 통신은 OS에 따라 다르다.
    - **process communiation in different host**
      - **end-system 간의 process 통신은 message 교환을 통해 이뤄진다.**

  - **client process**와 **server process**
    - **client process (클라이언트 측 프로세스)**
      - initiates communication (다른 process와 session을 시작하려고 communication을 initiate 하는 쪽이다.)
      - client - server : web browser process
      - p2p : 데이터를 받는 쪽 (request 하는 쪽)
    - **server process (서버 측 프로세스)** 
      - waits to be contected (다른 process와의 session을 기다리는 쪽이다.)
      - client - server : web server process
      - p2p : 데이터를 주는 쪽 (response 하는 쪽)

  - **socket interface : process와 network 사이의 출입구**
    - Process sends/receives messages to/from its socket
    - process에서 network로 message가 빠져나가고 들어오는 문이다.
    - 정확하게는, **application layer와 transport layer간의 interface 이다.**

![socket interface](https://user-images.githubusercontent.com/59442344/111037181-6884db00-8466-11eb-8176-95e114ade5ca.jpg)

  - **process identifier 배정**
    - To receive messages, process must have identifier (수신 프로세스가 identifier를 갖고 있어야 한다!)
    - **Identifier = (IP address , port number)**
      - IP address (IP 주소) : host를 식별한다.
      - port number : host 내에서 동작하고 있는 특정 process를 식별하기 위하여 필요하다. (process의 주소는 아니다!)   



# Transport service

## 1. Transport layer protocol service 종류
  - **Data Integrity : 신뢰적 데이터 전송 , 데이터를 오류 없이 수신 프로세스에게로 전달한다.**
  - **Timing : 송신자가 socket으로 내보내는 모든 비트가 수신자의 socket 내로 특정 시간대 안에 도달하도록 하는 것.**
  - **Throughtput : 보장된 처리량을 요구하는 경우들에 있어 필요하다. (영상, 노래 수신)**
  - **Security : 송신 프로세스가 보내는 정보를 암호화 시킬 수 있고, 해독할 수 있다.**
## 2. Transport layer protocol (TCP / UDP)
  - **TCP**
    - Reliable transport
    - Flow control
    - Congestion control
    - Connection-oriented
    - Does not provide: timing, minimum throughput guarantee, security
  - **UDP**
    - Unreliable
    -  Does not provide: reliability, flow control, congestion control, timing, throughput guarantee, security, or connection setup   


# Application layer protocol

## 1. Application layer protocol service defines
  - Type of message exchanged : 어떤 메시지가 교환 되는가? (요청 , 응답, ,,,)
  - Message syntax : 메시지는 어떤 문법으로 이루어져 있는가?
  - Message semantics : 메시지는 어떤 의미를 담고 있는가?
  - Rules : 메시지는 어떻게 교환되는가?
  - **어떤 메시지 / 어떤 문법 / 어떤 의미/ 어떻게 교환 ? 의 내용들을 지원한다.**
  
## 2. Application layer protocol
  - **HTTP**
  - **FTP**
  - **SMTP / POP3 / IMAP**
  - **DNS**
