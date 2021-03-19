## 용어 정리
  - cookie : 어떤 website에 접속할 때, client의 기기에 저장되는 작은 크기의 문자와 숫자로 이루어진 파일
  - Web cache (proxt server) : origin server를 대신해서, HTTP 요구를 충족하는 네트워크 개체

# HTTP
  - 2가지의 HTTP message에 대해서 들여다보자. : request, response message

## HTTP request message (요청 메시지)

![http request](https://user-images.githubusercontent.com/59442344/111738692-24119900-88c5-11eb-97e1-7d5c756b53cd.jpg)

  - **Request line + header line + entity body 의 구성**을 갖는다.

### 1. Request line
  - HTTP ASCII 텍스트의 첫번째 줄이다.
  - **method field** + **URL field** + **HTTP Version field**
    - **method field** : GET, POST, HEAD, PUT, DELETE, CONNECT 등의 값을 가질 수 있다.
      - **GET** method : URL을 통해서 서버로부터 객체를 요청할때 사용한다. (entity body 부분이 비어있다.)
      - **PUT** method : URL field로 명시된 곳에 entity body부분의 값을 업로드 시킨다.
      - **DELETE** method : URL field로 명시된 곳에 있는 파일을 삭제한다.
      - **POST** method : URL에 form input에 entity body의 내용을 업로드 한다. (form 제출..)
      - **HEAD** method : response 시에 HTTP 메시지로 응답하되, 요청한 객체는 보내지 않는다.
    - **URL field** : 참조할 객체가 존재하는 위치를 나타낸다.
    - **HTTP Version field** : 브라우저가 어느 버전의 HTTP를 구현하는지를 나타낸다.

### 2. header line
  - HTTP ASCII 텍스트의 두번째 줄부터, \r\n \r\n 까지의 줄이다.
  - **header field name** : **value** 로 구성된다.
    - **Host:** : 요청하는 객체가 존재하는 host를 명시하고 있다. (Web proxy cache에서 필요로 한다.)
    - **Connection:** : persistent, non-persistent connection에 대한 내용을 담고 있다.
    - **User-agent:** : client의 브라우저 타입을 명시한다.
    - **Accept:** : client가 이해가능한 컨텐츠 타입이 뭔지 알려준다.
    - **Accept-Encoding:** : clinet가 이해가능한 encoding 방식이 무엇인지 알려준다.
    - **Accept-language:** : client가 객체의 어떤 언어 버전을 원하는지 명시한다.


### 3. entity body
  - 수신자에게 전달하고자 하는 내용을 담는 부분이다.

## HTTP response message (응답 메시지)

![http response](https://user-images.githubusercontent.com/59442344/111738694-2542c600-88c5-11eb-8a3f-887c60568874.jpg)

  - **Status line + header line + entity body로 구성**된다.

### 1. Status line
  - **version field** + **status code field** + **phrase field**
    - **version field** : 어떤 HTTP 프로토콜 버전을 사용하는지 나타낸다.
    - **status code field** : 요청에 대한 성공여부를 나타낸다. (200, 404, ,,)
    - **phrase field** : 상태코드에 대한 설명을 글로 나타낸다. (OK, Not Found)
      - **200 OK**
      - **301 Moved Permanently**
      - **400 Bad Request**
      - **404 Not Found**
      - **505 HTTP Version Not Supported**

### 2. Header line
  - **header field name : value**로 구성된다.
  - **Connection:** : response를 보낸 후의 TCP 연결에 대한 내용을 담는다
  - **Date:** : server가 객체를 담아서 응답을 보낸 시간을 의미한다.
  - **Server:** : 어떤 server에 의해 만들어진 message인지를 명시한다
  - **Last-Modified:** : 마지막으로 객체가 언제 수정되었는지를 나타낸다.
  - **Content-Length:** : 송신하는 객체의 바이트수를 나타낸다.
  - **Content-Type:** : 송신하는 객체가 어떤 타입인지를 나타낸다. + utf-8같은 utf형식까지 같이 보낸다.

### 3. entity body
  - 송신자에 회신할때, 요청받았던 객체를 담아두는 부분이다.


# Cookie
  - **특정 기기에서 어떤 website에 접속할때, 그 기기로 다운로드 되는 작은크기의 문자와 숫자로 이뤄진 파일이다.**
  - 원래 HTTP는 비상태 프로토콜(statelss protocol)이기에, 서버는 접속하는 기기의 정보를 저장하지 않는다.
  - 그러나, 특정 목적을 위해서 **사용자의 정보를 식별하기 위해 cookie를 사용하여, 사용자의 정보를 추적한다.**
    - protocol endpoints maintain state(: HTTP messages carry state) at sender/receiver over multiple transaction

## Cookie의 4가지 요소

![cookie](https://user-images.githubusercontent.com/59442344/111746286-2a0d7700-88d1-11eb-8076-b18ab63a67ca.jpg)

  - **Cookie header line of HTTP response message** (응답 메시지의 cookie header line)
    - 첫 접속 시에 웹 사이트에서는 사용자에게 고유의 ID를 Set-cookie: 헤더의 value로 제공한다.
  - **Cookie header line in next HTTP request message** (요청 메시지의 cookie header line)
    - 다시 서버에 요청 시에, cookie 파일에서 ID 값을 발췌하여, Cookie: 헤더와 함께 HTTP로 전달한다.
  - **Cookie file kept on user's host, managed by user's browser** (사용자의 시스템에 있고, 브라우저에서 관리되는 cookie file)
    - Set-cookie: 헤더의 value로 제공받은 ID 값을, 브라우저가 cookie 파일에 저장한다.
  - **Back-end database at website** (웹 사이트의 벡엔드 DB)
    - 첫 접속 시에 웹 사이트는 사용자에게 고유의 ID를 만들어 제공하고, DB에 ID에 대한 entry를 만든다.
    - DB에 존재하는 ID값으로 접속하는 사용자에 대해서 서버는 사용자의 활동을 추적한다. 

## Cookie의 이용 (장점)
  - Authorization
  - Web Shopping
  - Recommendations
  - User session state

## Cookie와 sercurity problems (단점)
  - permit sites to learn a lot about us
  - we may supply name and e-mail to sites
  - informations associated with cookie could be sold illegally

# Web Cache (Proxy server)

![web cache](https://user-images.githubusercontent.com/59442344/111750983-43b1bd00-88d7-11eb-90d3-1990cfc0750d.png)

  - **origin server를 대신하여 HTTP 요구를 충족시키는 네트워크 개체.**
    - ISP들이 설치한다.
    - 자체 저장 디스크에 최근 호출된 객체의 사본을 저장, 보존하다. 
  - 목표는, **origin server를 거치지 않고 객체를 전달받을 수 있게 하는 것**이다.
    - browser - server 연결을 cache를 거치게끔 한다. (Proxy server는 server이면서, client이다.)
    - **cache에 최신 정보의 객체가 있다면 : cache에게서 정보를 받는다.**
    - **cache에 최신 정보의 객체가 없다면 : origin server로부터 정보를 받는다. + cache를 갱신 (조건부 GET)**
    - **cache에 객체가 없다면 : origin server로 부터 정보를 받는다. + cache를 갱신**

## Web Cache의 장점
  - internet 전체의 traffic을 실질적으로 줄여준다.
  - client의 요구에 대한 응답시간을 줄여준다.
  - 특정 기관에서 인터넷으로 접속하는 링크상의 web traffic을 대폭 줄여준다.
  - 컨텐츠 제공자들에게 금전적으로 효과적인 해결책을 제시한다.

### (예시)

![web cahce_2](https://user-images.githubusercontent.com/59442344/111753275-0569cd00-88da-11eb-991a-ce37756f0358.png)

  - **total delay = internet delay + access delay + LAN delay**
  - **total delay** : 요청으로부터, 객체 수신까지 걸리는 시간
  - **internet delay** : 접속회선의 internet 부분의 router가 HTTP 요청을 전달하고 응답받을 때까지 걸리는 평균 소요시간.
  - **access delay** : 라우터간의 지연 (queuing delay만 고려)
  - **LAN delay** : LAN access network의 router까지 HTTP 요청이 전달되기까지 걸리는 평균 소요 시간.

### 가정)
  - Avg. object size: 1M bits
  - Avg. request rate from browsers to origin servers: 15/sec
  - 접속회선의 인터넷 부분 라우터가 HTTP 요청을 전달하고 응답을 받을때까지 평균 소요시간 : 2 sec
  - Access link rate: 15 Mbps
  - LAN link rate : 100 Mbps
  - Cache hit rate : 0.4 (0.4 satisfied int cache, 0.6 satisfied in origin)

### 1. proxy server가 없는 경우
  - **internet delay** : 2 sec
  - **access delay** : (queuing delay만 고려), 15 * 1 / 15 = 1 [minutes]
  - **LAN delay** : (queuing delay만 고려), 15 * 1 / 100 = 0.15 [msec]
  - access delay가 1에 수렴하므로, 대기 시간이 몇분을 초과한다. (너무 느림)

### 2. proxy server가 있는 경우
  - **internet delay** : 2 sec
  - **access delay** : (queuing delay만 고려), 15 * 0.6 * 1 / 15 = 0.6 [msecs]
  - **LAN delay** : 무시할만한 정도
  - total delay = 0.6 * (2 + 0.01) + 0.4 * (0.01) sec
  - 0.6 * (2 + 0.01) : 0.6의 확률로, 2sec(internet delay) + 0.01sec(access delay + LAN delay)
  - 0.4 * (0.01) : 0.4의 확률로, 0.01sec(cache에 의해 즉시 만족)

## 조건부 GET

![조건부 GET](https://user-images.githubusercontent.com/59442344/111758751-2fbe8900-88e0-11eb-874a-36225969e959.png)

  - Cache에 있는 정보가 update된 정보가 아니라면, origin server의 객체를 받아 client에게 전달하도록 한다.
  - 어쨌든 origin server를 들리는 것이면 cache가 의미 없는 것 아닌가?
    - update된 정보인지를 확인하기 위한 전달은 매우 작은 정보단위의 전달이기에, 과도한 traffic을 일으키지 않는다!

### 조건부 GET의 과정
  - **client -> web cahce 객체 요구** (이전에 전달된 적이 있는 특정 객체에 대한 정보일 경우..)
  - **web cache -> server 객체의 update 상황 확인** (If-Modified-Since : ???)
   - web cahce에 사본이 저장된 이후로 **update가 이루어지지 않았다. (304 Not Modified)**
     - server에서 **entity body가 비어있는 회신**이 돌아옴 
     - web cache에 있는 객체 사본을 client에게 전달
   - web cache에 사본이 저장된 이후로 **update가 이루어진적이 있다. (200 OK)**
     - server에서 **entity body에 최신의 정보를 포함하는 회신**이 돌아옴
     - web cache 객체를 최신 정보로 갱신
     - web cache를 거쳐서 client에게 객체 전달



