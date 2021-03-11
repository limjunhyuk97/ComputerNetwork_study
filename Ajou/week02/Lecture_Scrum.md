## 용어 정리
  - **경로 상의 노드(node)** : 송, 수신 종단 시스템을 포함하여, 경로상에 존재하는 모든 패킷스위치들을 아우르는 개념
  - **업스트림(upstream)** : 클라이언트나 로컬기기에서, 서버나 원격 호스트로 보내지는 데이터. 혹은 그 흐름. 
  - **다운스트림(downstream)** : 서버나 원격호스트에서, 클라이언트나 로컬기기로 전송되는 데이터. 혹은 그 흐름.
  - **트래픽강도(traffic intensity)** : 큐잉 지연의 정도를 측정하는 측도. ( aL / R )
  - **종단간 처리율(end-end throughput)** : 종단에서 종단까지 단위시간당 전달되는 bit의 


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

# Protocol "layers"

## Internet protocol stack (TCP/IP model)

## ISO / OSI reference model (OSI model)

## Encapsulation



















## Throughput


# Protocol layers, service model
