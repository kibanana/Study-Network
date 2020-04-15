## Is a router a type of packet switch? [Link](https://www.quora.com/Is-a-router-a-type-of-packet-switch)
**라우터는 일종의 패킷 스위치인가?**

> Routers are advanced packet switches that work on the layer 3 of OSI Model  
라우터는 OSI 모델 3계층(Network Layer)에서 동작하는 고급 패킷 스위치다.

패킷 스위칭은 컴퓨터 네트워킹에 있는 개념인데, 데이터가 유선으로 얼마나 전송되느냐는 것이다.

라우터는 패킷 스위칭을 하는 서버다(여타 다른 컴퓨터들처럼).

스위치는 라우터 아래 계층에 있으며 특정 LAN 세그먼트에 속하는 패킷을 필터링한다.

노드 -> 스위치 -> 라우터 순서다.

## What is the difference between a host and end system?
**호스트와 엔드 시스템의 차이점은 무엇인가?**

#### 호스트와 엔드 시스템에는 큰 차이가 없다. 서로 대체 가능하며, 인터넷 시스템 분야에서는 모든 장치를 호스트 및 엔드 시스템이라고 한다. 엔드 시스템에는 이메일 서버, 워크스테이션, 웹서버, TV, 휴대폰, 태블릿 등이 있다. 호스트 시스템은 다른 시스템이나 사용자에게 서비스를 제공하는 모든 컴퓨터 네트워크다. 호스트 네트워크는 일반적으로 다중 사용자 운영체제를 실행한다.
Host is a computer conneced via Internet whereas End system is those type of computers connected computer network.

Example of end system are Pcs, Web Server etc.

Some end systems like Web server are not accessed directly by users.

#### 호스트와 엔드 시스템에는 차이가 없다. 인터넷에서는 모든 장치를 호스트 및 엔드 시스템이라고 하며, 따라며 서로 바꿔 사용할 수 있는 용어다. 엔드 시스템의 유형은 PC, 워크스테이션, 웹서버, 전자메일 서버, PDA, TV, 휴대폰, 태블릿 등이다.
I think there is no significant difference between host and end system, both can be interchangeable each other. In the field of Internet system all devices are called hosts and end systems.

The types of end systems are email servers, Workstations, Web servers, TVs, Cell Phones, Tablets etc.

Host system is any computer network which gives services to other systems or users. Host system usually runs a multi user operating system.


There is no difference between a host and an end system. In the internet, all devices are called hosts and end systems. So, hosts and end systems are used interchangeably. The types of end systems are PCs, Workstations, Web servers, email servers, PDAs, TVs, Cell Phones, Tablets, etc.

#### 기본적으로 컴퓨터 네트워크에 연결된 장치 또는 컴퓨팅 시스템을 엔드 시스템이라고 한다. 컴퓨터 네트워크의 가장자리에 위치하고 최종 사용자에 의해 운영되기 때문에 호출되는 입장이다. 일반적으로 통신 링크 및 패킷 스위치 네트워크로 연결된다. 요컨데 인터넷에서 호스트와 엔드 시스템은 서로 대체 가능한 용어로, 둘 간에 별 차이가 없다.

Basically the devices or the computing systems connected to the computer network are sometimes referred to as end systems. They are so called because they sit at the edge of the computer network and are operated by an end user.These are usually connected together by a network of communication links and packet switches.

Coming to the point, in the Internet, host and end systems are interchangeable terms which tells that there is no specific difference between them.


#### 호스트는 원격 위치에서 작업하는 사용자가 액세스하는 컴퓨터 시스템이며, 서버의 경우 엔드 시스템은 요청의 끝단이다.
Host is a computer system that is accessed by a user working at a remote location.

For ex - servers.

End system is the end point of your request.

For ex- when you access a web page than first request goes to the Name server and than it reads the zone file (SOA) and than it reads the A record and than “A” record request to web server for your web page. so the web server is the End point of your request.

#### 차이가 있다고 할 수도, 없다고 할 수도 있다. 일반적으로 다른 시스템을 제공하는 역할을 하는 시스템을 호스트라고 하는데, 최종 사용자 시스템은 네트워크를 통해 호스트의 데이터에 연결하고 액세스할 수 있다.

There may or may not have differences. Normally we refer a system as host when it is taking the role of serving other systems.

The end user systems can connect and access the data on the host via a network.


## Gateway와 Router의 차이점은 무엇인가?

> [link1](https://2cpu.co.kr/bbs/board.php?bo_table=network&wr_id=1853), [link2](https://www.netmanias.com/ko/post/qna/2279), [link3](https://lecnote.tistory.com/entry/%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9B%A8%EC%9D%B4%EC%99%80-%EB%9D%BC%EC%9A%B0%ED%84%B0%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)

#### 1

**매우 간단하게 말하면 라우팅을 해주는 게 게이트웨이다.**

게이트웨이와 라우터는 L3 장비들인데, Layer3은 IP와 연관되어있다. 
source, destination을 지정하는데 source가 되는 PC에도 routing table이 있다. 
여기에 목적지 정보가 없으면 무조건 게이트웨이로 패킷을 보낸다. 
즉, 어떤 호스트에게 게이트웨이라는 건 내 패킷을 무조건 라우팅시켜줄 통로인 것이다.

Gateway가 되는 장비 = L3 장비 = Router

- 공유기 : Router + DHCP + NAT 역할
- 방화벽 : Router + ACL + Firewall

> A traditional "gateway" can mean a device that (sometimes) is able to route traffic, but
whose primary goal is to *translate* from one protocol to another. For example- I would
use a "gateway" if I wanted to send packets from my IPv4 network to/from a IPv6 network,
or maybe an AppleTalk network to/from a Token Ring network.

> 전통적인 "게이트웨이"는 (때로는) 트래픽을 라우팅 할 수 있지만
주요 목표는 한 프로토콜에서 다른 프로토콜로 **번역**하는 것입니다. 예를 들어-
IPv4 네트워크에서 IPv6 네트워크로(에서) 패킷을 보내려면 "게이트웨이"를 사용하십시오.
또는 AppleTalk 네트워크에서 토큰 링 네트워크로(에서).

#### 2
Cisco(시스코)에서 얘기하는 **게이트웨이 모드**는 **공유기처럼 NAT가 활성화된 상태**를 말하고, 
**라우터 모드**는 **NAT가 비활성화된 상태**를 말한다.

사실 게이트웨이는 라우터와 구분되는 개념이 아니라, 
라우터 중에서 관문(Gateway) 역할을 하는 라우터를 게이트웨이 라우터(짧게 게이트웨이)라고 한다.

네트워크와 네트워크 사이를 이어주는 역할을 하는 것이 라우터이고, 
그 중에서도 가장 끝에 위치한 네트워크(ex. 가정, 회사)를 중앙(ex. 인터넷)으로 연결해주는 라우터를 특별히 게이트웨이라고 부른다.

방화벽 그 자체는 라우팅과 관련이 전혀 없고, 단지 조건에 따라 패킷을 걸러주는 역할을 한다. 방화벽 장비 중 라우팅 기능을 가진 장비가 있을 뿐이다.

---

일반적으로 라우터는 OSI 7 Layer 중 3계층에서 동작하며 Routing Protocol을 사용하여 IP 패킷이 
원하는 목적지까지 원활하게 갈 수 있도록 경로를 정하는 역할을 하는 장비를 지칭한다. 
라우터는 사용용도는 다른 네트워크(ex. IP의 Class가 다름, Subnetting이 다름)를 연결할 때 자주 사용된다. 

LAN과 WAN을 연결하는 등, 서로 다른 미디어 타입을 연결할 때 주로 사용되므로 항상 라우터는 Ethernet Interface와 WAN Interface를 기본으로 가지게 된다.

게이트웨이는 다른 네트워크로 가기 위한 관문이다.

3계층과 4계층의 스위치, 방화벽, Unix&Linux 시스템 등은 모두 부분적인 라우터 기능(Static Routing)을 가지고 있으므로 
모두 게이트웨이로서의 역할을 수행할 수 있다. 물론 라우터라고 불릴 수 있으려면 기본적인 Static Routing을 지원하는 것 뿐만 아니라 
Dynamic Routing(ex. RIP, OSPF, BGP)한 라우팅도 지원해야 한다.

만약 Static Routing이 있고 그 내용이 `ip route 10.10.100.0 255.255.255.0 10.10.10.1`라고 할 때, 
`10.10.100.0/24` 네트워크로 가기 위해 게이트웨이인 `10.10.10.1`이 nexhop이 된다는 의미다. 이런 Static Routing만 지원해도 게이트웨이는 될 수 있다.

요즘에는 라우터와 게이트웨이 기능이 장비 한대에서 구현되는 경우가 많아 같은 의미로 불리우고 있다.

---

> 네트워크 구성환경에 따라 라우터가 게이트웨이로 쓰일 수도 있고, 게이트웨이는 다른 장비나 소프트웨어가 될 수도 있다.



게이트웨이(Gateway): 컴퓨터 네트워크에서 서로 다른 통신망, 프로토콜을 사용하는 네트워크 간의 통신을 가능하게 하는 컴퓨터나 소프트웨어를 
두루 일컫는 용어다. 즉, 다른 네트워크로 들어가는 입구 역할을 하는 네트워크 포인트다. 
넓은 의미로는 종류가 다른 네트워크 간의 통로 역할을 하는 장치다.

라우터(Router, 경로기): 라우터 혹은 공유기는 패킷의 위치를 추출하여 최적의 경로를 지정하며 이 경로를 따라 데이터 패킷을 다음 장치로 전향시키는 장치다. 

## Host(호스트)

> 출처: 위키피디아

네트워크 호스트(Network Host)는 컴퓨터 네트워크에 연결된 컴퓨터나 기타 장치다. 
네트워크 호스트는 정보 리소스, 서비스, 애플리케이션을 네트워크 상의 사용자나 기타 노드에 제공할 수 있다. 
네트워크 호스트는 네트워크 주소가 할당된 네트워크 노드다.

### 서버와 호스트
모든 서버는 호스트지만, 모든 호스트가 서버인 것은 아니다. 
네트워크에 연결된 모든 장치는 호스트 자격이 있는 반면, 
다른 장치(클라이언트)로부터 연결을 수락하는 호스트만 서버가 될 수 있다.

### 개념의 기원
운영 체제에서 **터미널 호스트**라는 용어는 전통적으로 **컴퓨터 터미널**에 제공하는 
다중 사용자 컴퓨터나 소프트웨어, 또는 서비스를 소형이거나 기능이 떨어지는 장치에 제공하는 컴퓨터를 
가리킨다.

RFC 871 문서는 통신 네트워크에 연결되는 범용 목적의 컴퓨터 시스템으로서 호스트를 정의한다
(운영 체제에 참여하는 리소스를 달성하는 목적).

### Hostname(호스트명)

> 출처: 위키피디아

네트워크에 연결된 장치에게 부여되는 고유한 이름이다. 
특히 인터넷에서는 WWW(World Wide Web), 전자우편 등에서 호스트명을 흔히 사용하며, 
도메인 이름과 유사하지만 엄밀하게는 더 넓은 의미를 가지고 있다.

보통 호스트명은 사람이 읽고 이해할 수 있는 이름으로 지으며, 흔히 IP 주소나 MAC 주소와 같은 
기계적인 이름 대신 쓸 수 있다. 호스트명은 NIS, DNS, SMB 등 여러 체계에서 사용되기 때문에 
네트워크에 따라서 같은 컴퓨터에 배당된 호스트명이 달라질 수도 있다. 
하지만 대부분 호스트명은 인터넷 상의 호스트명을 가리키는 경우가 많다.

#### 인터넷 호스트명(Internet Hostname)
인터넷에 연결된 호스트(컴퓨터)의 이름으로, 보통 호스트의 지역 이름에 도메인명을 붙인 것이다. 
예를 들어 `ko.wikipedia.org`라는 호스트명에서 도메인 이름은 `wikipedia.org`이며, 
그 앞에 호스트의 지역명인 `ko`를 붙여서 호스트명을 만든다.

이렇게 만들어진 호스트명은 DNS를 통해 IP주소로 변환되거나, 
사용자의 컴퓨터에 있는 `hosts` 파일에서 IP 주소를 검색하여 사용하게 된다.

모든 호스트명은 DNS 상에서 사용할 수 있는 올바른 도메인명이다. 하지만 모든 도메인명이 호스트명은 아니다. 

예를 들어 올바른 호스트명인 `ko.wikipedia.org`와 `wikipedia.org`는 IP 주소가 할당된 올바른 호스트명이지만, 
`pmtpa.wikipedia.org`는 올바른 호스트명이 아니다. 그러나 이 도메인명 아래에 있는 `rr.pmtpa.wikipedia.org`는 
올바른 호스트명이다.


### Internet(인터넷)

> 출처: 위키피디아

**컴퓨터로 연결하여 `TCP/IP`라는 통신 프로토콜을 이용해 정보를 주고받는 컴퓨터 네트워크**다.

인터넷은 소규모 통신망을 상호 접속하는 형태에서 점차 발전하여 전 세계를 망라하는 거대한 통신망의 
집합체가 되었다.

인터넷에는 PC통신처럼 모든 서비스를 제공하는, 중심이 되는 호스트 컴퓨터(서버 컴퓨터)도 없고 
이를 관리하는 조직도 없다. 인터넷을 총괄적으로 관리하지는 않지만 인터넷 상의 
어떤 컴퓨터 또는 통신망에 이상이 발생하더라도 통신망 전체에는 영향을 주지 않도록 
실제 관리와 접속은 세계 각지에서 분산적으로 행해진다.

대중적인 월드 와이드 웹은 하이퍼텍스트 전송 프로토콜(HTTP)과 함께 사용되고, 
HTTP로 되어 있는 웹 페이지를 보기 위한 웹 브라우저를 이용한다.

인터넷은 표준 인터넷 프로토콜 집합(TCP/IP)을 사용해 전 세계 수십억 명의 사용자들에게 제공되는 지구 전체의 컴퓨터 네트워크 시스템이다. 
인터넷은 개인, 학교, 기업, 정부 네트워크 등을 한정적 지역에서 전체 영역으로 유선, 무선, 광케이블 기술 등을 통해 연결하여 구성한 네트워크들의 네트워크이다. 
인터넷은 하이퍼텍스트 마크업 언어(HTML)나 전자 우편을 지원하는 기반 기술 등을 통해 광대한 범위의 정보 자원과 서비스들을 운반한다.


### WWW(World Wide Web, 월드 와이드 웹)

인터넷에 연결된 컴퓨터를 통해 사람들이 정보를 공유할 수 있는 전세계적인 공간을 말한다. 
간단히 웹(Web)이라고 부르는 경우가 많다. 인터넷과 엄연히 다른 개념이다.

웹은 전자 메일과 같이 인터넷 상에서 동작하는 하나의 서비스일 뿐이나, 1993부터 
웹은 인터넷 구조의 절대적인 위치를 차지하고 있다.

인터넷에서 HTTP 프로토콜, Hypertext, HTML 형식을 사용하여 정보를 교환하는 전송방식을 말하기도 한다.

> Hypertext(하이퍼텍스트) : 참조(하이퍼링크)를 통해 독자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트다. 
주로 컴퓨터나 전자기기를 통해 표시된다. 인터넷과 결합하여 HTML의 주된 구성요소가 되었다. 
