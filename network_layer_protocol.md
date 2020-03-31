## IP (Internet Protocol)

**네트워크 환경에서 컴퓨터간 통신하기 위해 각 컴퓨터에 부여된 네트워크 상 주소**

- **비연결성(Connectionless)**
- **신뢰성 없는(Unreliable)**

IP는 Destination Address를 기반으로 라우팅(알고리즘을 통해 최적의 경로를 경유하여 목적지를 찾아가는 것)을 수행한다.  

### IP 단편화 (Fragmentation)
IP 패킷 or 데이터그램은 MTU(Maximum Transfer Unit, TCP/IP 네트워크 등과 같은 패킷 또는 프레임 기반의 네트워크에서 전송될 수 있는 최대 크기의 패킷 또는 프레임)에 
따른 단편화가 발생한다. IP Packet의 크기가 2(Data Link) Layer의 MTU보다 크면 데이터를 분할해서 보낸 다음 목적지에서 재조립을 해야 한다.  

### IP 라우팅 (Routing)

<Routing Rule>

1. Destination Address가 자신과 동일한 네트워크에 있다면? **직접전송**
2. Destination Address가 자신과 동일한 네트워크에 속하지 않는다면? (직접전송이 불가하므로)**1차 경유지(Gateway) 주소를 라우팅 테이블을 참조하여 찾음**
3. Destination Address와 자신의 주소가 동일하면? **Destination이 자신의 호스트이므로 상위계층으로 데이터를 전달**한다

<Routing Table>

- 검색 방식: IP 패킷의 목적지 IP와 라우팅 테이블의 **`netmast/genmask`를 bit and(&) 연산**(=> Destination IP에서 Network IP만 추출하기 위해)하여 
  라우팅테이블의 `Destination` 필드와 비교해서 일치하는 경로를 선택
  
- 우선순위
  1. Destination Host IP와 일치하는 경로 찾는다
  2. Destination Host IP와 `genmask`를 bit and(&) 연산값(네트워크 주소)과 일치하는 경로를 찾는다
  3. 일치하는 경로가 없으면 default gateway(0,0,0,0)로 보낸다
  
- `netstat -rn` (Linux)
  - Destination: 목적지 호스트 또는 네트워크 주소
  - Gateway: 목적지로 전송하기 위한 게이트웨이 주소
  - Genmask: Host를 식별하기 위한 마스크(255.255.255.255), 목적지 Network를 식별하기 위한 마스크(넷마스크 의미), default gateway를 식별하기 위한 마스크(0.0.0.0)
  - Flag: U(route is Up) = 경로 활성화 되었음, G(use Gateway) = Gateway 사용, H(target is a Host) = 목적지가 호스트

### IP Spoofing
시스템 간 Trust 관계(시스템에 미리 'A를 신뢰한다'고 등록해놓으면 별도의 인증 없이 접속할 수 있음)를 이용한다. 
공격자는 시스템과 신뢰관계에 있는 클라이언트 A를 연결 불가능한 상태로 만들고 공격자는 A의 IP 주소로 위조하여 시스템에 접근한다.  
트러스트 설정은 다수 시스템의 관리자가 일일이 인증할 때 불편해서 편하자고 만들어놓은 설정인데, 보안에는 안좋다.

### IP 주소 클래스 (= Network Class)

현재 점진적으로 IPv6 환경으로 넘어가는 추세지만, 이미 IPv4환경으로 인프라가 구축되어 있다. 이 인프라를 최대한 활용하기 위해 **IPv4 체계의 IP 주소를 사용하는 네트워크의 규모에 따라 관리하기 쉽도록 클래스라는 이름으로 나누기 시작**했고, 이는 네트워크 클래스의 시작이 된다.

IP Address = Network Address + Host Address

네트워크 주소는 인터넷 상에서 네트워크를 구분하는 역할을 하며, 
호스트 주소는 특정 네트워크 안에 존재하는 기기들을 구분하는 역할을 한다.

현재는 A~E 다섯 클래스로 나눠서 사용하지 않는다. 위의 클래스로 네트워크를 구분하게 되면, 어떤 기업이 웹 서비스를 구현하는데 있어서 Class C 네트워크 보다는 더 많은 호스트 수가 필요하고 Class B 네트워크 보다는 더 적은 호스트 수가 필요한 상황에서 Class B 주소를 할당받는다면, 이는 IP주소의 낭비로 이어질 수 있다.

이를 보완하려 서브넷(Subnet)이라는 기술을 사용하면, 네트워크 영역을 클래스로 구분한 것 처럼 8비트, 16비트, 24비트로 구분하지 않고, 네트워크 주소를 17비트, 27비트로 적절한 양으로 구분할 수 있게 된다.

즉 개념적으로 사용하고는 있지만, 서브넷과 연계하여 사용하는 것이 일반적이다.

---

32자리 2진수로 구성된 IP 주소는 십진수로 표현할 수 있는데, 옥텟 당 `.` 를 찍어서 구분해야 한다. 
또한 한 네트워크 안에서 IP들의 네트워크 영역은 같아야하고, 호스트 IP는 서로 달라야 통신이 가능하다.  
ex) `203.240.100.1`이라는 IP는 C 클래스이기 때문에 `203.240.100`은 네트워크 주소고, `1`은 호스트 주소다.  
이렇듯 IP 주소에는 클래스라는 개념이 있고, 이 클래스의 개념을 알면 어디까지가 네트워크 영역이고, 어디까지가 호스트 IP 영역인지 알 수 있다. 
클래스틑 하나의 IP 주소에서 네트워크 영역과 호스트 영역을 나누는 방법이자, 약속이다.

  ![image](https://user-images.githubusercontent.com/37951612/78069276-c7c41980-73d4-11ea-994a-8b5d0db2c68d.png)

  ![image](https://user-images.githubusercontent.com/37951612/78069991-fe4e6400-73d5-11ea-9106-fc9e760a8acd.png)
  
  ![image](https://user-images.githubusercontent.com/37951612/78071201-0a3b2580-73d8-11ea-9c5a-799c72ee21d8.png)
  
#### A클래스
하나의 네트워크가 가질 수 있는 호스트의 수가 가장 많은 클래스다.  
IP 주소를 32자리 2진수로 표현했을 때 맨 앞자리 수가 항상 **0**인 경우가 A 클래스다.  
ex) `0xxx xxxx. xxxx xxxx. xxxx xxxx. xxxx xxxx`  

A 클래스에서 가질 수 있는 IP 범위는 `0.0.0.0 ~ 127.255.255.255`다.  
A 클래스에서는 첫번째 옥텟만 네트워크 부분을 나타내고, 나머지 부분은 호스트 부분을 나타낸다.  
네트워크 주소는 가장 작은 네트워크인 `1.0.0.0` 과 가장 큰 네트워크인 `126.0.0.0` 까지로 규정되어 있으며
(`0xxx xxxx x`가 가질수 있는 **경우의 수가 네트워크 범위**이며, 여기서 127은 제외됨) 참고로 네트워크에서 0은 호스트 부분이라는 뜻이다.

IP주소 중에서 **1부터 126**으로 시작하는 네트워크는 A클래스라고 생각하면 되고, 
호스트 주소가 가질 수 있는 갯수는 `(2^24) - 2`개(**모두**가 **1**인 경우 **브로드캐스트 주소**로 사용, **모두 0**인 경우 **네트워크 주소**로 사용)다.

#### B클래스
IP 주소를 32자리 2진수로 표현했을 때 맨 앞자리 수가 항상 **10**으로 시작한다.  
ex) `10xx xxxx. xxxx xxxx. xxxx xxxx. xxxx xxxx`  

B 클래스에서 가질 수 있는 IP 범위는 `128.0.0.0 ~ 191.255.255.255`다.  
네트워크 범위는 10xx xxxx. xxxx xxxx 에서 x들이 가질 수 있는 경우의 수(2^14 개)다.  
호스트 주소가 가질 수 있는 갯수는 `(2^14) - 2`개(모두가 1인 경우 브로드캐스트 주소로 사용, 모두 0인 경우 네트워크 주소로 사용)다.

#### C클래스  
  IP 주소를 32자리 2진수로 표현했을 때 맨 앞자리 수가 항상 **110**으로 시작한다.  
ex) `110x xxxx. xxxx xxxx. xxxx xxxx. xxxx xxxx`  

C 클래스에서 가질 수 있는 IP 범위는 `192.0.0.0 ~ 223.255.255.255`다.  
네트워크 범위는 110x xxxx. xxxx xxxx. xxxx xxxx 에서 x들이 가질 수 있는 경우의 수(2^21 개)다.  
호스트 주소가 가질 수 있는 갯수는 `(2^8) - 2`개(모두가 1인 경우 브로드캐스트 주소로 사용, 모두 0인 경우 네트워크 주소로 사용)다.

#### D, E 클래스  
  네트워크 주소, 호스트 주소가 나뉘지 않은 특수용도 IP이다. 멀티캐스트용, 연구용으로 사용된다.  
  D Class 네트워크 => 멀티캐스트(메시지를 한 번 송신해서 특정 네트워크 내의 두 개 이상에 기기에 전송하는 기술)를 위해 존재하는 네트워크  
  E Class 네트워크 => 미래에 사용될 예약된 주소를 구분해 놓은 네트워크  

#### 예약된 IP 주소 (특정한 기능을 수행한다)
- `127.0.0.1` - 루프백(Loopback) 주소, 자기자신을 가리키는 주소이다.
- `192.168.0.0` - 사설내트워크
- `224.0.0.0` - 멀티캐스트
- `240.0.0.0` - 미래 사용 용도로 예약

## ARP (Address Resolution Protocol)
네트워크 상에서 논리적 주소인 IP주소를 물리적 주소인 MAC주소로 대응(bind)시키기 위해 사용되는 프로토콜이다.  
브로드캐스트

### ARP Cache Table  

![image](https://user-images.githubusercontent.com/37951612/78061203-cd673280-73c7-11ea-86a3-2e8f97bf9fa2.png)

  ARP 요청을 통해 MAC Address를 담아두는 저장소인데, 영구적으로 저장된 것은 아니고 OS별로 좀 다르지만 1, 2분 정도 남아있는다고 한다. 
- `arp -a`: ARP Cache Table 정보를 볼 수 있다
(`static`은 관리자가 직접 입력한 것이며 시스템이 종료되고 재부팅될 때까지 남아있고, `dynamic`은 request, reply를 통해 동적으로 저장된 것을 말한다)
- `arp -d`: ARP Cache Table 모든 정보 삭제 (Windows)
- `arp -d <ip-address>`: IP 주소에 해당하는 정보만 삭제 (Linux)
- `arp -s <ip-address> <MAC-address>`: static 정보 저장 (Windows의 경우 MAC Address 작성 시 `:` 대신 `-` 사용)

### ARP Spoofing 대응
- ARP Cache를 static으로 설정 => ARP Reply로 갱신되지 않는다, 캐시정보는 재부팅하면 사라지므로 기동되면 다시 static으로 설정한다
- 네트워크 상의 ARP 트래픽을 실시간으로 모니터링하는 프로그램을 이용하여 IP Address, MAC Address 매핑을 감시하며 변경 발생 시 확인한다

### GARP (Gratutious ARP, 불필요한 ARP)
Source IP, Destination IP가 동일한 ARP 요청을 말한다. 네트워크에 연결되었을 때 
동일 네트워크에 있는 다른 노드들에게 '내가 들어왔다'고 알려주는 역할을 한다. 
그럼 다른 노드들은 ARP Cache 내의 MAC Address, IP Address를 갱신한다.  
충돌감지 기능이 있기 때문에 
만약 내 MAC Address, IP Address를 모든 노드에게 날렸을 때 내가 보낸 IP Address와 같은 주소를 쓰고 있는 노드가 있다면 
ARP Reply를 보낸다. 이건 **'누군가 내 IP와 동일한 IP를 사용해서 IP 충돌이 일어났다'** 를 의미한다.  
ARP는 인증을 하지 않으므로 GARP를 변조할 경우 동일 네트워크에 있는 모든 노드의 ARP Cache 정보를 변조할 수 있다(=> 보안 문제).


## RARP (Reverse Address Resolution Protocol)
ARP를 전환시킨 것으로 물리적 주소인 MAC주소를 논리적 주소인 IP주소로 대응(bind)시킨다. 지금은 거의 안쓴다고 한다.


## ICMP (Internet Control Message Protocol)
인터넷 제어 메시지 프로토콜. 오류 메시지에 주로 쓰인다.  
IP 프로토콜은 전송 상태 관리가 되지 않고 있는, 신뢰성 없는 프로토콜인데 이를 보완해주는 프로토콜이 ICMP다.

> ICMP 메세지들은 일반적으로 IP 동작에서 진단이나 제어로 사용되거나 오류에 대한 응답으로 만들어진다.  
ICMP 오류들은 원래 패킷의 소스 IP 주소로 보내지게 된다. 

### ICMP 용도
- 인터넷 or 통신 상에서 발생한 일반적인 상황 보고(**report**)
- 인터넷 or 통신 상에서 발생한 **오류**에 대한 보고
- **위험한 상황**에 대한 보고

### ICMP 기능
- IP 프로토콜을 이용하여 ICMP 메시지 전달
- Network Layer에 속하여 네트워크 관리 프로토콜의 역할을 수행하며, 종단간의 데이터 수송은 하지 않는다.

### ICMP 활용
- `ping`: 상대방 호스트의 작동 여부 및 응답시간 측정  
  아래의 메시지로 종단 노드 간에 네트워크 및 호스트 상태진단을 한다.
  - `Echo Request`: ICMP 질의메시지 요청
  - `Echo Reply`: ICMP 응답메시지 요청
- `tracer`: 목적지까지의 라우팅 경로 추적

### ICMP Packet Header 구조

![image](https://user-images.githubusercontent.com/37951612/78064879-62205f00-73cd-11ea-9a61-692e23aaa2ee.png)

- Type: ICMP 메시지 구별
- Code: 메시지 내용(=> ICMP Type)에 대한 추가 정보
- Checksum: ICMP 값 변조 여부
- Message(Data): Type에 따라 가변적인 내용
  - ICMP TYPE 3 (`DESTINATION UNREACHABLE`), ICMP TYPE 11 (`TIME EXCEEDED`) 등에서는 사용되지 않으므로 0이 채워짐
  - ICMP TYPE 8 (`ECHO REQUEST`), ICMP TYPE 0 (`ECHO REPLY`) 같은 메시지에서는 특정 값이 주어짐

![image](https://user-images.githubusercontent.com/37951612/78065115-c04d4200-73cd-11ea-9837-763c46cc454d.png)

## ARP Spoofing

![image](https://user-images.githubusercontent.com/37951612/78066226-81b88700-73cf-11ea-988c-186cbe99e2a1.png)

ARP 스푸핑은 각 시스템에 기록된 MAC 주소가 동적으로 유지되는 점을 이용한 공격이다. 따라서 MAC 주소를 정적으로 사용하면 이 공격이 불가능하다.

## Redirect 공격

### ARP Redirect 공격
대상자의 ARP Cache Table을 변조하는 것

### ICMP Redirect 공격
대상자의 Routing Table을 변조하는 것  
공격자가 Router / Gateway인 척을 하며 대상자를 속여서 Routing Table을 수정하고, 공격자의 주소로 패킷이 가도록 한다.  
ICMP Redirection 메시지는 Router / Gateway만 보낼 수 있기 때문에 공격자는 먼저 Spoofing으로 자신의 IP 주소를 게이트웨이 주소로 변조해야 한다.
