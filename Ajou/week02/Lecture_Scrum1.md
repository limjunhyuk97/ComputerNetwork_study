## 용어 정리
  - **경로 상의 노드(node)** : 송, 수신 종단 시스템을 포함하여, 경로상에 존재하는 모든 패킷스위치들을 아우르는 개념
  - **업스트림(upstream)** : 클라이언트나 로컬기기에서, 서버나 원격 호스트로 보내지는 데이터. 혹은 그 흐름. 
  - **다운스트림(downstream)** : 서버나 원격호스트에서, 클라이언트나 로컬기기로 전송되는 데이터. 혹은 그 흐름.
  - **트래픽강도(traffic intensity)** : 큐잉 지연의 정도를 측정하는 측도. ( aL / R )
  - **종단간 처리율(end-end throughput)** : 종단에서 종단까지 단위시간당 전달되는 bit의 양
  - **헤더 필드(header field)** : protocol layer을 지나감에 따라 붙는 부가적인 정보가 담긴 부분

# Delay, Loss, Throughput in Networks
  - **Delay** : 지연
  - **loss** : 손실
  - **Throughput** : 처리율


## Delay (지연)
  - packet arrival rate > packet output link capacity == packet queue, wait for turn.
  - packet은 route의 **각 node 상에서 4가지의 delay**를 겪을 수 있다.
  - **Total nodal delay = nodal processing delay + queuing delay + transmission delay + propagation delay**

<img src = "https://user-images.githubusercontent.com/59442344/110794139-c5db2980-82b8-11eb-91a4-a59b8cfae507.jpg" width="80%" height="80%">
<img src = "https://user-images.githubusercontent.com/59442344/110794144-c7a4ed00-82b8-11eb-9294-1dd4a9d90993.jpg" width="80%" height="80%">

### 1. Nodal processing delay (노드 처리 지연)
  - check bit error ( 운반 중 에러 발생 체크 )
  - determine output link ( 무슨 link로 보낼 지 결정 )

### 2. queuing delay (큐잉 지연)
  - time waiting at output link for transmission ( link buffer/queue에서 packet이 기다림 )
  - Depends on congestion level of router ( 앞에 막혀 있는 packet의 수에 따라 지연 정도가 달라짐 )
  - L : **packet length**
  - R : **link bandwidth**
  - a : **average packet arrival rate**
  - **Traffic intensity : aL/R , 0: delay small, 1: delay large, >1: delay infinite**

<img src = "https://user-images.githubusercontent.com/59442344/110810247-f3c86a00-82c8-11eb-9526-5c5e69583388.jpg" width="60%" height="60%">

### 3. transmission delay (전송 지연)
  - node(라우터)에서 처음 packet을 내보낼 때 걸리는 시간
  - L : **packet length (bits) , packet size**
  - R : **link bandwith (bps, bit/sec) , transmission rate**
  - **Dtrans = L / R**

### 4. propagation delay (전파 지연)
  - 한 node(라우터)에서 다른 node(라우터)로 bit가 전파되는게 걸리는 시간
  - d : **length of physical link**
  - s : **propagation speed**
  - **Dprop = d / s**


## Loss (손실)
  - Queue(Buffer) has finite capacity ( queue는 한정된 공간을 갖고 있다. )
  - Packet arriving to full queue dropped ( queue가 가득차 있으면 packet loss가 일어난다. )
  - Lost packet may be retransmitted by previous node or not at all ( loss된 packet은, 사라지거나, 다시 돌아간다. )

![packet_loss](https://user-images.githubusercontent.com/59442344/110813518-e6f94580-82cb-11eb-8972-bf8ae6647b94.jpg)


## Throughput (처리율)
  - Throughput : rate (bits/time unit) at which bits transferred between sender / receiver. 
    - 단위시간당, destination에 도착하는 bit 양
  - **Instantaneous Throughput** : rate at given point in time (순간 처리율)
  - **Average Throughput** : rate over longer period of time (평균 처리율)
    - F(bit) / T(F bit 수신완료까지 걸린 시간)
  - **Bottle-neck link** : end-end throughput(종단간 처리율)을 결정하는 전송률이 제일 작은 링크.
    - min{R1, R2, R3, ,,,} 

### 1. Throughput VS Bandwidth

 |Throughput|Bandwidth|
 |:---:|:---:|
 |단위 시간당 destination으로 전송되는 bit수|단위 시간당 link 상에서 전송되는 bit 수|
 |현상, 결과|사실|
 
  - Bandwidth가 커도, Throughput가 작을 수 있다.

### 2. Per-connection end-end throughtput
  - 여러 연결의 경우, 연결당 종단간 처리율
  - min(Rc, Rs, R/N) : core link를 N개의 연결이 사용하므로, R/N이 core의 through-put
    - 실제로는 서버 접속 링크인 Rs 또는, 클라이언트 접속 링크 Rc가 bottle-neck link일 경우가 많다.

<img src="https://user-images.githubusercontent.com/59442344/110821833-ce8d2900-82d3-11eb-8444-a27400eabe47.jpg" width="60%" height="60%">



# Protocol "layers"
  - protocol은 컴퓨터 간 통신을 위한 약속이다.
  - Network상의 구성요소들은 자신의 역할에 맞는 프로토콜들에 대한 정보들을 답
  - protocol은 쓰임에 따라서 묶음으로 묶여있고, 그 묶음들은 layer(계층)을 이루고 있다.
    - 각 계층은 내부에서 어떤 일을 수행하거나
    - 상부계층이 하부계층의 서비스에 의존한다.
  - **protocol stack**
    - TCP / IP model (5 layer model) : 인터넷 세계에서 표준으로 사용되는 네트워크 프로토콜이다.
    - ISO / OSI model (7 layer model)
  
  
## Internet protocol stack (TCP/IP model) , ISO / OSI reference model (OSI model)
![tcp ip osi iso](https://user-images.githubusercontent.com/59442344/110824264-3f354500-82d6-11eb-92b3-4664b3c4cccc.jpg)


## Layers
  - 왜 layers로 나뉘어져 있는 것일까?
    - **network protocol 설계에 대한 구조를 제공하기 위해서** 계층(layer)을 조직한다.
      - 시스템 구성요소에 대해 논의하기 위한 구조화된 방법을 제공한다.
    - 시스템의 구성요소의 유지보수, 갱신을 쉽게 해준다.
      - 각자 특정 역할을 담당하는, 특정 계층에서의 변화가, 전체 시스템에 영향을 끼치지 않는다.

### 1. Application layer
  - supporting network application
    - **종단 시스템끼리의 정보 packet을 교환**하는데 이 protocol을 쓴다
    - **process의 기능에 대한 책임을 지는 protocol이다! : 어떤 내용을, 어떤 형식으로, 언제, 어떻게 !**
    - 정보 packet : **message**
  - DNS HTTP SMTP FTP POP BitTorrent DHT
    - **HTTP** protocol : 웹문서 요청과 전송
    - **SMTP** protocol : 전자메일 전송
    - **FTP** protocol : 종단 시스템 간의 파일 전송 

### 2. transport layer
  - process-process data transfer
    - **클라이언트-서버 간의 Application layer message 전송에 관련**한 protocol을 담당한다.
    - **message 전달의 책임을 지는 protocol : 정보 전달의 신뢰성, 처리량, 시간소요, 보안 !** 
    - 정보 packet : **segment**
  - UDP TCP SCTP DCCP
    - **TCP** protocol : application에 연결지향형 서비스 제공 (전달 보장-신뢰성, 혼잡제어, 흐름제어 O)
    - **UDP** protocol : application에 비연결지향형 서비스 제공 (전달 보장-신뢰성, 혼잡제어, 흐름제어 X)

### 3. network layer
  - routing of datagrams from source to destinatiion
    - **UDP, TCP**에서 전달된 **segment와 함께, 목적지 주소를 전달받으면,** 목적지 transport layer로 전달하는 역할을 수행.
    - **router(packet switch), network 단위에서의 움직임 제어.**
    - 정보 packet : **datagram**
  - RIP OSPF BGP PIM-SM DVMRP ICMP IGMP IPv4 IPv6 DHCP
    - **하나의 IP protocol과, 네트워크 별로 여러개 존재하는 Routing protocol**
    - **IP** protocol : **IP datagram의 필드를 어떻게 정의**하는지와, **end-system과 router가 IP datagram 필드에 어떻게 동작**하는지 제시.
    - **Routing** protocol : **datagram의 이동경로를 결정**한다.

### 4. link layer
  - data transfer between neighboring network elements
    - **datagram을 현재 노드에서 다음 노드로 전달하는 역할을 수행한다.***
    - **router와, link-layer-switch(packet switch) 단위에서 움직임 제어 : 하나의 network 요소 -> 옆 network 요소로의 이동**
    - 정보 packet : **frame**
  - ARP Ethernet IEEE 802.11

### 5. physical layer
  - bits "on the wire" 
    - **frame 내부의 bit를 node에서 다음 node로 이동시키는 방법에 대한 protocol**

### + presentation layer
  - allow application to interpret meaning of data
  - 교환되는 데이터 의미 해석을 위한 서비스 제공 (데이터 압축, 데이터 암호화)

### + session layer
  - synchronization, checkpointing, recovery of data exchange
  - 데이터 교환의 경계와 동기화를 제공 (체킹포인트, 데이터 회복 수단 포함)

## Encapsulation(캡슐화)
  - 상위 계층에서 하위 계층으로 내려갈수록 앞에 정보(헤더)가 덕지덕지 붙어서 내려온다.
  - 하위 계층에서 상위 계층으로 올라갈때는 앞의 정보들이 떨어지면서 올라간다.
  - 각 layer에서 이루어지는 encapsulation
    - 애플리케이션 계층 메시지
    - 트랜스포트 계층 세그먼트
    - 네트워크 계층 데이터그램
    - 링크 계층 프레임
    - 각 layer에서 **packet은 header field와, payload field(상위 계층 전달 packet)로 이루어진다.**
<img src ="https://user-images.githubusercontent.com/59442344/110901487-bc4cd280-8347-11eb-9ae1-4ec43905b399.jpg" width = "80%" height = "80%">



















