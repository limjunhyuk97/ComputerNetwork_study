## 용어정리
  - **user agent** : 사용자가 email을 전송, 저장, 관리 하도록 도와주는 프로그램이다.

# E-mail

## 4 major component
  - User agent
  - Mail server
  - Simple Mail Transfer Protocol (SMTP)
  - Mail access protocol (POP3, IMAP)

![email service](https://user-images.githubusercontent.com/59442344/112724245-c7ae1980-8f55-11eb-80de-26f63de95949.png)

## User Agent (e-mail reader)
  - e-mail에 접근하고 관리하는 프로그램으로, 메시지를 읽고, 저장하고, 전달하고, 구성하고, 보내게 해준다.
  - User agent 중에서는 gmail, hotmail, outlook과 같은 웹 기반의 이메일 프로그램이 일반적으로 사용된다.

### 1. User Agent -> Mail Server
  - SMTP 이용, 메시지 전달 가능
  - 전달된 메시지는 출력 Message queue로 이동

### 2. Mail Server -> User Agent
  - POP3, IMAP등을 이용, 서버상의 메시지를 읽고, 저장 가능.
  - 유저에게 온 message들을 위한 mail 공간인 Mail Box에 저장된 message들을 가져오는 것.

## Mail Server
<img src="https://user-images.githubusercontent.com/59442344/112740566-01fcd280-8fb9-11eb-81f6-ce4f13d12d1f.png" width=60% height=60%>

  - E-mail 구조의 중심이다.
  - User에게 도착하는 mesage들을 보관하는 Mail Box를 갖고 있다.
  - User들이 전송하는 message들을 임시적으로 대기시키는 Message queue를 갖고 있다.
  - SMTP를 사용하여 client역할을 하는 mail server가 msg를 보내고, server역할을 하는 mail server가 msg를 받는다. 

### 1. Mail Box
  - User에게 온 메일들을 관리하는 mail server 내의 공간이다.
  - User가 mail box에 온 msg를 보려면 Mail access protocol을 사용하여 계정과 비밀번호 인증과정을 거쳐야 한다.

### 2. Message Queue
  - User가 보내려 하는 msg들이 client역할의 mail server에서 임시로 대기하는 공간이다.
  - server역할의 mail server에 문제가 있다면 queue에 msg를 대기시키고, 문제 해결되지 않으면 반송한다.

## SMTP
  - **client, server 역할 모두를 수행**한다.
    - 메일을 보낼 때에는 SMTP의 client로 동작한다
    - 메일을 받을 때에는 SMTP의 server로 동작한다
  - **TCP를 이용하여 client-server 연결**을 짓고, **25번 포트를 이용**한다.
    - TCP 연결은 persistent connection을 이용한다.
  - **몸체가 ASCII로 이루어져야만 한다.**
  - **명령(command)은 ASCII text로 이루어져 있고, 답변(response)은 status code와 phrase로 받는다.**
    - 명령 : HELO, MAIL FROM, RCPT TO, DATA, QUIT
    - 답변 : 250 Sender OK, 250 Recipient OK, ,,,
    - CRLF. CRLF (.) : message 끝을 뜻한다.
<img src="https://user-images.githubusercontent.com/59442344/112798631-21bef400-90a8-11eb-9980-5dd09b4da175.png" width=70% height=70%>


### 1. SMTP transfer
  - server에서 목적지 server로 mail을 보낼때, server를 경유하지 않는다.

#### 1.0 user agent - mail server
  - 사용자가 user agent에서 작성한 mail을 mail server의 message queue로 보낸다.
#### 1.1 mail server - mail server (tcp connection 설정)
  - client가 server에게 persistent connection을 연결을 설정한다.
#### 1.2 handshaking
  - 연결이 설정되면, client와 server가 서로 자기 소개를 한다.
  - client가 server와, client의 주소를 제공한다.
#### 1.3 message transfer
  - persistent connection을 통해서 같은 TCP 연결 상에서 모두 보낸다.
  - 도착한 mail들은 mail server의 수신자를 위한 mail box 공간에 저장된다.
#### 1.4 closure
  - 모두 보냈다면 TCP에게 연결 닫을 것을 명령한다.
#### 1.5 mail server - user agent
  - mail server의 mail box에 있는 mail에 접근하기 위해 mail access protocol을 사용한다.
  
### 2. HTTP vs SMTP

#### 2.1 공통점
  - 파일을 하나의 host에서 다른 host로 보내기 위해서 사용한다.
  - 지속 HTTP와 SMTP 모두 persistent connection을 사용한다.
  - ASCII를 이용하여 command, request가 구성된다.

#### 2.2 차이점

 |SMTP|HTTP|
 |:---:|:---:|
 |원칙적으로 push protocol이다.(정보를 보내기 위한)|원칙적으로 pull protocol이다.(정보를 받기 위한)|
 |message로 전달되는 내용이 ASCII format 이어야 한다.|이러한 제약이 없다.|
 |ASCII가 아닌 문자나, 멀티미디어 파일에 대한 별도의 encoding이 필요하다.|이러한 제약이 없다.|
 |하나의 message, 여러개의 object 캡슐화 가능.|하나의 message, 하나의 object 캡슐화 가능.|

### 3. SMTP format
<img src="https://user-images.githubusercontent.com/59442344/112801243-9cd5d980-90ab-11eb-8bf8-f831c887d456.png">
  - 우리가 사용하는 실제 우편과 유사하게 생겼다.
  - envelope의 SMTP MAIL FROM, RCPT TO와 header의 From, To는 서로 다른 것이다.

### 4. MIME
  - SMTP가 ASCII 밖에 다루지 못하는 문제를 해결하기 위해 등장했다.
  - message header에서 ..
    - ASCII외의 다른 문자 집합들을 사용할 수 있게 해준다.
  - message body에서 ..
    - ASCII외의 다른 문자 집합들도 사용할 수 있게 해준다.
    - text format이 아닌 non-textual format 또한 첨부할 수 있게 해분다.
    - 여러 파트로 쪼개서 볼 수 있게 해준다

## Mail access Protocol

### 1. POP3
  - **download and keep** : 서로 다른 기기들에 message를 다운로드하고, server에 유지한다.
  - **download and delete** : 서로 다른 기기들에 message를 다운로드하고, server에서 지운다.
  - **원격 서버에 폴더 계층을 유지해 주지 않는다**. 원격 폴더 생성 및, 메시지 할당, 관리 등이 불가능하다.
  - 즉, **세션 사이의 상태정보가 전달되지 않는다**. **(stateless across sessions)**

![POP](https://user-images.githubusercontent.com/59442344/112804880-e1fc0a80-90af-11eb-93fe-a77cf7411bfe.png)

### 2. IMAP
  - **원격 서버에 폴더 계층을 유지**해 준다. 원격 폴더 생성 및, 메시지 할당, 관리 등이 가능하다.
  - **세션 사이의 상태정보가 전달** 된다 = 여러 다른 기기에서 사용 시에 상태정보들이 전달된다. **(keep state across sessions)**

![IMAP](https://user-images.githubusercontent.com/59442344/112804878-e1637400-90af-11eb-9bb0-d1a00b8e27ec.png)
