---
layout: post
image: /assets/img/network/HTTP.png
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
# accent_color: "#ccc"
# theme_color: "#ccc"
description: >
  인터넷 네트워크에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - web-basic
  - network
---
# Internet Network

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `Inflearn`에서 `HTTP 웹 기본 지식`을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간에는 `Internet Network`에 대해 알아보겠습니다.

---

## 인터넷 통신
웹, HTTP는 모두 인터넷을 기반으로 통신하기 때문에, 인터넷 통신을 기본적으로 이해하고 넘어가도록 하겠습니다.

> 인터넷에서 컴퓨터 둘은 어떻게 통신할까?

컴퓨터들은 인터넷을 통해 연결되어 있습니다. 그런데 데이터를 전송하려면 생각보다 고려해야 할 사항들이 많이 있는 것 같습니다. 어떤 것들이 있는지 하나씩 알아보도록 하겠습니다. 

<img src="/assets/img/network/internet-network/Internet-communication.png" width="650" height="350"  >

### IP (Internet Protocol)
먼저 처음으로 고려해야 할 사항은 **IP(Internet Protocol)**입니다. 만약, 한국에서 인터넷을 통해 미국으로 데이터를 전송해야 한다면 어떻게 해야할까요?? **IP**는 이런 상황에서 필요하게 됩니다.

<img src="/assets/img/network/internet-network/IP-address.png" width="650" height="350"  >

- **IP(Internet Protocol)**의 역할
  - 지정한 **IP 주소(IP Address)**에 데이터 전달
  - **패킷(Packet)**이라는 통신 단위로 데이터 전달

#### IP 패킷 정보
<br>
<img src="/assets/img/network/internet-network/IP-packet-information.png" width="650" height="350"  >

**IP 패킷**에는 개략적으로 다음과 같은 내용들이 입력됩니다.
- **출발지 IP**
- **목적지 IP**
- **기타...**

##### 클라이언트 패킷 전달
<br>
<img src="/assets/img/network/internet-network/client-packet-transfer.png" width="650" height="350"  >
클라이언트에서는 위의 패킷 내용들을 가지고 **IP** 패킷을 인터넷 망에 전달합니다. 그러면 인터넷 프로토콜 규약을 따르고 있는 노드들이, 패킷을 전달하게 되고 최종적으로 **서버로 패킷을 전달**하게 됩니다.

##### 서버 패킷 전달
<img src="/assets/img/network/internet-network/server-packet-transfer.png" width="650" height="350"  >
서버는 전달받은 패킷에 대해 응답을 하고 해당 내용을 포함하여 출발, 목적지의 IP주소를 가지고 인터넷 망에 패킷을 전달합니다. 동일하게 노드들이 패킷을 전달하게 되고 최종적으로 **클라이언트로 패킷을 전달**하게 됩니다.

#### IP의 한계
- **비연결성**
  - 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷이 전송된다
- **비신뢰성**
  - 중간에 패킷이 사라지면?
  - 패킷이 순서대로 오지 않으면?
- **프로그램 구분**
  - **같은 IP**를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이라면??

##### 비연결성
<img src="/assets/img/network/internet-network/IP-non-connected.png" width="650" height="350"  >
 
위의 그림을 살펴보면, **IP:200.200.200.2**는 인터넷에 연결되어있지 않음에도 불구하고 패킷은 전송되게 됩니다.

##### 패킷 소실
<img src="/assets/img/network/internet-network/IP-packet-lost.png" width="650" height="350"  >

만약 케이블이 끊어져있다면 패킷이 전달되지 않습니다.

##### 패킷 전달 순서 문제
<img src="/assets/img/network/internet-network/IP-packet-order.png" width="650" height="350"  >

패킷은 보통 **1500바이트**가 넘으면 한번에 많은 것을 보내기에는 부담스럽기 때문에 끊어서 보내게 됩니다. 그러다보면 서버에 도착할 때 패킷의 순서가 바뀌게 될 수 있습니다.

<br>

이러한 문제점들 때문에, **TCP**라는 개념이 소개되었습니다.

### TCP, UDP
앞서 말씀드렸던, IP의 단점을 TCP가 보완해줍니다. (UDP는 IP의 단점을 보완하지는 않습니다만 뒤에서 간략히 설명하겠습니다.)

<img src="/assets/img/network/internet-network/ip-stack-order.png" width="650" height="350"  >

기본적인 인터넷 프로토콜 스택은 다음과 같은 4계층으로 이루어져 있습니다.
1. 애플리케이션 계층 - **HTTP**, **FTP**
2. 전송 계층 - **TCP**, **UDP**
3. 인터넷 계층 - **IP**
4. 네트워크 인터페이스 계층 

---

<img src="/assets/img/network/internet-network/protocol-layers.png" width="650" height="350"  >

예를 들어, 채팅 프로그램으로 미국에 사는 친구에게 메시지를 전송한다고 가정해보겠습니다. 보통 **SOCKET 라이브러리**를 사용하게 되는데, **OS**에게 메시지를 전달하게 됩니다.<br>

그렇게 되면 **OS**는 **TCP** 정보를 생성하여 메시지에 씌워주고, 그 후에 **IP** 패킷을 생성하여 메시지에 씌워줍니다. 마지막으로 네트워크 인터페이스 계층으로 전달되면 **Ethernet Frame**도 씌워진 채 인터넷으로 전달되는 것이지요. (웹 개발을 공부하는 데에는 **Ethernet Frame** 내용까지 알 필요는 없습니다만 필요하면 배우는 것을 추천합니다.)

#### TCP/IP 패킷 정보
<img src="/assets/img/network/internet-network/tcp-ip-packet-information.png" width="650" height="350"  >
`패킷(packet) = 패키지(package) + 버킷(bucket)`의 합성어라고 이해하시면 됩니다. **TCP** 정보에는
  - 출발지 PORT
  - 목적지 PORT
  - 전송 제어
  - 순서
  - 검증 정보
등과 같은 내용들이 들어가게 됩니다. 즉 **IP**만으로 해결되지 않았던 순서 제어와 같은 부분들이 해결되게 되는 것입니다.

##### TCP 특징
**TCP(Transmission Control Protocol, 전송제어 프로토콜)**는 다음과 같은 특징을 갖게 됩니다.

- **연결 지향 => TCP 3 way handshake (가상 연결)**
- **데이터 전달 보증**
- **순서 보장**
- **신뢰할 수 있는 프로토콜**
- **현재 대부분 애플리케이션에서는 TCP를 사용**

이러한 특징들은 **TCP** 정보에 전송 제어, 순서, 검증정보 등과 같은 내용들이 들어있기 때문입니다.

---

###### TCP 3 way handshake
<img src="/assets/img/network/internet-network/tcp-3-way-handshake.png" width="650" height="350"  >

<br>
**TCP/IP**로 연결을 하게 되면 위의 그림과 같이 됩니다.
1. 클라이언트에서 **SYN(접속 요청)** 이란 메시지를 보냅니다.
2. 클라이언트에 응답을 하기 위해 서버는 **SYN**+**ACK(요청 수락)**를 보냅니다.
3. 서버의 **SYN**을 받은 클라이언트는 **ACK**를 서버로 보내줍니다.
<br>

중요한 점이 있습니다. **TCP 3 way handshake**라는 것은 **실제로 연결이 이루어진 것이 아닌, 개념적으로 연결이 이루어졌다**는 것을 의미합니다. 클라이언트에서 서버로 연결될 때 까지 중간의 수많은 서버(노드)들을 지나게 될 것 인데 **TCP 3 way handshake**에서는 그 노드들에 대해 알 수 없습니다.

###### 데이터 전달 보증
<img src="/assets/img/network/internet-network/data-transmission-confirm.png" width="650" height="350"  ><br>

데이터를 전송하게 되면 서버에서 데이터를 잘 받았다는 메시지를 전송해줍니다. 이를 통해 데이터가 잘 전송되었다는 것을 확인할 수 있겠죠.


###### 순서 보장
<img src="/assets/img/network/internet-network/order-guarantee.png" width="650" height="350"  ><br>

보낸 패킷들이 중간에 순서가 잘못되면 올바른 순서로 패킷을 보내도록 다시 요청합니다.

##### UDP 특징
**UDP(User Datagram Protocol, 사용자 데이터그램 프로토콜)**는 기능이 거의 없습니다. 일반적으로 하얀 도화지에 비유합니다. 일반적으로 다음 특징들을 가집니다.

- 연결지향 => TCP 3 way handshake X
- 데이터 전달 보증 X
- 순서 보장 X
- 데이터 전달 및 순서가 보장되지 않지만 단순하고 빠름
<br>
- 정리
  - IP와 거의 같음 (+ PORT + 체크섬)
  - 애플리케이션에서 추가 작업이 필요  

<br>

**PORT**는 아래에서 계속 설명하겠지만, 하나의 **IP**에 접근하는 수 많은 패킷들을 받아서 **여러 애플리케이션이 동작할 수 있도록 해주는 연결 통로**라고 생각해주시면 되겠습니다.

---
**UDP**는 앞서 배운 **TCP**에 비해 기능이 없는데 왜 사용되는 것일까요? 이유는 **TCP**에는 이미 구현된 기능들이 많고 이미 인터넷에서는 이 **TCP**를 기본으로 사용하고 있어서 최적화를 하기가 힘들지만, **UDP**는 아무런 기능도 없기 때문에 **TCP**를 그대로 사용하면서, **사용자가 원하는대로 구축할 수 있도록 UDP를 사용**하면 되기 때문입니다. <br>

이러한 장점들 덕분에 **HTTP/3.0** 에서는 **UDP** 통신이 선택되었다고 할 수 있겠습니다. 

### PORT

### DNS