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
  - 명령을 ASCII text로 보내고, 답변은 status code와 phrase로 받는다.

### 1. SMTP transfer 3단계

### 2. HTTP vs SMTP

### 3. SMTP format

### 4. Mail Extensions

#### 4.1 MIME

## Mail access Protocol

### 1. POP3

### 2. IMAP
