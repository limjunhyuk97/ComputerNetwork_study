# 교과목 학습 중점

  - **end-end가 어떻게 상호작용하는가? (end system들의 상호작용 방법)**
  - **Network Core에서 end system 사이의 상호작용을 어떻게 도와주는가?**

## 용어정리

  - **호스트(host), 종단 시스템 (end-system)** : 네트워크 상 제일 말단(Network edge)에 연결되는 모든 장치들 (client, server, data centers)
    - **송신 종단 시스템** : 데이터를 보내는 end-system
    - **수신 종단 시스템** : 데이터를 받는 end-system
    - **클라이언트 (client)** : 데이터 송신을 요구하는 컴퓨터
    - **서버(server)** : 데이터 송신을 요청받는 컴퓨터
    - **데이터 센터(data center)** : 서버 컴퓨터와 네트워크 회선 등을 제공하는 시설
  - **패킷(packet)** : 송신 end-system이 보낼 데이터를 segment로 나눠서 header를 붙인 정보 패키지.
  - **패킷 스위치 / 교환기(packet switch)** : 패킷을 받고, 전달하는 역할을 한다. 
    - **라우터(router)** : 네트워크 코어(network core)에서 사용, 입력 packet을 출력 link로 교환하는 일 수행
    - **링크 계층 스위치(link-layer-switch)** : 접속 네트워크(access network) 사용
    - **출력 버퍼(output buffer)** : 패킷 스위치와 연결된 각 링크별로, 송신하려는 packet을 임시로 저장하는 역할 함.
  - **통신 링크(communication link)** : 네트워크 상의 호스트, 패킷 스위치들을 물리매체(physical media)를 통해 연결해주는 역할을 한다.
    - **전송률(transmission rate)** : 통신 링크의 초당 비트수. (단위 : bps, bit per second)
  - **경로(route / path)** : 데이터가 지나온 일련의 통신링크와, 패킷 스위치
  - **ISP(Internet Service Provider)** : 종단 시스템에게 인터넷 서비스 제공. 패킷 스위치와 통신 링크로 이루어진 네트워크.
    - 종류 : 가정ISP, 대학ISP, 기업ISP, 셀룰러 데이터ISP ...
    - ISP tier : 서비스 제공의 범위에 따라서 tier가 달라진다.
    - ISP들은 따로 각각 따로 관리되고, IP 프로토콜을 수행하고, naming과 주소배정 방식을 따른다. 
  - **프로토콜(Protocol)** : 통신하는 둘 이상의 원격 개체가 주고받는 메시지의 포맷과 순서뿐 아니라, 메시지 송수신과 다른 이벤트에 따른 행동들을 정의하는 일련의 규칙
    - **TCP/IP** : 인터넷의 주요 프로토콜
    - **What is communicated? When / How it is communicated?**
  - **소켓 인터페이스 (socket interface)** : end system 간의 정보 전달의 방식에 대한 규칙 제공
  - **분산 애플리케이션 (distributed application)** : 데이터를 교환하는 많은 end-system을 포함하는 애플리케이션
  - **가장자리 라우터 (edge router)** : 특정 종단 시스템에서 다른 종단 시스템으로 이르는 경로 상에 위치한 첫 라우터

## What is internet?
  
  - **"Any interconnected group or system", "Network of networks"**
  
  - **"Nuts and bolts" view (구성 요소 관점)**
    - **인터넷은 어떤 하드웨어와 소프트웨어로 이루어져 있을까??**
    - 인터넷은 하드웨어적으로.., end-systmem(host) + packet switch + communication link 로 이루어져 있다.
    - 인터넷의 그러한 하드웨어들은, 프로토콜(protocol)을 수행한다.
    
  - **Service view (사용 관점)**
    - **인터넷은 인프라구조로서 애플리케이션(응용 프로그램)에 서비스를 어떻게 제공하는가??**
    - 인터넷의 응용(사용)은, 종래의 서비스인 이메일 뿐만 아니라, 현재의 음악 스트리밍, 다중 참여 게임 등등을 가능하게 한다.
    - 인터넷의 응용(사용)은, 분산 애플리케이션들이 하나의 종단 시스템에서 다른 종단 시스템으로 정보를 전달하는 방식의 규칙(인터넷 소켓 인터페이스)을 제공한다.
    - 인터넷의 응용(사용)은, 또한 다양한 정보 전달의 규칙을 제공한다.

## Network

![Network](https://user-images.githubusercontent.com/59442344/110133895-2c230080-7e10-11eb-8d44-3915b5a789b4.jpg)

### 1. Network Edge
  
  - 네트워크의 가장자리 말단 부분들로, **end-system, access network, physical media** 로 이루어진다.
  
  - **End system (종단 시스템)**
    - **송신 종단 시스템** : 데이터를 보내는 end-system
    - **수신 종단 시스템** : 데이터를 받는 end-system
    - **클라이언트 (client)** : 데이터 송신을 요구하는 컴퓨터
    - **서버(server)** : 데이터 송신을 요청받는 컴퓨터
    - **데이터 센터(data center)** : 서버 컴퓨터와 네트워크 회선 등을 제공하는 시설

  - **Access networks (엑세스 네트워크)**
    - **end-system을 edge-router에 연결하는 네트워크**
    - **end-system과 edge-router 연결의 종류**
      - residential access net : DSL, 케이블 방식
      - institutional access networks : 이더넷 기술, 무선 LAN 접속(WIFI)
      - mobile access networks
    
  - **Physical media (물리 매체)**

### 2. Network Core

  - **end-system 들을 연결하는 packet switch와 link들의 mesh**.
  - **interconnected router**
  - **network of networks**

![packet switch, circuit switch](https://user-images.githubusercontent.com/59442344/110193425-257ba400-7e77-11eb-9e0b-483f309706f1.jpg)

  - **packet switching(패킷 교환)**
    - 송신 end system에서 보내고자 하는 데이터를 packet이라는 데이터 덩어리로 분할한다.
    - 목적지 end system에서는 packet들을 다시 모아서 하나의 데이터 덩어리로 복원한다.
    - 분할된 packet은 communication link와, packet switch를 지나서 목적지 end system에 도착한다.
    - **packet / communication link**
      - packet은 각 link의 transmission rate에 따라서, 최대 전송속도로 이동한다.
      - 1 packet, N개 link, transmission rate : R bps, packet 단위 : L bit = 전송 시간 : N * L/R (sec)
    - **packet / packet switch**
      - **store and forward transmission**
        - packet의 모든 비트를 router가 전송 받은 후(store), 다시 전송(forward) - 패킷 단위의 데이터 전송
        - entire packet must arrive at router before it can be transmitted on next link
    - **queuing delay / packet loss**
      - arrival rate > transmission rate for a period of time : **packet will wait** to be transmitted, **can be lost**
      - **queuing delay** : packet이 전송을 위해서 출력버퍼에서 대기하는 현상
      - **packet loss** : 출력 버퍼의 공간이 가득 차서 packet의 정보가 폐기되는 현상
    - **Forwarding / routing**
      - nework-core's two key function 
      - **Forwarding (packet 전달)**
        - 목적지까지 가려면 어떤 링크로 packet을 보내야 적절한지에 대한 정보를 담고 있는 forwarding table을 참고하여 packet 전달
      - **Routing (경로 설정)**
        - 목적지까지의 최단경로를 계산하는 routing algorithm을 사용하는 routing protocol을 통해 forwarding table이 설정된다


  - **circuit switching(회선 교환)**
