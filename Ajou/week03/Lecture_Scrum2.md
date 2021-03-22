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


# DNS(Domain Name System)


## DNS란?
  - Domain Name System의 약자이다. 
  - Domain Name System이란 무엇일까?
    - **구조적으로 봤을 때)** DNS server들이 계층구조로 분산된, 분산 계층 데이터베이스이다 
    - **protocol 관점에서 봤을 때)** host가 분산된 DNS server에 질의하도록 허락하는 application layer protocol이다.
    - **서비스 관점에서 봤을 때)** domain "name"을 중심으로 **다양한 서비스를 제공하는 시스템**을 말하는 것이다. 

## protocol 관점에서의 DNS
  - application layer protocol이 domain name(host name)을 IP address로 바꾸기 위한 질의를 할 때 사용하는 protocol이다. 
  - 예를 들면, HTTP가 요청 메시지를 www.naver.com으로 보내기 위해서 ..
    - 브라우저가 동작하는 기기가 DNS의 client 측이다.
    - 1. 브라우저에서 URL을 추출하여 DNS client 측으로 건네준다.
    - 2. DNS client가 DNS server들에 IP 주소를 질의하는 과정을 거친다. (Local DNS server를 거쳐서 질의 한다.)
    - 3. DNS client는 DNS server들로부터 IP 주소를 받고, 그 정보를 브라우저로 건넨다.
    - 4. 브라우저가 IP주소를 바탕으로 그 위치(HTTP server process)로 TCP 연결을 초기화 한다.  

## service 관점에서의 DNS
  - **domain name -> IP address 로 바꾸는 service**
  - Host aliasing
    - 
  - Mail service aliasing
    - 
  - Load distribution
    - 

## DNS의 동작 원리

### 왜 중앙에서 모든 IP주소를 관리해선 안되는가?

### 분산 계층 데이터베이스

### DNS caching


## DNS record와 message

### DNS record

### DNS message
