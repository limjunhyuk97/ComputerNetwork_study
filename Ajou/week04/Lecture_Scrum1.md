## 용어 정리
  - **peer** : P2P 구조 상에서 연결되어 있는 기기들을 peer라고 한다.
  - **Bit Torrent**
    - **torrent** : Bit torrent에서, 파일 분배에 참여하는 peer들의 모임
    - **chunk** : Bit torrent에서의 파일의 분배단위. (256KB)
    - **tracker** : torrent에 참여하는 peer의 list를 갖고 있다.
    - **Peer churn** : Peer들이 P2P network에 자유롭게 참가할 수도 있고, network에서 나갈 수도 있다.

# P2P


## P2P architecture
  - server에 거의 의존하지 않거나, 아예 의존하지 않는다.
  - 임의의 end-system들끼리 서로 통신한다.
    - end-system들을 사용자들의 기기이다.
    - end-syste들은 간헐적으로 연결되며, IP 주소가 바뀔 수 있다.


## P2P file sharing tech evolution
  - **내가 원하는 파일이 어딨는지 어떻게 찾아낼 것인가?**
    - Napster
    - Gnutella
    - KaZaA
  - **파일을 chunk 단위로 조각내서 주고 받자!**
    - BitTorrent

### 1. Napster
![Napster](https://user-images.githubusercontent.com/59442344/112718430-4a26e100-8f36-11eb-9989-420384b9f26a.png)
  - 보조적인 central server가 존재한다
    - peer들의 IP 주소 저장
    - peer들이 갖고 있는 contents에 대한 정보 저장
  - 서버에 특정 content가 어떤 peer에게 있는지 질의하고, 그 peer의 IP 주소에 접속해서 request file

### 2. Gnutella
![Gnutella](https://user-images.githubusercontent.com/59442344/112718515-dcc78000-8f36-11eb-8cc1-67af5e69a6d2.png)
  - 주변에 있는 peer들에게 TCP 연결 후, 특정 content를 갖고 있냐고 query를 보낸다.
    - 답변(query hit)은 갔던 경로를 그대로 다시 돌아온다.
    - limited scope flooding을 사용해서 주변으로 어느 정도의 query만 보낸다.
  - file은 HTTP를 통해 보낸다.

### 3. KaZaA
![KaZaA](https://user-images.githubusercontent.com/59442344/112718671-e1406880-8f37-11eb-8185-09de2649e60e.png)
  - peer를 Group leader와, ordinary peer로 나눈다.
  - ordinary peer : content를 갖고 있는 일반적인 peer
  - Group leader : ordinary peer를 묶는 peer
    - 다른 Group leader들 끼리 연결되어 있다
    - 자기가 거느리고 있는 ordinary peer들이 갖고 있는 content가 무엇인지에 대한 정보를 갖고 있다.  

### 4. BitTorrent
![Bit torrent](https://user-images.githubusercontent.com/59442344/112719264-77c25900-8f3b-11eb-8de8-778574cb42b1.png)
  
#### 4.1 Bit Torrent란?
  - File을 256KB 단위의 chunk 단위로 주고 받는다.
  - peer는 download하면서 동시에 upload한다.
  - peer들의 목록은 tracker에 저장되어, peer들끼리 서로 연결된다.
  - peer들은 자기와 연결된 peer를 바꿀 수 있다 - 시간에 따라서 TCP로 연결된 대상 peer들이 바뀐다.
  - **Churn** : Peer들은 Selfishly torrent를 떠날 수도, Altruistically torrent에 남을 수도 있다.
#### 4.2 Bit Torrent의 장점
  - **File을 chunk 단위로 주고 받는다**에서 장점이 생긴다 : **유연한 데이터 전송, 수신이 가능해진다.**
  - **File상의 순서에 상관 없이 chunk들을 주고받을 수 있다.**
  - **내가 File에 대한 정보를 모두 갖고 있지 않더라도 chunk들을 주고받을 수 있다**
#### 4.3 Requesting chunk (rarest first) / 어떤 chunk를 요구할지
  - peer들에게 chunk list를 받는다.
  - 자기가 갖고 있지 않은 chunk 중에서도, **peer들에게 존재하는 chunk 중 가장 희소한 것 부터 받고 뿌린다.**
#### 4.4 Sending chunk (tit-for-tat) / 어떤 peer에 반응할지
  - 항상 총 **5개의 peer**들에게 정보를 보낸다.
  - **choked peers**
    - unchoked된 5개의 peer를 제외한 나머지 peer들에게는 정보를 보내주지 않는다. 
  - **unchoked peers(4)**
    - 활성화된 peer라고 한다.
    - 나에게 가장 빠르게 정보를 제공하는 4개의 peer와 교역 파트너를 맺는다.
    - 10초에 한번 씩 갱신된다.
  - **optimistically unchoked peers(1)**
    - 낙관적으로 활성화된 peer라고 한다.
    - 연결이 없었던 peer중 하나에 임의로 chunk를 보낸다. (4가지 가능성)
      - **상대 입장에서 나의 전송이 top4 안에 들면 나는 unchoked peer가 되고, 나에게 회신을 보낼 것이다.**
      - **회신을 받았을 때, 상대가 top4 안에 들면 상대는 unchoked peer가 되고, 서로가 unchoked peer가 된다.**
      - **회신을 받았을 때, 상대가 top4 안에 들지 않으면 30초 뒤에 끊길 것이다.**
      - **상대 입장에서 나의 전송이 top4 안에 들지 않으면, 나에게 회신은 오지 않을 것이고, 30초 뒤에 끊길 것이다.**
    - 30초에 한번 씩 임의로 연결한다. 


## File Distribution Time

![File distribution](https://user-images.githubusercontent.com/59442344/112718812-b86ca300-8f38-11eb-895b-b4c69cb437c0.png)
  
  - **server (peer)가 N개의 client (peer)들에게 파일을 제공할때, 모든 client들이 파일을 받는데 걸리는 시간**
  - 모든 병목 현상이 network edge 부분에 있다는 가정했을때의 P2P 와 server-client 구조에서의 비교

### 1. Client-Server architecture

![client - server](https://user-images.githubusercontent.com/59442344/112718899-447eca80-8f39-11eb-9653-0ad7c333ed81.png)
  
  - File Distribution Time이 **단일 server에서의 uploading time**과 **client의 downloading time**에서 결정
  - **하나의 server에 의해서 N개의 file이 전송되므로, file 수의 증가에 따라, distribution time이 linear하게 증가**

### 2. P2P architecture

![P2P](https://user-images.githubusercontent.com/59442344/112719042-fd450980-8f39-11eb-96e7-f43207518a3a.png)
  
  - File Distribution Time이 **peer의 1개 uploading time**, **peer의 downloading time, uploadin time**에서 결정
  - **여러개의 peer에 의해서 N개의 file이 전송되게 되므로 distribution time이 linear하지 않게 증가**

### 3. P2P vs Client-Server

![FDT](https://user-images.githubusercontent.com/59442344/112719157-c6232800-8f3a-11eb-8abc-2966c449488f.png)



## DHT (Distributed Hash Table)
  - **분산된 P2P Database이다.**
  - file에 대한 (key, value) 쌍을 P2P network 상의 peer들에게 흩뿌리는 것이다.
  - peer들은 key 값을 통해 DHT에게 질의 한다.
  - peer들은 (key, value) 쌍을 DHT에 등록할 수도 있다.

### 1. Hash Table & Distributed Hash Table
  - peer의 Identifier
    - **peer Identifier : IP주소를 Hash해서 만든 고유 값**
  - (key, value) 쌍 
    - **file key : file name를 Hash 해서 만든 고유 값**
    - **file value : [file이 실제 존재하는 peer 기기의 IP 주소] + [peer 기기 상의 port number]** 로 이루어져 있다
  - identifier 값에 key 값 assign
    - **peer identifier와 file key 값은 같은 범위 내의 값이어야 한다.**
    - key 값과 같은 값의 identifier값을 같는 peer에 key, value 쌍을 할당한다.
    - key 값과 같은 값이 없다면, key 값보다 큰 값중 가장 작은 identifier 값을 같는 peer에 key, value 쌍을 할당한다.

### 2. Circular DHT
  - **P2P에서의 file sharing을 위한 방법이다 : file이 어디에 위치하는가? 에 대한 정보를 제공하는 방법**
  - 각 peer들은 successor, predecessor, shortcut의 IP주소를 아는 방식으로 원형으로 연결되어 있다.
  - **Overlay Network**
    - Network 상의 peer들 위에 존재하는 또 하나의 Network이다.

![DHT overlaynetwork](https://user-images.githubusercontent.com/59442344/112722281-f0311600-8f4b-11eb-962d-624cd0fcfc01.png)

#### 2.1 Circular - DHT Based File Share
  - **[Circular - DHT Based File Shared에서 파일 위치 찾기]**
  - DHT 상에 존재하는 peer에게, **특정 파일의 위치**에 대해서 질의를 한다.
  - 특정 파일명을 hashing하여 key값을 도출하고, 그 key값에 mapping되는 identifier값을 가진 peer를 찾는다.
  - 해당 peer에게 특정 파일의 위치에 대한 (key, value) 쌍이 존재하지 않는 다면, successor peer에게 질의한다.
    - Circular DHT에서는 각 peer들이 본인의 immediate successor과, predecessor만 알고 있다.
  - 질의를 반복하다가, **key 값을 갖고 있는 peer가 (key, value) 쌍에 대한 정보를 첫 질의를 했던 peer에게 돌려준다.**

![Circular DHT based File Share](https://user-images.githubusercontent.com/59442344/112722996-c4b02a80-8f4f-11eb-90b7-818064e8deb9.png)

#### 2.2 Circular DHT with shortcuts
  - Each peer keeps track of IP addresses of predecessor, successor, shor cuts.
  - 그냥 circular로 돌면 너무 오래 걸릴 수 있으니, **shortcut에 대한 정보도 저장**한다.

![circular DHT predecessor - successor shortcut](https://user-images.githubusercontent.com/59442344/112723186-a4cd3680-8f50-11eb-8b63-51abdd752507.png)

#### 2.4 peer churn
  - **Peer churn : Peer들이 P2P network에 자유롭게 참가할 수도 있고, network에서 나갈 수도 있다.**
  - 자유로운 입장, 퇴장이 가능하기 때문에 바로 옆의 successor만 안다면, 문제가 될 수도 있다.
  - **그러므로, 각 peer들은 successor의 successor의 IP 주소까지 알고 있다.**











