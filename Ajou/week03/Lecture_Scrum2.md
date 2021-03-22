## 용어정리
  - **Host name VS Domain name**
  - **Domain name**
    - website의 주소를 보통 의미한다.
    - IP 주소들을 사람들이 더 쉽게 인식할 수 있도록 하려 만들었다.
    - 모든 domain name이 host name은 아니다.
    - 여러개의 label들과, label들을 .(dot)으로 연결한 구조로 이루어져 있다.
      - 각각의 label들은 최대 63개의 문자로 제한되고, domain name의 최대 길이는 253개의 문자로 제한된다.
  - **Host name**
    - network에 연결되어 있는 기기들을 의미한다.
    - 웹 상의 기기들을 사람들이 쉽게 구분할 수 있도록 만들었다.
    - 모든 host name은 domain name이다.
    - LDH(Letter, Digit, Hypen)로 이루어져 있고, Hypen으로 시작하거나 끝날 수 없다.

<img src="https://user-images.githubusercontent.com/59442344/111987199-3bb18180-8b52-11eb-8acb-63731869b178.png" width="40%" height="40%">
  
  - **IP address(IP 주소) VS Host name**
  - **Host name(Domain name)**
    - 사람 친화적인 인터넷 상의 호스트의 위치 정보이다.
    - 가변 길이에, 위치에 대한 정확한 정보를 제공하지 않는다.
    - www.naver.com
  - **IP address**
    - 컴퓨터 친화적인, 인터넷 상의 호스트의 위치 정보이다.
    - 고정길이(4 byte)에, 위치에 대한 정확한 정보를 제공한다.  
    - 121.7.106.83 

  - **name server** : DNS의 server들을 name server라고도 한다.



# DNS(Domain Name System)


## DNS란?
  - **Domain Name System의 약자**이다. 
  - Domain Name System이란 무엇일까?
    - **구조적 관점에서 봤을 때)** DNS server들이 계층구조로 분산된, 분산 계층 데이터베이스이다 
    - **protocol 관점에서 봤을 때)** host가 분산된 DNS server에 질의하도록 허락하는 application layer protocol이다.
    - **서비스 관점에서 봤을 때)** domain "name"을 중심으로 **다양한 서비스를 제공하는 시스템**을 말하는 것이다. 


### protocol 관점에서의 DNS
  - application layer protocol이 domain name(host name)을 IP address로 바꾸기 위한 질의를 할 때 사용하는 protocol이다. 
  - 예를 들면, HTTP가 요청 메시지를 www.naver.com으로 보내기 위해서 ..
    - 브라우저가 동작하는 기기가 DNS의 client 측이다.
    1. 브라우저에서 URL을 추출하여 DNS client 측으로 건네준다.
    2. DNS client가 DNS server들에 IP 주소를 질의하는 과정을 거친다. (Local DNS server를 거쳐서 질의 한다.)
    3. DNS client는 DNS server들로부터 IP 주소를 받고, 그 정보를 브라우저로 건넨다.
    4. 브라우저가 IP주소를 바탕으로 그 위치(HTTP server process)로 TCP 연결을 초기화 한다.  


### service 관점에서의 DNS
#### 1. domain name -> IP address 로 바꾸는 service
#### 2. Host aliasing
  - 복잡한 host name을 가진 host에게 하나 이상의 host name을 부여해주는 service
  - 정식 host name(canonical hostname)보다 이해하기 쉬운 별칭 host name(alias hostname)을 부여해주는 service이다.

![DNS service aliasing](https://user-images.githubusercontent.com/59442344/111992711-1116f700-8b59-11eb-87fd-f3ccca55d774.png)

#### 3. Mail service aliasing
  - 메일 서비스 사용자에게 더 쉬운 메일주소를 부여해주는 service
  - 기업의 메일 server와 웹 server가 같은 host name(alias hostname)을 갖는 것을 허용한다.  

![mail server aliasing](https://user-images.githubusercontent.com/59442344/111993151-9f8b7880-8b59-11eb-8b44-f526d8ddc0a9.png)

#### 4. Load distribution
  - 여러개의 server로 웹 서비스를 운영하는 기업이나 단체에게 부여된 hostname에 대해서, 여러개의 중복 서버의 IP 주소를 mapping 해주는 service
  - 사용자가 많이 몰리는 웹 서비스에 대해서, 다중 사용으로 인한 부하의 분산을 제공해주는 service이다.
  - load distribution의 과정
    - client가 DNS server에 domain name의 IP 주소에 대한 질의를 하면, IP 주소 전체 집합으로 응답한다.
    - slient는 대체로 주소 집합 첫번째의 IP 주소로 HTTP 요청을 보낸다.

![load distribution](https://user-images.githubusercontent.com/59442344/111993596-26d8ec00-8b5a-11eb-9e44-7b7d8c988975.png)


## DNS의 동작 원리

### 왜 단일 server에서 모든 IP주소를 관리해선 안되는가?
  - **확장성이 전혀 없기 때문이다. (Doesn't scale!)**
  - **서버고장 == 인터넷 통신 중단**
    - DNS는 domain name과, TCP 연결을 위한 IP 주소를 연결지어주는 서비스이기에, 서버가 고장나면, 아무도 IP 주소를 받지 못한다.
    - IP주소를 알 수 없다면, end-system간의 연결이 불가능해진다.
  - **과도한 트래픽**
    - 하나의 server로 모든 질의가 집중되면, 과도한 트래픽이 발생한다. (모든 HTTP 요청과, 전자메일의 처리에 관여해야 함)
  - **거리 문제**
    - 단일 server와 client와의 위치에 따라서 과도한 지연이 발생할 수 있다.  
  - **유지 관리**
    - 단일 server에서 모든 인터넷 server에 대한 record를 관리해야 하므로, 유지관리가 어려워진다. (잦은 갱신, 인증 문제)

### 분산 계층 데이터베이스
  - DNS는 host에 대한 **mapping 정보를**, 전세계에 존재하는 DNS 서버에 계층 구조에 따라 **분산시킨다.**
  - 계층에 따라, **3가지 유형의 DNS 서버가 존재**한다.

![DNS server hierarchy](https://user-images.githubusercontent.com/59442344/112000390-4d4e5580-8b61-11eb-8c9d-432ae27ac55d.png)

#### naver.com의 IP 주소를 얻기 위해 DNS server들에게 mapping 정보를 질의한다고 가정한다면..
#### 1. Root DNS server
  - client가 naver.com에 대한 정보를 얻기 위해 가장 먼저 질의를 보내는 곳이다.
  - 질의에 대한 답변으로 Top-level domain인 com에 대한 내용을 관리하는 TLD server의 IP 주소 정보를 보내준다.
  - **전세계에 root server들이 뿌려져 있고, IP 주소를 얻기 위해서 가장 먼저 질의를 보내는 server이다.**
  - **domain name 상의 TLD를 확인하고, 그 TLD server IP 주소를 보내준다.**
#### 2. TLD(Top-level domain) DNS server
  - Root DNS server에게 받은 IP주소를 바탕으로 com TLD DNS server에 naver.com의 mapping 정보에 대한 질의를 보낸다.
  - 질의에 대한 답변으로 naver.com에 대한 내용을 관리하는 Authoritative DNS server의 IP 주소 정보를 보낸다.
  - **상위 레벨 도메인(com, org, edu ...), 국가 상위 레벨 도메인(kr, uk, fr, ca, jp ...) 들에 대한 server이다.**
  - **다음 위계 상의 authoritative server의 IP 주소를 보내준다.**
#### 3. Authoritative DNS server
  - TLD DNS server에게 받은 IP 주소를 바탕으로 naver.com authoritative DNS server에 목적지 상세 주소에 대한 mapping 정보를 질의한다.
  - 질의에 대한 답변으로 상세주소에 대한 IP주소가 돌아오고, client의 브라우저는 해당 IP 주소로 TCP 연결을 초기화 한다.
  - **host name과 IP 주소를 mapping하는 DNS record를 공개적으로 제공하는 server이다. (실제, host name과 IP주소를 엮은 정보를 관리하는 server.)**
  
### Local DNS server (default name server)
  - **실제 DNS query를 보내면, query는 Local DNS server로 보내진다.**
  - **DNS 계층구조에 속한 server는 아니지만, DNS 구조의 중심에 있다.**
  - **proxy server 처럼 동작한다.(IP 주소에 대한 cache를 갖고 있다.)**

![DNS query](https://user-images.githubusercontent.com/59442344/112010176-6b6c8380-8b6a-11eb-8386-1862826d3ac0.png)

#### 1. Iterative query : 직접 mapping 정보를 얻기위해서 보내는 query
#### 2. Recursive query : 본인을 대신하여 mapping 정보를 얻도록 보내는 query

### DNS caching
  - **매번 client가** local DNS server를 통해서 mapping정보에 대한 **query를** root server부터, authoritative server까지 **날리는 과정이 반복되지는 않는다.**
  - **DNS server에 대한 응답들은, local name server에 단기적으로 저장**될 수 있다.
    - **domain server - IP address mapping 에 대한 정보**
    - **TLD server의 address에 대한 정보** (root server 우회)
    - 그렇지만, host가 IP address를 바꾼다면, cache의 TTL(Time-to-live, 데이터 유효기간)이 expire될때까지 모를 수도 있다.

## Resource Record(RR)와 DNS message

### RR (Resource Record)
  - **host name을 IP address와 mapping하기 위한 record**를 RR이라 한다 (mapping 정보를 담고 있는 record)
  - DNS는 **하나이상의 RR를 가진 DNS message로 query에 응답**한다.
  - **DNS server에는 특정 TYPE을 가진 RR들이 존재**한다.
    - authoritative server : 특정 host name에 대한 A TYPE RR 존재 가능
    - TLD server : authoritative server들에 대한 A TYPE RR 존재 가능
  - **RR의 구성**
    - \<NAME> \<TYPE> \<CLASS> \<TTL> \<RD LENGTH> \<RDATA>
    - **"TYPE : !, NAME : ! 이라면, RDATA : ! 일 것이다." 에 대한 표**

|TYPE|NAME|->|RDATA|
|:--:|:--:|:--:|:--:|
|A(default)|host name||IP address|
|NS|domain||authoritative DNS server hostname for this domain|
|CNAME|alias name||canonical name|
|MX|name associated with mail server name||canonical name|
|PTR|reversed IP address.in-addr.arpa||host name|

<img src="https://user-images.githubusercontent.com/59442344/112018976-5d226580-8b72-11eb-97cf-874a869c902b.png" width="35%" height="35%">
                                                    
### DNS message
  - **DNS 요청과 응답을 위한 주고 받는 message**이다. (**요청, 응답 message 같은 포맷을 공유**한다.)

![dns message](https://user-images.githubusercontent.com/59442344/112021034-36fdc500-8b74-11eb-97db-c0d8288230a6.png)

  - **identification** : 질의 / 응답 일치 여부를 식별
  - **flags** : 질의(0) / 응답(1) 구분 +a
  - **#questions** : 질문의 수 (1번 필드 질문 갯수)
  - **#answer RRs** : 질의된 이름에 대한 RR (2번 필드 답변 갯수)
  - **#authority RRs** : authoritative server에 대한 RR의 수 (3번 필드 RR 수)
  - **#additional RRs** : 추가적으로 도움 되는 정보의 수 (4번 필드 수)
  - **questions** : 질의로 보내는 NAME, TYPE 필드
  - **answers** : 질의에 대한 답변 RR
  - **authority** : authoritative server에 대한 RR
  - **additional info** : (추가적인 도움되는 정보들) answer로 name이 왔다면, IP 주소를 같이 보낸다.










