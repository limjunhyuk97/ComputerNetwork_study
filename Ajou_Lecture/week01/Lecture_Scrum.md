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
    - **라우터(router)** : 네트워크 코어(network core)에서 사용
    - **링크 계층 스위치(link-layer-switch)** : 접속 네트워크(access network) 사용
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

  - **packet switching(패킷 교환)**

  - **circuit switching(회선 교환)**

# 1주차 질문

## access network의 역할이 무엇이라고 이해했는지 기술하시오.
  
network에 접속하기 위한 network로서, access network는 어떤 end-system과, 다른 end-system을 연결하는 경로 상에 있는 첫번째 라우터(가장자리 라우터)에 end-system을 연결하는 network라고 이해했습니다. 그리고 국지적인 장소에서 같은 라우터를 공유하는 end-system들을 묶어주는 역할을 한다고 이해했습니다. 예로 residential access net에서 단일 DSL 링크 속의 다른 주파수 대역을 통해 집에서 이용하는 전화와 인터넷들을 core network에 연결하는 것을 들 수 있을 것 같습니다.

## Internet의 가장자리에 위치하는 다양한 종류의 네트워크들을 설명했는데, 그 중에 가장 관심이 가는 혹은 흥미로운 네트워크가 어떤 것인지 적으시오. 그리고, 어떤 점에서 흥미가 있는지 적으시오.

가장 흥미롭게 느껴진 네트워크는 outer space network 였습니다.

## packet switching과 circuit switching의 특징 (차이점)
  
  - 

## packet switching과 circuit switching의 장/단점

  - 
