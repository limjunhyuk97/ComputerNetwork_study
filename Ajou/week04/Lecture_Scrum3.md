## 용어정리
  - **CDN** : 많은 사용자를 감당하기 위한 콘텐츠 분배 네트워크이다.
  - **DASH** : 가용대역폭의 차이 문제를 해결하기 위해 동영상을 다른 품질수준으로 인코딩하여 straming하는 기법이다.


## Video Streaming
![Video Streaming](https://user-images.githubusercontent.com/59442344/112808578-170a5c00-90b4-11eb-98b4-63cafffa80fb.png)

## Challenge
  - 엄청나게 많은 사용자들을 어떻게 감당할 것인가?
  - 사용자마다 다른 가용 대역폭의 문제를 어떻게 극복할 것인가?



# DASH
  - **Dynamic Adaptive Streaming over HTTP**
  - 사용자들의 가용 대역폭이 다른 것을 고려하여, 비디오를 서로 다른 버전으로 인코딩해두고, 제공하는 streaming 방법이다.
![dash](https://user-images.githubusercontent.com/59442344/112815038-de21b580-90ba-11eb-87e8-bfb865be9818.png)

## manifest file
  - 동영상들이 chunk로 나뉘어져 있고, chunk 단위들도 비트율에 따른 다른 버전으로 나뉘어져 있다.
  - 동영상을 제공할 때는, chunk단위의 다른 버전의 동영상들의 서버상 위치(URL)를 manifest file을 통해 제공한다.

## Server
  - 비디오를 chunk 단위로 나눈다.
  - 각 chunk는 다른 비트율, 품질로 encoding 된다.
  - **manifest file을 제공한다.**

## Client
  - 주기적으로 대역폭의 상태를 확인한다.
  - **대역폭 상태를 바탕으로 능동적을 파일을 요구**한다.
    - what encoding rate : 어떤 버전의 동영상을 요구할 지 결정
    - where to request chunk : 어디로 chunk를 요구할 지 (거리, 대역폭 등등.. 고려)
    - when to request chunk : 언제 chunk를 요구할 지 (buffer overflow 등등.. 고려)



# CDN
  - **Contents Nistribution Network**
  - 하나의 서비스에 대한 엄청난 수요를 극복하기 위하여 다수의 지점에서 분산된 server를 운영
  - 하나의 집중된 server에서 모든 서비스를 제공한다면, scale할 수 없는 문제가 발생(서비스 확장가능성 감소)

## CDN 기법

### 1. Enter deep
  - server를 최대한 사용자 가까이에 위치시킨다.
  - access network들에 cdn들을 깊숙이 배치시킨다.

### 2. bring Home
  - Enter deep 기법보다 덜 깊게 들어간다 : delay와 throughput이 상대적으로 나빠진다.
  - 적은 수의 server cluster를 핵심 지점에 위치시키는 방법이다.

## CDN vs Web Cache
  - 공통점 : client와 가까운 곳에서 contents를 제공하는 데 도움을 준다는 유사성이 있다.
  - 차이점 : 어디에 어떤 내용을 저장할 것인지에 대해 결정하는 주체가 다르다.

 |CDN|Web Cache|
 |:---:|:---:|
 |사본의 저장이 contents 제공자의 목적에서 기인한다.|사본의 저장이 사용자의 목적에서 기인한다.|

## CDN 동작
  - CDN을 구현하는 데에는 다양한 고려사항이 필요하다.
    - 지역 client들의 수요가 어떻게 되는가?
    - client가 network가 혼잡할 경우 어떻게 행동하는가?
    - client를 어떤 CDN node로 연결지어 주어야 하는가? ...

### 구체적인 동작

![CDN works](https://user-images.githubusercontent.com/59442344/112820838-feed0980-90c0-11eb-8cd9-56239927d1fa.png)

1. *service server*에 동영상 요구
2. *service server*에서 동영상이 위치한 *video가 위치한 server*의 url 제공
3. url에 대해서 local DNS에 질의
4. local DNS가 url 위치에 대한 질의를 *video가 위치한 server의 책임 DNS server*로 보냄
(**동영상이 위치한 server의 책임 DNS 서버가 CDN 서비스 네트워크로 우회시켜버림!**)
5. *video위치한 server의 책임 DNS server*가 *video가 위치한 server의 IP 주소* 대신, *CDN 서비스 server의 호스트명*을 알려줌
6. *CDN 서비스 server의 호스트명*에 대해서 local DNS에 질의
7. local DNS가 *CDN 서비스의 사설 DNS 구조*로 질의를 한 끝에 *CDN 서비스 server의 IP 주소*를 알아냄
8. local DNS가 이 IP 주소를 client에게 건네줌
9. client는 이 IP 주소와의 Dash 연결을 통해 비디오를 받게 된다.
