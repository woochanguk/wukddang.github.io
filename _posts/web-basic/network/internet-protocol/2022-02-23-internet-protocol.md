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

<img src="/assets/img/network/Internet-communication.png" width="650" height="350"  >

### IP (Internet Protocol)
먼저 처음으로 고려해야 할 사항은 **IP(Internet Protocol)**입니다. 만약, 한국에서 인터넷을 통해 미국으로 데이터를 전송해야 한다면 어떻게 해야할까요?? **IP**는 이런 상황에서 필요하게 됩니다.

<img src="/assets/img/network/IP-address.png" width="650" height="350"  >

- **IP(Internet Protocol)**의 역할
  - 지정한 **IP 주소(IP Address)**에 데이터 전달
  - **패킷(Packet)**이라는 통신 단위로 데이터 전달

#### IP 패킷 정보
<br>
<img src="/assets/img/network/IP-packet-information.png" width="650" height="350"  >

**IP 패킷**에는 개략적으로 다음과 같은 내용들이 입력됩니다.
- **출발지 IP**
- **목적지 IP**
- **기타...**

##### 클라이언트 패킷 전달
<br>
<img src="/assets/img/network/client-packet-transfer.png" width="650" height="350"  >
클라이언트에서는 위의 패킷 내용들을 가지고 **IP** 패킷을 인터넷 망에 전달합니다. 그러면 인터넷 프로토콜 규약을 따르고 있는 노드들이, 패킷을 전달하게 되고 최종적으로 **서버로 패킷을 전달**하게 됩니다.

##### 서버 패킷 전달
<img src="/assets/img/network/server-packet-transfer.png" width="650" height="350"  >
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
<img src="/assets/img/network/IP-non-connected.png" width="650" height="350"  >
 
위의 그림을 살펴보면, **IP:200.200.200.2**는 인터넷에 연결되어있지 않음에도 불구하고 패킷은 전송되게 됩니다.

##### 패킷 소실
<img src="/assets/img/network/IP-packet-lost.png" width="650" height="350"  >

만약 케이블이 끊어져있다면 패킷이 전달되지 않습니다.

##### 패킷 전달 순서 문제
<img src="/assets/img/network/IP-packet-order.png" width="650" height="350"  >

패킷은 보통 **1500바이트**가 넘으면 한번에 많은 것을 보내기에는 부담스럽기 때문에 끊어서 보내게 됩니다. 그러다보면 서버에 도착할 때 패킷의 순서가 바뀌게 될 수 있습니다.

<br>

이러한 문제점들 때문에, **TCP**라는 개념이 소개되었습니다.

### TCP, UDP

### PORT

### DNS